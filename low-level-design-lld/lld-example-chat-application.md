# LLD Example: Chat Application

Building a real-time chat application like WhatsApp, Slack, or Messenger requires more than just sending messages between users. The system must support millions of concurrent users, ensure instant message delivery, manage presence, handle media sharing, and scale seamlessly. In this Low-Level Design example, we break down the chat system into concrete, maintainable TypeScript classes, interfaces, and data structures, with a focus on extensibility and reliability.

***

## 1. Core Classes & Responsibilities

| Class/Component       | Responsibility                                                    |
| --------------------- | ----------------------------------------------------------------- |
| `User`                | Represents a chat user, their profile, and status                 |
| `Message`             | Represents a chat message (text/media, sender, timestamp, status) |
| `ChatRoom`            | Represents a group or 1:1 chat, manages participants and messages |
| `ChatServer`          | Handles message routing, delivery, and presence management        |
| `PresenceManager`     | Tracks user online/offline status                                 |
| `NotificationService` | Sends push notifications to offline users                         |
| `MediaStorageService` | Handles upload and retrieval of media files                       |
| `CacheService`        | Caches recent messages and user presence for fast access          |
| `MessageQueue`        | Buffers and delivers messages asynchronously for reliability      |

***

## 2. Class Diagram (Text Representation)

```
+-------------------+         +------------------+         +-----------------+
|      User         |<>------>|   ChatRoom       |<>------>|    Message      |
+-------------------+         +------------------+         +-----------------+
| id: string        |         | id: string       |         | id: string      |
| name: string      |         | name: string     |         | senderId: string|
| status: Status    |         | participants: [] |         | content: string |
| ...               |         | messages: []     |         | timestamp: Date |
+-------------------+         +------------------+         | status: Status  |
                                                          +-----------------+
         ^                               ^
         |                               |
+-------------------+           +-----------------------+
| PresenceManager   |           |   ChatServer          |
+-------------------+           +-----------------------+
| userStatus: Map<> |           | rooms: Map<>          |
| setStatus()       |           | sendMessage()         |
| getStatus()       |           | deliverMessage()      |
+-------------------+           | joinRoom()            |
                                | leaveRoom()           |
                                +-----------------------+
```

***

## 3. Database Schema

**User Table**

| Field    | Type      | Description           |
| -------- | --------- | --------------------- |
| id       | UUID      | Unique user ID        |
| name     | VARCHAR   | Display name          |
| status   | ENUM      | Online/Offline/Away   |
| lastSeen | TIMESTAMP | Last active timestamp |

**ChatRoom Table**

| Field   | Type    | Description       |
| ------- | ------- | ----------------- |
| id      | UUID    | Unique room ID    |
| name    | VARCHAR | Room name         |
| isGroup | BOOLEAN | Group or 1:1 chat |

**Message Table**

| Field     | Type      | Description               |
| --------- | --------- | ------------------------- |
| id        | UUID      | Unique message ID         |
| roomId    | UUID      | ChatRoom ID               |
| senderId  | UUID      | Sender User ID            |
| content   | TEXT      | Message text or media URL |
| timestamp | TIMESTAMP | Sent time                 |
| status    | ENUM      | Sent/Delivered/Read       |
| type      | ENUM      | Text/Image/Video/File     |

***

## 4. Method Signatures (TypeScript)

