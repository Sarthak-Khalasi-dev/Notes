# Notes API Application

A complete RESTful API built with Node.js, Express, and Mongoose for managing notes. This application supports single and bulk note operations, custom error handling, mock authorization, request logging, and structured JSON responses.

Postman Documentation: [Link to Postman Docs](https://documenter.getpostman.com/view/50841270/2sBXqJL1jf)

## Features

- **Notes Management**: Create, view, update, replace, and delete notes.
- **Bulk Operations**: Create multiple notes in bulk or delete multiple notes by ID in a single request.
- **Data Validation**: Enforced schemas for note properties (`title`, `content`, `category`, and `isPinned`).
- **Flexible Sorting**: Notes list retrieves in reverse-chronological order (most recent first).
- **Request Logging**: Middleware to log the exact timestamp of incoming requests.
- **Error Handling**: Global error catching and formatted error responses.
- **Mock Authentication**: Auth middleware for route-level access protection (optional check).

---

## Tech Stack

- **Runtime Environment**: Node.js
- **Web Framework**: Express (v5.x)
- **Database Wrapper**: Mongoose / MongoDB (v9.x)
- **Development Tooling**: Nodemon, Dotenv

---

## Getting Started

### Prerequisites

Make sure you have the following installed on your machine:
- [Node.js](https://nodejs.org/) (v16+ recommended)
- [MongoDB](https://www.mongodb.com/) (running locally or a remote MongoDB Atlas URI)

### Installation

1. **Clone or navigate** to the repository folder:
   ```bash
   cd Notes
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Configure Environment Variables**:
   Copy `.env.example` to create a `.env` file in the root of the project:
   ```bash
   cp .env.example .env
   ```
   Open the `.env` file and set your desired configurations:
   ```env
   PORT=3000
   MONGO_URI=mongodb://localhost:27017/notes-app
   ```

---

## Running the Server

### Development Mode (Auto-restart on file changes)
```bash
npm run dev
```

### Production Mode
```bash
npm start
```

Once started, the server will log:
```text
MongoDB connected successfully.
Server is running on port http://localhost:3000
```

---

## API Endpoints

All endpoints are prefixed with `/api/notes`.

### Core Note Endpoints

| Method | Endpoint | Description | Payloads / Params |
| :--- | :--- | :--- | :--- |
| **GET** | `/test` | Server health check | None |
| **POST** | `/api/notes` | Create a single note | `{ title, content, category, isPinned }` |
| **POST** | `/api/notes/bulk` | Create multiple notes in bulk | `{ notes: [{ title, content, ... }] }` |
| **GET** | `/api/notes` | Retrieve all notes | None |
| **GET** | `/api/notes/:id` | Get note by ID | Path parameter `:id` |
| **PUT** | `/api/notes/:id` | Replace note fully | `{ title, content, category, isPinned }` |
| **PATCH** | `/api/notes/:id` | Partially update note | JSON payload with fields to update |
| **DELETE** | `/api/notes/:id` | Delete a note | Path parameter `:id` |
| **DELETE** | `/api/notes/bulk` | Delete notes in bulk | `{ ids: ["id1", "id2", ...] }` |

### Field Details for Notes

- `title`: String (Required, trimmed)
- `content`: String (Required, trimmed)
- `category`: String (Enum: `"work"`, `"personal"`, `"study"`. Default: `"personal"`)
- `isPinned`: Boolean (Default: `false`)

---

## Middlewares

1. **Logger Middleware (`src/middlewares/logger.middleware.js`)**: Logs the request timestamps to the console.
2. **Auth Middleware (`src/middlewares/auth.middleware.js`)**: Verifies the `Authorization` header against the mock token `"valid_token"`. Can be added to secure routes.
3. **Global Error Handler (`src/middlewares/errorHandler.middleware.js`)**: Automatically catches operational errors and returns consistent `{ success, message }` JSON responses.
