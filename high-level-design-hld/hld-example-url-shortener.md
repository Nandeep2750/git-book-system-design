# HLD Example: URL Shortener

## 1. **Requirements** <a href="#undefined" id="undefined"></a>

**Functional:**

* Users can submit a long URL and receive a unique, shortened version.
* Anyone visiting the short URL is redirected to the original long URL.
* (Optional) Users can specify custom aliases.
* (Optional) Track analytics (clicks, locations, etc.).

**Non-Functional:**

* High availability and scalability (handle millions of URLs and redirects).
* Low latency for redirection.
* Prevent duplicate short URLs for the same long URL.
* Handle collisions in short code generation.
* Data consistency and durability.

## 2. **Core Components** <a href="#undefined" id="undefined"></a>

| Component              | Role                                                              |
| ---------------------- | ----------------------------------------------------------------- |
| Load Balancer          | Distributes incoming requests to multiple servers for scalability |
| Application Servers    | Handle API requests (shorten, redirect, analytics)                |
| URL Generation Service | Generates unique short codes (with collision handling)            |
| Database               | Stores mappings: short code ↔ long URL (+ metadata, expiration)   |
| Cache                  | Stores frequently accessed mappings for fast redirects            |
| Analytics Service      | (Optional) Tracks and reports usage statistics                    |

## 3. **System Architecture Diagram** <a href="#undefined" id="undefined"></a>

```
text[Client]
   |
[Load Balancer]
   |
[Application Servers]
   |         |         |
[Database] [Cache] [Analytics Service]
```

* All requests go through the Load Balancer.
* Application Servers interact with the Database, Cache, and Analytics Service.

## 4. **Data Flow** <a href="#undefined" id="undefined"></a>

### A. URL Shortening

1. User sends a request to shorten a long URL (`POST /shorten`).
2. Application Server validates the URL.
3. Checks if the URL already exists in the database (to avoid duplicates).
4. If not, URL Generation Service creates a unique short code (using hashing, counter, or random string).
   * Handles collisions if the code already exists.
5. Stores the mapping (short code ↔ long URL) in the database.
6. Returns the short URL to the user.

### B. URL Redirection

1. User visits the short URL (`GET /{short_code}`).
2. Application Server checks the Cache for the mapping.
   * If not found, queries the Database.
   * Updates Cache if needed.
3. Redirects the user to the original long URL.
4. (Optional) Sends event to Analytics Service.

## 5. **Key Design Decisions** <a href="#undefined" id="undefined"></a>

### **Short Code Generation**

* **Hashing:** Generate a hash (e.g., MD5/SHA) of the long URL, encode part of it as Base62 for a short string[1](https://blog.algomaster.io/p/design-a-url-shortener)[7](https://www.designgurus.io/blog/url-shortening).
* **Counter:** Use a global/incrementing counter, encoded in Base62.
* **Random String/UUID:** Generate a random or unique code, check for collisions.

**Collision Handling:**

* Re-hash with a salt, or append a suffix if a collision is detected.

### **Database Choice**

* **NoSQL (e.g., DynamoDB, Cassandra):**
  * Handles billions of key-value lookups, with high scalability and availability.
* **Schema Example:**
  * Table: `short_code`, `long_url`, `created_at`, `expiration`, `user_id` (optional).

### **Cache**

* Use Redis or Memcached for fast lookup of popular short codes.
* Apply LRU or LFU eviction policy.

### **Load Balancer**

* Distributes traffic, ensures high availability.

### **Analytics (Optional)**

* Track clicks, referrers, geolocation, etc.[5](https://www.hellointerview.com/learn/system-design/problem-breakdowns/bitly).

## 6. **API Endpoints** <a href="#undefined" id="undefined"></a>

* **POST /shorten**\
  Request: `{ "long_url": "https://example.com/very/long/url" }`\
  Response: `{ "short_url": "https://sho.rt/abc123" }`
* **GET /{short\_code}**\
  Redirects to the original long URL.
* **(Optional) GET /analytics/{short\_code}**\
  Returns analytics data for the short URL.

## 7. **Scaling and Reliability** <a href="#undefined" id="undefined"></a>

* **Horizontal scaling:** Add more application servers behind the load balancer.
* **Replication:** Database and cache replication for high availability.
* **Partitioning/Sharding:** Distribute data across multiple database nodes for scalability.
* **Backup & Recovery:** Regular database backups for durability.

## 8. **Security and Best Practices** <a href="#undefined" id="undefined"></a>

* **Rate limiting:** Prevent abuse of the shortening service.
* **Validation:** Ensure submitted URLs are valid and safe.
* **HTTPS:** Secure all endpoints.
* **Monitoring & Logging:** Track errors, usage, and system health.

## 9. **Summary Table** <a href="#undefined" id="undefined"></a>

| Component              | Technology Example  | Purpose                   |
| ---------------------- | ------------------- | ------------------------- |
| Load Balancer          | AWS ELB, Nginx      | Distribute traffic        |
| Application Server     | Node.js, Python     | Handle API logic          |
| URL Generation Service | Custom logic        | Create unique short codes |
| Database               | DynamoDB, Cassandra | Store mappings            |
| Cache                  | Redis, Memcached    | Fast lookup for redirects |
| Analytics Service      | Custom, BigQuery    | Track and analyze usage   |

**This HLD ensures the URL shortener is scalable, reliable, and fast, ready to handle millions of requests efficiently.**
