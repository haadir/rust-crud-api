A lightweight RESTful API built with Rust for managing user data with full CRUD (Create, Read, Update, Delete) operations. The application uses PostgreSQL as the database and runs in Docker containers for easy deployment.

  ## Features

  - Create new users
  - Retrieve a single user by ID
  - Retrieve all users
  - Update existing users
  - Delete users
  - PostgreSQL database integration
  - Docker containerization

  ## Tech Stack

  - **Language**: Rust
  - **Database**: PostgreSQL 12
  - **Serialization**: Serde (JSON)
  - **Database Client**: postgres crate
  - **Containerization**: Docker & Docker Compose

  ## Prerequisites

  - Docker
  - Docker Compose
  - Postman (for testing)

  ## Setup & Installation

  ### Using Docker Compose

  1. Clone the repository
  2. Navigate to the project directory
  3. Build and run the containers:

  docker-compose up --build

  The API will be available at http://localhost:8080
  PostgreSQL will be available at localhost:5432

  Docker Compose Configuration

  The docker-compose.yml file sets up two services:
  - rustapp: The Rust API application (port 8080)
  - db: PostgreSQL database (port 5432)

  Database credentials:
  - User: postgres
  - Password: postgres
  - Database: postgres
  - Connection String: postgres://postgres:postgres@db:5432/postgres

  API Endpoints

  | Method | Endpoint    | Description       |
  |--------|-------------|-------------------|
  | POST   | /users      | Create a new user |
  | GET    | /users      | Get all users     |
  | GET    | /users/{id} | Get a user by ID  |
  | PUT    | /users/{id} | Update a user     |
  | DELETE | /users/{id} | Delete a user     |

  Testing with Postman

  1. Create a User (POST)

  Request:
  POST http://localhost:8080/users
  Content-Type: application/json

  {
    "name": "John Doe",
    "email": "john@example.com"
  }

  Response:
  User created

  2. Get All Users (GET)

  Request:
  GET http://localhost:8080/users

  Response:
  [
    {
      "id": 1,
      "name": "John Doe",
      "email": "john@example.com"
    },
    {
      "id": 2,
      "name": "Jane Smith",
      "email": "jane@example.com"
    }
  ]

  3. Get User by ID (GET)

  Request:
  GET http://localhost:8080/users/1

  Response:
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
  }

  4. Update a User (PUT)

  Request:
  PUT http://localhost:8080/users/1
  Content-Type: application/json

  {
    "name": "John Updated",
    "email": "john.updated@example.com"
  }

  Response:
  User updated

  5. Delete a User (DELETE)

  Request:
  DELETE http://localhost:8080/users/1

  Response:
  User deleted

  Project Structure

  rust-crud-api/
  ├── src/
  │   └── main.rs          # Main application code
  ├── Cargo.toml           # Rust dependencies
  ├── Dockerfile           # Multi-stage Docker build
  ├── docker-compose.yml   # Docker Compose configuration
  └── README.md

  Database Schema

  The application automatically creates a users table with the following schema:

  CREATE TABLE IF NOT EXISTS users (
      id SERIAL PRIMARY KEY,
      name VARCHAR NOT NULL,
      email VARCHAR NOT NULL
  );

  How It Works

  1. The application starts a TCP listener on port 8080
  2. On startup, it connects to PostgreSQL and creates the users table if it doesn't exist
  3. Each HTTP request is parsed to determine the route and method
  4. The appropriate handler function executes the database operation
  5. Responses are returned as HTTP/1.1 with JSON content

  Development

  Building Locally

  cargo build --release

  Running Locally (requires PostgreSQL)

  Set the DATABASE_URL environment variable:

  export DATABASE_URL=postgres://postgres:postgres@localhost:5432/postgres
  cargo run

  Docker Commands

  Stop the containers:
  docker-compose down

  View logs:
  docker-compose logs -f rustapp

  Rebuild after code changes:
  docker-compose up --build

  Remove volumes (clears database):
  docker-compose down -v
