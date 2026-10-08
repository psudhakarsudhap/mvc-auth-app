# MVC Auth App

A Node.js and Express REST API for user registration and login, plus authenticated, user-specific todo management. It uses MongoDB through Mongoose, bcrypt for password hashing, and JSON Web Tokens (JWT) for authentication.

## Requirements

- Node.js (an LTS release is recommended)
- npm
- MongoDB, either local or hosted

## Setup

1. Install dependencies:

   ```bash
   npm install
   ```

2. Create a local environment file from the example:

   ```bash
   cp .env.example .env
   ```

   On Windows Command Prompt, use `copy .env.example .env` instead.

3. Set the values in `.env`:

   | Variable | Description |
   | --- | --- |
   | `PORT` | Port for the API server (defaults to `9000`) |
   | `MONGO_URI` | MongoDB connection URI |
   | `DB_NAME` | Database name |
   | `JWT_SECRET` | Secret used to sign authentication tokens; use a strong, private value |

4. Start the server:

   ```bash
   npm run dev
   ```

   Or run `npm start` to start without watch mode. The API is available at `http://localhost:9000` by default. `GET /` returns a basic server status message.

## API

All request and response bodies use JSON. Register and log in using the user endpoints; include the returned JWT in the `Authorization` header as `Bearer YOUR_JWT` when calling any todo endpoint.

| Method | Endpoint | Authentication | Description |
| --- | --- | --- | --- |
| `POST` | `/api/v1/users/register` | No | Register with `username`, `email`, and `password` |
| `POST` | `/api/v1/users/login` | No | Log in with `email` and `password`; returns a JWT |
| `POST` | `/api/v1/todos` | Bearer token | Create a todo with `todo` |
| `GET` | `/api/v1/todos` | Bearer token | List your todos; optionally filter with `?completed=true` or `?completed=false` |
| `GET` | `/api/v1/todos/:id` | Bearer token | Get one of your todos |
| `PUT` | `/api/v1/todos/:id` | Bearer token | Update a todo, for example with `todo` or `completed` |
| `DELETE` | `/api/v1/todos/:id` | Bearer token | Delete one of your todos |

Example login request:

```json
{
  "email": "user@example.com",
  "password": "your-password"
}
```

Example create-todo request:

```json
{
  "todo": "Read the project documentation"
}
```

## Project structure

```text
config/       MongoDB connection
controllers/  Request handlers for users and todos
middleware/   JWT authentication middleware
models/       Mongoose user and todo models
routes/       Express API routes
server.js     Application entry point
```

Do not commit `.env` or real credentials. The `.env.example` file is a template; replace example values, especially `JWT_SECRET`, before running the app in a real environment.
