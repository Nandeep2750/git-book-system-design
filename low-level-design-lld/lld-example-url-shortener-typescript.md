# LLD Example: URL Shortener (TypeScript)

Imagine you want to build a service like **bit.ly** or **tinyurl.com,** a platform that transforms long, unwieldy URLs into short, shareable links that are easy to remember and quick to type. Sounds simple, right? But behind this seemingly straightforward functionality lies a complex system that must be **fast**, **scalable**, **reliable**, and **secure**.

In this guide, we dive deep into the **low-level design (LLD) of a URL shortener, breaking down the system into concrete classes, methods, and data structures, all of which are** implemented in **TypeScript**. Whether preparing for a system design interview or building your URL shortening service, this example will give you a clear, step-by-step blueprint.

**By the end, you'll understand how to:**

* Design modular, maintainable components using TypeScript classes and interfaces.
* Implement efficient short code generation with collision handling.
* Use caching to speed up redirects and reduce database load.
* Handle edge cases and errors gracefully.
* Build a scalable backend ready for millions of users.

Let’s get started and turn the high-level idea into a developer-ready, production-quality design!

***

## 1. **Core Classes & Responsibilities** <a href="#undefined" id="undefined"></a>

| Class/Component       | Responsibility                                           |
| --------------------- | -------------------------------------------------------- |
| `UrlShortenerService` | Main service for shortening URLs and handling redirects  |
| `ShortCodeGenerator`  | Generates unique short codes (random/hash/counter-based) |
| `UrlRepository`       | Manages database CRUD for URL mappings                   |
| `CacheService`        | Handles caching of short code → long URL mappings        |
| `AnalyticsService`    | (Optional) Tracks click counts and analytics             |

## 2. **Class Diagram (Text Representation)** <a href="#undefined" id="undefined"></a>

```
+----------------------------------------------------------------------+
|                         UrlShortenerService                          |
+----------------------------------------------------------------------+
|                       - repo: UrlRepository                          |
|                       - cache: CacheService                          |
|                       - generator: ShortCodeGenerator                |
+----------------------------------------------------------------------+
| + shortenUrl(longUrl: string, customAlias?: string): Promise<string> |
| + getLongUrl(shortCode: string): Promise<string>                     |
+----------------------------------------------------------------------+
                                |
                                ▼
                    +--------------------------+
                    |   ShortCodeGenerator     |
                    +--------------------------+
                    | + generateCode(): string |
                    +--------------------------+
                                |
                                ▼
+-------------------------------------------------------------------+
|                        UrlRepository                              |
+-------------------------------------------------------------------+
| + saveUrl(shortCode: string, longUrl: string, ...): Promise<void> |
| + findByCode(shortCode: string): Promise<string | null>           |
| + findByUrl(longUrl: string): Promise<string | null>              |
| + incrementClickCount(shortCode: string): Promise<void>           |
+-------------------------------------------------------------------+
                                |
                                ▼
        +--------------------------------------------------+
        |      CacheService                                |
        +--------------------------------------------------+
        | + get(key: string): Promise<string | null>       |
        | + set(key: string, value: string): Promise<void> |
        +--------------------------------------------------+
```

## 3. **Database Schema** <a href="#undefined" id="undefined"></a>

**Table: url\_mapping**

| Column       | Type       | Description                                |
| ------------ | ---------- | ------------------------------------------ |
| short\_code  | VARCHAR(8) | Primary key (unique short code)            |
| long\_url    | TEXT       | Original long URL                          |
| created\_at  | TIMESTAMP  | Creation time                              |
| expiration   | TIMESTAMP  | Optional expiration time                   |
| user\_id     | UUID       | Optional owner of the short URL            |
| click\_count | INT        | Number of times the short URL was accessed |

## 4. **Class Implementations (TypeScript)** <a href="#undefined" id="undefined"></a>

### **ShortCodeGenerator**

```typescript
class ShortCodeGenerator {
  private readonly length: number;
  private readonly characters: string;

  constructor(length = 7) {
    this.length = length;
    this.characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  }

  generateCode(): string {
    let code = '';
    for (let i = 0; i < this.length; i++) {
      code += this.characters.charAt(Math.floor(Math.random() * this.characters.length));
    }
    return code;
  }
}
```

### **UrlRepository**

_(Assuming a generic async DB interface, e.g., using Prisma, Sequelize, or raw SQL)_

