# Leave Management System

A RESTful backend API for managing employee leave requests, built for **IST Africa**. The system handles the full leave request lifecycle — submission, approval, and tracking — with secure JWT-based authentication and OAuth2 support.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 21 |
| Framework | Spring Boot 3.2.3 |
| Security | Spring Security + JWT (jjwt 0.11.5) + OAuth2 |
| Database | PostgreSQL |
| ORM | Spring Data JPA / Hibernate |
| Build Tool | Maven (via Maven Wrapper) |
| Containerization | Docker + Docker Compose |
| Utilities | Lombok, Jakarta Validation, spring-dotenv |

---

## Prerequisites

- Java 21+
- Maven 3.9+ (or use the included `./mvnw` wrapper)
- PostgreSQL database
- Docker & Docker Compose (for containerized setup)

---

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/NAILLAHHHH/Leave_Management_System-main.git
cd Leave_Management_System-main
```

### 2. Configure Environment Variables

Create a `.env` file in the project root (the app uses `spring-dotenv` to load it):

```env
SPRING_DATASOURCE_URL=jdbc:postgresql://<host>:<port>/<database>
SPRING_DATASOURCE_USERNAME=<your_db_user>
SPRING_DATASOURCE_PASSWORD=<your_db_password>
SPRING_JPA_HIBERNATE_DDL_AUTO=update
JWT_SECRET=<your_jwt_secret>
```

### 3. Run Locally

```bash
./mvnw spring-boot:run
```

The application starts on **http://localhost:8080**.

### 4. Run with Docker Compose

```bash
docker-compose up --build
```

This builds the application image and starts the service, exposing port `8080`.

---

## Build

```bash
# Package the JAR (skip tests)
./mvnw clean package -DskipTests

# Run tests
./mvnw test
```

---

## Docker

The project uses a two-stage Docker build:

1. **Build stage** — uses `maven:3.9.6-eclipse-temurin-21` to compile and package the JAR.
2. **Run stage** — uses a lightweight `eclipse-temurin:21-jre-jammy` image to run it.

```bash
# Build the image manually
docker build -t leave-management-system .

# Run the container
docker run -p 8080:8080 leave-management-system
```

---

## Security

- **JWT Authentication** — stateless token-based auth using the `jjwt` library.
- **OAuth2 Client** — supports OAuth2 login flows via Spring Security OAuth2 Client.
- **OAuth2 Resource Server** — validates bearer tokens with `spring-security-oauth2-resource-server` and `spring-security-oauth2-jose`.
- **Input Validation** — enforced via Jakarta Validation and Hibernate Validator on request payloads.

---

## Project Structure

```
src/
└── main/
    └── java/com/ist/
        ├── controller/     # REST API endpoints
        ├── service/        # Business logic
        ├── repository/     # JPA data access layer
        ├── model/          # Entity classes
        ├── dto/            # Data Transfer Objects
        ├── security/       # JWT & auth configuration
        └── config/         # Spring configuration
```

---

## API Overview

All endpoints are available under `http://localhost:8080`. Authentication is required for protected routes (pass a valid JWT as a `Bearer` token in the `Authorization` header).

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/register` | Register a new user |
| POST | `/auth/login` | Authenticate and receive a JWT |
| GET | `/leaves` | List all leave requests |
| POST | `/leaves` | Submit a new leave request |
| GET | `/leaves/{id}` | Get a specific leave request |
| PUT | `/leaves/{id}` | Update a leave request |
| DELETE | `/leaves/{id}` | Delete a leave request |

> Refer to the source controllers for the full and authoritative list of endpoints.

---

## Configuration Reference

| Property | Description |
|---|---|
| `SPRING_DATASOURCE_URL` | JDBC URL for the PostgreSQL database |
| `SPRING_DATASOURCE_USERNAME` | Database username |
| `SPRING_DATASOURCE_PASSWORD` | Database password |
| `SPRING_JPA_HIBERNATE_DDL_AUTO` | Schema management (`update`, `create`, `none`) |

---
