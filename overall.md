# Overall

### Overview

In this session, you’ll get an end-to-end guide to System Design, covering both High-Level Design (HLD) and Low-Level Design (LLD). We’ll begin by defining what system design is and why hands-on development experience is essential before diving into HLD—identifying both functional and non-functional requirements, and then learning all the core components you need to design large-scale services (e.g., Netflix). In the second half, we’ll switch to LLD: writing well-structured code, designing APIs, applying SOLID principles and design patterns, handling concurrency, and practicing common interview problems. Finally, we’ll talk about realistic timelines and the importance of continuous practice.

***

### 1. What Is System Design?

1. **Definition:**\
   Designing day-to-day tech systems—like Netflix, Uber, Instagram, WhatsApp—that millions use.
2. **Interview Context:**\
   In an SDE-2 or higher-level interview (or if you aim for a CTO/Tech Lead role), you’ll often be asked to “design WhatsApp” (for example).
3. **Two Parts of System Design:**
   * **High-Level Design (HLD):** Sketching out the overall architecture—services, data stores, caching layers, message queues—without writing actual code.
   * **Low-Level Design (LLD):** Digging into machine-level design—APIs, class diagrams, data models, and writing actual code snippets or pseudocode.

***

### 2. Prerequisites

* **Hands-On Development Experience:**\
  If you jump straight into system design without having built real projects (at least SD-1 level), many concepts will feel purely theoretical. Practice by building small end-to-end systems first.

***

### 3. High-Level Design (HLD)

#### 3.1 Functional vs. Non-Functional Requirements

* **Functional Requirements:**\
  Features users interact with (e.g., register/login, purchase subscription, play/pause video).
* **Non-Functional Requirements:**\
  System qualities (e.g., security, low latency, scalability).

#### 3.2 Core Steps in HLD

1. **Understand Fundamentals:**
   * Client-server vs. serverless architectures (e.g., AWS EC2 vs. Lambda) and the trade-offs.
   * Vertical vs. horizontal scaling (scale-up RAM/CPU vs. scale-out with multiple servers + load balancer).
   * How the Internet works: DNS, request–response cycle, threads/processes.
2. **Learn Databases:**
   * Types: SQL vs. NoSQL (MongoDB, Neo4j), in-memory stores.
   * Data sharding (horizontal partitioning), replication, migration.
   * Pros/cons of different models.
3. **Consistency & Availability:**
   * Consistency levels: eventual, causal, linearizable, etc.
   * The CAP theorem: making trade-offs during network partitions.
   * When to favor consistency (e.g., payment systems) vs. availability (e.g., messaging notifications).
4. **Caching:**
   * What is a cache, why use it (low-latency access to hot data).
   * Types: Redis, Memcached.
   * Write and eviction policies (write-back vs. write-through; LRU, LFU, segmented LRU).
   * CDNs for static content distribution.
5. **Networking:**
   * TCP vs. UDP; HTTP/1.x vs. HTTP/2 vs. HTTP/3.
   * WebSockets, WebRTC for real-time streaming use cases.
6. **Load Balancing:**
   * Algorithms: round-robin, least-connections, consistent hashing.
   * Stateless vs. stateful LB.
7. **Message Queues:**
   * Asynchronous processing (non-critical tasks): Kafka, RabbitMQ.
   * Publisher–subscriber models.
8. **Monoliths vs. Microservices:**
   * When to start as a monolith.
   * Why migrate to microservices: avoid single points of failure, enable team autonomy.
   * Containerization (Docker) and orchestration.
9. **Monitoring & Logging:**
   * Tools: AWS CloudWatch, Grafana, Prometheus.
   * Anomaly detection.
10. **Security:**
    * Authentication/authorization (OAuth, tokens, ACLs).
    * Encryption, rate limiting to prevent DDoS.
11. **Trade-Offs & Thought Process:**
    * No single “right” answer—always justify your choices (push vs. pull, SQL vs. NoSQL, consistency vs. availability).
12. **Practice:**
    * Design a variety of popular systems: YouTube, Twitter, WhatsApp, Amazon, Zoom, Instagram, Uber.
    * Each system highlights different patterns: e-commerce, video streaming, social feeds, ride-sharing.

***

### 4. Low-Level Design (LLD)

#### 4.1 Core Coding Principles

1. **SOLID Principles:**
   * Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion.
2. **Design Patterns:**
   * Creational (Factory, Singleton), Structural (Adapter, Decorator), Behavioral (Observer, Strategy).
3. **Concurrency & Thread Safety:**
   * Locks, producer-consumer, synchronization, race conditions.
4. **UML Diagrams (Optional):**
   * Class and component diagrams—ask your recruiter if they expect UML.
5. **API Design:**
   * Request/response modeling, versioning, extensibility.
   * Avoiding “God classes,” following clean code principles.
6. **Practice Common Problems:**
   * Small systems you can code in an interview: URL shortener, notification service, chess game logic, tic-tac-toe, etc.

***

### 5. Learning Timeline & Tips

* **For Active Engineers:** 2–3 months to cover everything if you’re already using many of these concepts day-to-day.
* **For Beginners:** 4–6 months if most concepts are new.
* **Key to Mastery:** Practice relentlessly—sketch designs on a whiteboard, implement mini-projects, review others’ system designs.

***

### 6. Takeaway

System design interviews are all about demonstrating your thought process, understanding trade-offs, and justifying your architectural decisions. Build the fundamentals first, then practice on a variety of real-world systems until you can confidently discuss both high-level components and low-level implementation details.

{% embed url="https://youtu.be/CuQmQpvw04I?si=s6tVTbNT_pZ3kuTl" %}