```typescript
// User
class User {
  constructor(
    public id: string,
    public name: string,
    public status: Status = 'offline'
  ) {}
}

// Message
class Message {
  constructor(
    public id: string,
    public roomId: string,
    public senderId: string,
    public content: string,
    public timestamp: Date,
    public status: MessageStatus = 'sent',
    public type: MessageType = 'text'
  ) {}
}

// ChatRoom
class ChatRoom {
  constructor(
    public id: string,
    public name: string,
    public participants: User[] = [],
    public messages: Message[] = []
  ) {}

  addParticipant(user: User): void
  removeParticipant(userId: string): void
  addMessage(message: Message): void
  getRecentMessages(limit: number): Message[]
}

// PresenceManager
class PresenceManager {
  private userStatus: Map<string, Status> = new Map();

  setStatus(userId: string, status: Status): void
  getStatus(userId: string): Status
}

// ChatServer
class ChatServer {
  private rooms: Map<string, ChatRoom> = new Map();

  sendMessage(roomId: string, message: Message): void
  deliverMessage(roomId: string, message: Message): void
  joinRoom(roomId: string, user: User): void
  leaveRoom(roomId: string, userId: string): void
}

// NotificationService
class NotificationService {
  sendNotification(userId: string, message: Message): void
}

// MediaStorageService
class MediaStorageService {
  uploadMedia(file: File): Promise<string> // returns media URL
  getMedia(url: string): Promise<File>
}

// CacheService
class CacheService {
  getRecentMessages(roomId: string): Message[]
  setRecentMessages(roomId: string, messages: Message[]): void
  getUserStatus(userId: string): Status
  setUserStatus(userId: string, status: Status): void
}

// MessageQueue
class MessageQueue {
  enqueue(message: Message): void
  dequeue(): Message | null
}
```

***

## 5. Sequence Diagram (Text Representation)

### A. Sending a Message

```
text[User] → [ChatClient] → [ChatServer] → [MessageQueue] → [ChatServer] → [ChatRoom] → [Recipient User(s)]
         |               |                 |                |               |            |
         |               |                 |                |               |            |
         |               |                 |                |               |            v
         |               |                 |                |               |      [NotificationService] (if offline)
```

### B. Receiving a Message

1. The recipient’s client receives the message in real-time via WebSocket.
2. If offline, NotificationService sends a push notification.
3. Read receipt is sent back to the server and updated for the sender.

### C. Media Sharing

1. User uploads media via MediaStorageService.
2. The media URL is sent as a message through the chat flow.

***

## 6. Error Handling

| Error Type           | Scenario                    | Handling                        |
| -------------------- | --------------------------- | ------------------------------- |
| UserNotFoundError    | User does not exist         | Return error, notify client     |
| RoomNotFoundError    | Chat room does not exist    | Return error, notify client     |
| MessageDeliveryError | Message cannot be delivered | Retry, log error, notify sender |
| MediaUploadError     | Media file upload fails     | Return error, allow retry       |

***

## 7. Design Patterns & Principles

* **MVC Pattern:** Separates data (Model), UI (View), and logic (Controller).
* **Observer Pattern:** Notifies users about new messages and presence changes.
* **Factory Pattern:** Creates users, chat rooms, and messages.
* **Singleton Pattern:** Ensures a single instance for managers like PresenceManager.
* **Proxy Pattern:** Handles communication between clients and the chat server.
* **Publish-Subscribe Pattern:** Used for message delivery and notifications.
* **Load Balancing Pattern:** Distributes user requests across multiple servers.
* **SOLID Principles:** For maintainable, extensible code.

***

## 8. API Endpoints (Sample)

* `POST /api/v1/messages/send` - Send a message to a chat room.
* `GET /api/v1/messages/history?roomId=...` - Fetch recent messages.
* `POST /api/v1/rooms/create` - Create a new chat room.
* `POST /api/v1/media/upload` - Upload a media file.
* `GET /api/v1/presence/{userId}` - Get user presence status.

***

## 9. Summary Table

| Component           | Technology/Pattern         | Purpose                                   |
| ------------------- | -------------------------- | ----------------------------------------- |
| ChatServer          | Node.js, Go, Java          | Real-time message routing & delivery      |
| WebSocket Gateway   | Socket.io, WS, custom      | Persistent client-server connections      |
| API Server          | Express, Fastify, Spring   | REST APIs for user, room, media, presence |
| MessageQueue        | Kafka, RabbitMQ, Redis     | Reliable, async message delivery          |
| Database            | Cassandra, DynamoDB, Mongo | Stores users, messages, rooms             |
| Cache               | Redis, Memcached           | Fast access to hot data                   |
| NotificationService | Firebase, APNs, custom     | Push notifications                        |
| MediaStorageService | AWS S3, GCP Storage        | Store images, videos, files               |

***

**This LLD provides a modular, scalable, and production-ready blueprint for building a real-time chat application, ready to handle millions of users and messages.**
