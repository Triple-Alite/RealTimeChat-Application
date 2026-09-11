# Real-Time Chat Application Backend

A production-ready Node.js, Express, Socket.IO, MongoDB, Redis, JWT, and Swagger-based real-time chat API.

## Project Overview

This project provides a modular backend for a real-time chat application that supports authentication, private and group rooms, message persistence, read receipts, typing indicators, and Redis-backed online/offline presence.

## Features

- JWT Authentication
- Private Messaging
- Group Chat
- WebSocket Communication
- Message Persistence
- Typing Indicators
- Online/Offline Presence
- Read Receipts
- Room Management
- Redis Presence Tracking
- MongoDB Message Storage
- Swagger API Documentation

## Tech Stack

- Node.js: JavaScript runtime
- Express.js: REST API framework
- Socket.IO: realtime bidirectional communication
- MongoDB: primary data storage for users, rooms, and messages
- Mongoose: MongoDB object modeling
- Redis: presence tracking and session state
- JWT: authenticated access tokens
- bcryptjs: password hashing
- dotenv: environment configuration
- cors: cross-origin request support
- swagger-jsdoc + swagger-ui-express: API docs
- Nodemon: development server restart
- Jest + Supertest: automated testing

## Installation

```bash
git clone <repository-url>
cd realtime-chat-api
npm install
```

Configure `.env` using the `.env.example` file. Ensure MongoDB and Redis are running locally.

Development:

```bash
npm run dev
```

Production:

```bash
npm start
```

## API

### Health Check

```http
GET /api/health
```

### Authentication

```http
POST /api/auth/register
POST /api/auth/login
```

### Rooms

```http
POST /api/rooms
GET /api/rooms
POST /api/rooms/:id/members
DELETE /api/rooms/:id/members/:userId
```

### Messages

```http
GET /api/rooms/:id/messages
```

## WebSocket Protocol

The server supports Socket.IO authentication using JWT:

```js
import { io } from "socket.io-client";

const socket = io("http://localhost:5000", {
  auth: {
    token: "YOUR_JWT_TOKEN"
  }
});

socket.on("connect", () => {
  console.log("Connected:", socket.id);
});

socket.on("message:new", (message) => {
  console.log("New message:", message);
});

socket.on("user:online", (user) => {
  console.log("User online:", user);
});

socket.on("user:offline", (user) => {
  console.log("User offline:", user);
});

socket.emit("message:send", {
  room_id: "ROOM_ID",
  content: "Hello!",
  type: "text"
});
```

### Socket Events

Client -> Server:

- `message:send` payload:

```json
{
  "room_id": "room_id",
  "content": "Hello!",
  "type": "text"
}
```

- `message:read` payload:

```json
{
  "room_id": "room_id",
  "message_id": "message_id"
}
```

- `user:typing` payload:

```json
{
  "room_id": "room_id",
  "is_typing": true
}
```

Server -> Client:

- `message:new`
- `user:typing`
- `user:online`
- `user:offline`
- `room:updated`
- `error`

## Swagger

API documentation is exposed at `/api/docs`.

WebSocket functionality is tested through a Socket.IO client rather than Swagger.

## Testing

```bash
npm test
```

## Production Notes

This is a clean scaffolding and ready-to-be-extended backend with production-oriented structure, validation middleware, centralized error handling, and service-oriented business logic separation.