```
typescriptexport interface UrlMapping {
  shortCode: string;
  longUrl: string;
  createdAt: Date;
  expiration?: Date;
  userId?: string;
  clickCount: number;
}

export class UrlRepository {
  // Implement with your DB library
  async saveUrl(shortCode: string, longUrl: string, userId?: string, expiration?: Date): Promise<void> {
    // Save to DB
  }

  async findByCode(shortCode: string): Promise<UrlMapping | null> {
    // Query DB by shortCode
    return null;
  }

  async findByUrl(longUrl: string): Promise<UrlMapping | null> {
    // Query DB by longUrl
    return null;
  }

  async incrementClickCount(shortCode: string): Promise<void> {
    // Increment click count in DB
  }
}
```

### **CacheService**

_(Assuming Redis, but can be swapped for any cache implementation)_

```
typescriptimport { createClient, RedisClientType } from 'redis';

export class CacheService {
  private client: RedisClientType;

  constructor(redisUrl: string) {
    this.client = createClient({ url: redisUrl });
    this.client.connect();
  }

  async get(key: string): Promise<string | null> {
    return await this.client.get(key);
  }

  async set(key: string, value: string, ttlSeconds = 86400): Promise<void> {
    await this.client.set(key, value, { EX: ttlSeconds });
  }
}
```

### **UrlShortenerService**

```
typescriptimport { UrlRepository, UrlMapping } from './UrlRepository';
import { CacheService } from './CacheService';
import { ShortCodeGenerator } from './ShortCodeGenerator';

export class UrlShortenerService {
  constructor(
    private repo: UrlRepository,
    private cache: CacheService,
    private generator: ShortCodeGenerator
  ) {}

  async shortenUrl(longUrl: string, customAlias?: string): Promise<string> {
    // 1. Validate URL
    if (!this.isValidUrl(longUrl)) {
      throw new Error('Invalid URL');
    }

    // 2. Check for existing mapping
    const existing = await this.repo.findByUrl(longUrl);
    if (existing) {
      return this.buildShortUrl(existing.shortCode);
    }

    // 3. Generate short code (or use custom alias)
    let shortCode = customAlias || this.generator.generateCode();
    if (await this.repo.findByCode(shortCode)) {
      throw new Error('Short code already exists');
    }

    // 4. Save to DB and cache
    await this.repo.saveUrl(shortCode, longUrl);
    await this.cache.set(shortCode, longUrl);

    return this.buildShortUrl(shortCode);
  }

  async getLongUrl(shortCode: string): Promise<string> {
    // 1. Check cache
    let longUrl = await this.cache.get(shortCode);
    if (!longUrl) {
      // 2. Check DB
      const mapping = await this.repo.findByCode(shortCode);
      if (!mapping) {
        throw new Error('Short URL not found');
      }
      longUrl = mapping.longUrl;
      await this.cache.set(shortCode, longUrl);
    }
    // 3. Update analytics (optional)
    await this.repo.incrementClickCount(shortCode);
    return longUrl;
  }

  private buildShortUrl(shortCode: string): string {
    return `https://sho.rt/${shortCode}`;
  }

  private isValidUrl(url: string): boolean {
    try {
      new URL(url);
      return true;
    } catch {
      return false;
    }
  }
}
```

## 5. **API Endpoints (Sample)** <a href="#undefined" id="undefined"></a>

*   **POST `/shorten`**\
    Request:

    ```
    json{ "longUrl": "https://example.com/very/long/url", "customAlias": "myalias" }
    ```

    Response:

    ```
    json{ "shortUrl": "https://sho.rt/myalias" }
    ```
* **GET `/{shortCode}`**\
  Redirects to the original long URL.

## 6. **Error Handling** <a href="#undefined" id="undefined"></a>

| Error Type             | Scenario                                | Handling                                    |
| ---------------------- | --------------------------------------- | ------------------------------------------- |
| InvalidURLError        | User submits an invalid URL.            | Return HTTP 400 with "Invalid URL".         |
| ShortCodeConflictError | Custom alias already exists.            | Return HTTP 409 with "Alias already taken". |
| ShortCodeNotFoundError | Short URL does not exist or is expired. | Return HTTP 404 with "Short URL not found". |

## 7. **Summary Table** <a href="#undefined" id="undefined"></a>

| Component           | Technology/Pattern | Purpose                           |
| ------------------- | ------------------ | --------------------------------- |
| UrlShortenerService | Service Layer      | Handles main business logic       |
| ShortCodeGenerator  | Base62/Random/Hash | Generates unique short codes      |
| UrlRepository       | SQL/NoSQL DB       | Stores and retrieves URL mappings |
| CacheService        | Redis/Memcached    | Fast access to popular URLs       |
| AnalyticsService    | Custom/BigQuery    | (Optional) Track click counts     |

**This LLD in TypeScript provides a modular, developer-ready foundation for building a scalable URL shortener.**
