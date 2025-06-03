# High-Level Design (HLD): Step-by-Step Guide

## Step 1: Fundamentals <a href="#step-1-fundamentals" id="step-1-fundamentals"></a>

**Key Concepts:**

* [**Serverless vs. Serverful:**](../concepts/serverless-vs.-serverful.md)
  * **Serverless:** Cloud provider manages servers, scaling, and maintenance. Pay only for what you use. Good for unpredictable workloads and rapid scaling.
  * **Serverful:** You manage and customise your servers. Offers more control and can be optimised for performance, but requires expertise and is harder to scale.
* [**Horizontal vs. Vertical Scaling:**](../concepts/horizontal-vs.-vertical-scaling.md)
  * **Horizontal:** Add more machines (scale out).
  * **Vertical:** Add more resources (CPU/RAM) to a single machine (scale up).
* **Threads and Processes:**
  * **Threads:** Lightweight units of execution within a process, enabling multitasking.
  * **Processes:** Independent programs with their own memory space.
* **Pages:**
  * Memory management units in operating systems are part of virtual memory.
* **How the Internet Works:**
  * DNS, HTTP(S), routing, request-response cycle.

**What does it matter?**\
Understanding these basics is crucial for selecting the right architecture, ensuring scalability, and developing systems that are both reliable and efficient.

**What you could add:**

* Clarify _process vs. thread_ for concurrency.
* Basic OS concepts: sockets, file descriptors, which are crucial for backend design.

## Step 2: Databases <a href="#step-2-databases" id="step-2-databases"></a>

**Key Concepts:**

* **SQL vs. NoSQL:**
  * SQL: Structured, relational (e.g., MySQL).
  * NoSQL: Flexible, document, key-value, or graph (e.g., MongoDB).
* **In-memory DBs:**
  * Fast, temporary storage (e.g., Redis, Memcached).
* **Replication & Migration:**
  * Making copies for reliability, moving data.
* **Partitioning & Sharding:**
  * Splitting data for scalability.

**What does it matter?**\
The right database choice affects scalability, flexibility, and performance.

**What you could add:**

* _Indexing_: Speeds up queries.
* _ACID vs. BASE_: Transaction guarantees.
* _Backup & Recovery_: Essential for data durability.

## Step 3: Consistency vs. Availability <a href="#step-3-consistency-vs-availability" id="step-3-consistency-vs-availability"></a>

**Key Concepts:**

* **Data Consistency Levels:**
  * Strong, eventual, causal, etc.
* **Isolation Levels:**
  * How transactions interact (read uncommitted, committed, repeatable read, serializable).
* **CAP Theorem:**
  * Trade-off between Consistency, Availability, and Partition Tolerance.
* **Durability:**
  * Data survives failures.

**What does it matter?**\
Balancing consistency and availability is crucial for user experience and data integrity.

**What you could add:**

* _Quorum-based systems_: How distributed systems achieve consensus.

## Step 4: Cache <a href="#step-4-cache" id="step-4-cache"></a>

**Key Concepts:**

* **What is Cache?**
  * Fast, temporary storage for frequently accessed data.
* **Write Policies:**
  * Write-back, write-through, write-around.
* **Replacement Policies:**
  * LFU, LRU, Segmented LRU.
* **CDNs:**
  * Distribute static content closer to users.

**What does it matter?**\
Caching improves speed, reduces database load, and enhances user experience.

**What you could add:**

* _Cache Invalidation_: Keeping cache and source data in sync.
* _Cache Warmup_: Preloading the cache for performance.

## Step 5: Networking <a href="#step-5-networking" id="step-5-networking"></a>

**Key Concepts:**

* **TCP vs. UDP:**
  * TCP: Reliable, ordered.
  * UDP: Fast, no guarantee.
* **HTTP/1, 2, 3 & HTTPS:**
  * Web communication protocols.
* **WebSockets, WebRTC:**
  * Real-time, persistent connections.

**What does it matter?**\
Choosing the right protocol impacts performance, reliability, and real-time capabilities.

**What you could add:**

* _REST vs. gRPC_: Modern API communication styles.
* _Network Latency & Bandwidth_: Impact on system performance.

## Step 6: Load Balancers <a href="#step-6-load-balancers" id="step-6-load-balancers"></a>

**Key Concepts:**

* **Load Balancing Algorithms:**
  * Stateless (round robin), stateful (session-aware).
* **Consistent Hashing:**
  * Evenly distributes load, useful for caching/sharding.
* **Proxy & Reverse Proxy:**
  * Improves security and scalability.
* **Rate Limiting:**
  * Prevents abuse.

**What does it matter?**\
Load balancing ensures high availability and reliability, preventing server overload.

**What you could add:**

* _Health Checks_: Detect failed servers.
* _SSL Termination_: Offload HTTPS to the load balancer.

## Step 7: Message Queues <a href="#step-7-message-queues" id="step-7-message-queues"></a>

**Key Concepts:**

* **Asynchronous Processing:**
  * Decouple tasks (Kafka, RabbitMQ).
* **Publisher-Subscriber Model:**
  * Multiple consumers for the same message/event.

**What does it matter?**\
Message queues help handle spikes in traffic and enable scalable, decoupled architectures.

**What you could add:**

* _Dead Letter Queues_: Handling failed messages.
* _At-least-once vs. Exactly-once Delivery_: Message delivery guarantees.

