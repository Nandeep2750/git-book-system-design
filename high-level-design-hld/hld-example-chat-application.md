# HLD Example: Chat Application

Ever wondered how apps like WhatsApp, Slack, or Messenger deliver real-time chat to millions of users across the globe? Behind the scenes, a chat application requires a robust, scalable, and low-latency system design that can handle everything from instant messaging and media sharing to presence detection and notifications.

In this section, we’ll walk through the **High-Level Design (HLD)** for a modern chat application. You’ll learn how to break down requirements, design the core components, and make key architectural decisions that power real-time messaging at scale.

***

## 1. **Requirements** <a href="#undefined" id="undefined"></a>

**Functional:**

* Real-time 1:1 and group messaging.
* Show online/offline and last seen status.
* Message delivery/read receipts (sent, delivered, read).
* Support for media sharing (images, videos, audio).
* Push notifications for new messages (when offline).
* Store and retrieve chat history.
* User authentication and profile management.

**Non-Functional:**

* Scalability: Handle millions of users and messages.
* High availability and reliability.
* Low latency for real-time messaging.
* Data durability and consistency.
* Security (encryption, access control).

## 2. **Core Components** <a href="#undefined" id="undefined"></a>

| Component               | Role                                                                    |
| ----------------------- | ----------------------------------------------------------------------- |
| Load Balancer           | Distributes incoming connections to chat servers                        |
| Chat Servers            | Manage real-time message delivery, connections, and presence            |
| Application/API Servers | Handle authentication, user profiles, media uploads, and business logic |
| WebSocket Gateway       | Manages persistent, bidirectional connections for real-time messaging   |
| Message Queue           | Buffers messages for delivery and decouples message processing          |
| Database                | Stores users, messages, group info, metadata                            |
| Cache                   | Stores frequently accessed data (e.g., recent messages, user status)    |
| Notification Service    | Sends push notifications to offline/mobile users                        |
| Media Storage           | Stores and serves images, videos, and files                             |
| Monitoring & Logging    | Tracks system health, logs events, and detects anomalies                |

## 3. **System Architecture Diagram** <a href="#undefined" id="undefined"></a>

```
text[Client App/Web]
     |
[Load Balancer]
     |
[WebSocket Gateway] <------> [Chat Servers] <-----> [Message Queue]
     |                          |                      |
[API Servers]               [Database]             [Cache]
     |                          |
[Media Storage]          [Notification Service]
```

* Clients connect via WebSocket Gateway for real-time chat.
* API Servers handle authentication, user management, and media.
* Chat Servers process messages and manage user presence.
* Message Queue ensures reliable, decoupled message delivery.
* Database stores persistent data; Cache accelerates hot data access.
* Notification Service alerts users about new messages when offline.

## 4. **Data Flow** <a href="#undefined" id="undefined"></a>

### A. Sending a Message

1. A user sends a message via WebSocket to the Chat Server.
2. Chat Server authenticates and validates the message.
3. The message is placed in the Message Queue.
4. Chat Server retrieves a message from the queue and delivers it to the recipient’s active connection (if online).
5. If the recipient is offline, the message is stored in the Database and a push notification is sent.
6. Message status (sent, delivered, read) is updated and synced.

### B. Receiving a Message

1. The recipient’s client receives the message in real-time via WebSocket.
2. If offline, the message is delivered when the client reconnects.
3. Read receipts are sent back to the server and updated for the sender.

### C. Media Sharing

1. Media is uploaded to Media Storage via API Server.
2. A message with the media URL is sent through the chat flow.

## 5. **Key Design Decisions** <a href="#undefined" id="undefined"></a>

* **WebSockets for Real-Time:**\
  Persistent, bidirectional connections for instant message delivery.
* **Sticky Sessions or Service Discovery:**\
  Ensure users remain connected to the same chat server for session consistency.
* **Message Queue (e.g., Kafka, RabbitMQ):**\
  Decouples message processing, prevents loss, and enables scaling.
* **Database Choice:**
  * NoSQL (e.g., Cassandra, DynamoDB) for scalability and high write throughput.
  * SQL for relational data (user profiles, groups).
* **Cache (e.g., Redis):**\
  For hot data like recent messages and user presence.
* **Media Storage:**\
  Use object storage (e.g., AWS S3) for images and videos.
* **End-to-End Encryption:**\
  Secure messages in transit and at rest.

## 6. **API Endpoints (Sample)** <a href="#undefined" id="undefined"></a>

* `POST /api/v1/chat/{conversationId}`: Send message.
* `GET /api/v1/chat/{conversationId}`: Retrieve chat history.
* `GET /api/v1/chat/status/{userId}`: Get user presence.
* `GET /api/v1/lastseen/{userId}`: Get last seen timestamp.
* `POST /api/v1/media/upload`: Upload media file.

## 7. **Scaling and Reliability** <a href="#undefined" id="undefined"></a>

* **Horizontal scaling:** Add more chat servers and API servers behind the load balancer.
* **Replication:** Database and cache replication for high availability.
* **Partitioning/Sharding:** Distribute users/messages across database partitions.
* **Backup & Recovery:** Regular database backups for durability.
* **Monitoring & Alerting:** Use tools like Grafana, Prometheus for system health.

## 8. **Security and Best Practices** <a href="#undefined" id="undefined"></a>

* **Authentication & Authorisation:** JWT tokens, OAuth, ACLs.
* **Encryption:** TLS for all connections, encrypt sensitive data at rest.
* **Rate Limiting:** Prevent spam and abuse.
* **Input Validation:** Prevent injection attacks.
* **Logging & Auditing:** Track user actions and system events.

## 9. **Summary Table** <a href="#undefined" id="undefined"></a>

| Component            | Technology Example       | Purpose                           |
| -------------------- | ------------------------ | --------------------------------- |
| Load Balancer        | AWS ELB, Nginx           | Distribute traffic                |
| Chat Server          | Node.js, Go, Java        | Real-time messaging               |
| WebSocket Gateway    | Socket.io, WS, custom    | Persistent connections            |
| API Server           | Python, Java, Node.js    | Auth, user, media, business logic |
| Message Queue        | Kafka, RabbitMQ          | Reliable message delivery         |
| Database             | Cassandra, DynamoDB, SQL | Store messages, users, groups     |
| Cache                | Redis, Memcached         | Fast access to hot data           |
| Notification Service | Firebase, APNs, custom   | Push notifications                |
| Media Storage        | AWS S3, GCP Storage      | Store images, videos, files       |

**This HLD ensures the chat application is scalable, reliable, secure, and ready for millions of users and messages in real time.**