## Step 8: Monoliths vs. Microservices <a href="#step-8-monoliths-vs-microservices" id="step-8-monoliths-vs-microservices"></a>

**Key Concepts:**

* **Why Microservices:**
  * Scalability, independent deployment, and team autonomy.
* **Single Point of Failure:**
  * Avoiding system-wide outages.
* **Avoiding Cascading Failures:**
  * Prevent one failure from taking down others.
* **Containerization (Docker):**
  * Isolate services for deployment.
* **Migrating to Microservices:**
  * When and how to split a monolith.

**What does it matter?**\
Choosing the right architecture affects scalability, maintainability, and team productivity.

**What you could add:**

* _Service Discovery_: How services find each other.
* _API Gateway_: Entry point for microservices.

## Step 9: Monitoring & Logging <a href="#step-9-monitoring--logging" id="step-9-monitoring--logging"></a>

**Key Concepts:**

* **Logging Events & Monitoring Metrics:**
  * Track system health and user actions.
* **Anomaly Detection:**
  * Spot unusual patterns or failures early.

**What does it matter?**\
Monitoring and logging help detect, diagnose, and resolve issues quickly, ensuring system reliability.

**What you could add:**

* _Alerting_: Automated notifications for failures.
* _Tracing_: Following a request across services (distributed tracing).

## Step 10: Security <a href="#step-10-security" id="step-10-security"></a>

**Key Concepts:**

* **Tokens for Auth:**
  * Use tokens (JWT) to verify users’ identities.
* **SSO & OAuth:**
  * Single Sign-On, secure authorisation.
* **Access Control Lists (ACLs) & Rule Engines:**
  * Define who can access what.
* **Encryption:**
  * Protect data at rest and in transit.

**What does it matter?**\
Security protects user data, builds trust, and is often required by law or regulations.

**What you could add:**

* Security audits, penetration testing, and compliance standards.

## Step 11: System Design Tradeoffs <a href="#step-11-system-design-tradeoffs" id="step-11-system-design-tradeoffs"></a>

**Key Concepts:**

* **Push vs. Pull Architecture:**
  * The system sends updates automatically (push) or waits for requests (pull).
* **Consistency vs. Availability:**
  * Up-to-date data or always accessible, especially during network issues.
* **SQL vs. NoSQL Databases:**
  * Structured vs. flexible, scalable data.
* **Memory vs. Latency:**
  * More memory for lower latency.
* **Throughput vs. Latency:**
  * More requests per second vs. faster response.
* **Accuracy vs. Latency:**
  * Instant results vs. more accurate data.

**What does it matter?**\
Understanding tradeoffs helps you justify your design decisions and tailor systems to real-world needs.

**What you could add:**

* Simplicity vs. Flexibility, Cost vs. Performance, Monolith vs. Microservices[7](https://www.designgurus.io/answers/detail/understanding-design-trade-offs-in-system-design-interviews).

## **Step 12: Practice, Practice, Practice** <a href="#undefined" id="undefined"></a>

The best way to master system design is through regular practice. Try designing the architecture for popular real-world systems, one teaches you will learn different patterns and challenges.

**Practice designing systems like:**

1. YouTube (video streaming)
2. Twitter (social feed)
3. WhatsApp (real-time messaging)
4. Uber (ride-sharing)
5. Amazon (e-commerce)
6. Dropbox/Google Drive (file storage)
7. Netflix (high-scale streaming)
8. Instagram (photo sharing)
9. Zoom (video conferencing)
10. Booking.com/Airbnb (travel & booking platforms)

**What does it matter:**\
Each example exposes you to different requirements, tradeoffs, and best practices. The more you practice, the more confident and skilled you’ll become in system design interviews and real-world projects.

**What you could add:**

* Participate in system design discussions, code reviews, and mock interviews.

## **Summary Table**

<table><thead><tr><th width="70">Step</th><th width="238">Topic</th><th>Key Points/Examples</th></tr></thead><tbody><tr><td>1</td><td>Fundamentals</td><td>Server types, scaling, threads, internet basics</td></tr><tr><td>2</td><td>Databases</td><td>SQL/NoSQL, sharding, replication, backup</td></tr><tr><td>3</td><td>Consistency &#x26; Availability</td><td>CAP theorem, durability, quorum</td></tr><tr><td>4</td><td>Caching</td><td>Redis, policies, CDN, invalidation</td></tr><tr><td>5</td><td>Networking</td><td>TCP/UDP, HTTP, WebSockets, API styles</td></tr><tr><td>6</td><td>Load Balancing</td><td>Algorithms, health checks, SSL termination</td></tr><tr><td>7</td><td>Message Queues</td><td>Kafka, RabbitMQ, pub-sub, delivery guarantees</td></tr><tr><td>8</td><td>Monoliths/Microservices</td><td>Architecture, containers, API gateway</td></tr><tr><td>9</td><td>Monitoring &#x26; Logging</td><td>Logs, metrics, alerting, tracing</td></tr><tr><td>10</td><td>Security</td><td>Auth, encryption, ACLs, rate limiting</td></tr><tr><td>11</td><td>Tradeoffs</td><td>Consistency vs. availability, push vs. pull</td></tr><tr><td>12</td><td>Practice</td><td>Design real systems, learn patterns</td></tr></tbody></table>

> **With these 12 steps, you have a complete roadmap for approaching High-Level Design in any system. Mastering each step will help you design robust, scalable, and reliable systems-whether for interviews or real-world projects!**
