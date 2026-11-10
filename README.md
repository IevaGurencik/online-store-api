# Online Store API

REST API for an e-commerce catalog: products, categories and users, with JWT authentication and role-based access.

## Tech stack

- Java 21, Spring Boot 3.5, Maven
- Spring Security + JWT (jjwt)
- Spring Data JPA / Hibernate, PostgreSQL, Flyway
- springdoc-openapi (Swagger UI)
- JUnit 5, Mockito, MockMvc

## Features

- User registration and login with JWT tokens
- Two roles: `USER` and `ADMIN`. Anyone can browse the catalog; only admins can change it
- Product filtering by price range, category, active status and text search
- Database schema and demo catalog data managed with Flyway migrations
- Global exception handling with a unified error response
- Layered architecture: controller → service → repository, DTOs and mappers

## API

| Method | Endpoint | Access | Description |
|---|---|---|---|
| POST | `/api/auth/register` | Public | Register a new user |
| POST | `/api/auth/login` | Public | Log in and receive a JWT |
| GET | `/api/products` | Public | List products (`minPrice`, `maxPrice`, `categoryId`, `active`, `q`) |
| GET | `/api/products/{id}` | Public | Get a product |
| POST | `/api/products` | Admin | Create a product |
| PUT | `/api/products/{id}` | Admin | Update a product |
| PATCH | `/api/products/{id}/stock` | Admin | Update stock |
| DELETE | `/api/products/{id}` | Admin | Delete a product |
| GET | `/api/categories` | Public | List categories |
| GET | `/api/categories/{id}` | Public | Get a category |
| POST | `/api/categories` | Admin | Create a category |
| PUT | `/api/categories/{id}` | Admin | Update a category |
| DELETE | `/api/categories/{id}` | Admin | Delete a category |

Protected endpoints expect the header `Authorization: Bearer <token>`.

Full interactive documentation is available in Swagger UI once the app is running.

## Getting started

**Requirements:** JDK 21, PostgreSQL

1. Create the database:
   ```bash
   createdb store
   ```
2. Set environment variables:
   ```bash
   export DB_PASSWORD=<your postgres password>
   export JWT_SECRET=<random string, at least 32 characters>
   ```
3. Run the application:
   ```bash
   ./mvnw spring-boot:run
   ```
   Flyway creates the schema and seeds roles, a demo admin user and a demo catalog on first start.

4. Open Swagger UI: http://localhost:8080/swagger-ui.html

## Tests

```bash
./mvnw test
```

Service-layer tests use Mockito; controller tests use MockMvc.

## Project structure

```
src/main/java/com/example/projectREST
├── config/          # Security config, JWT filter
├── controller/      # REST controllers
├── dto/             # Request/response DTOs
├── exceptionhandle/ # Global exception handler
├── mapper/          # Entity ↔ DTO mappers
├── model/           # JPA entities
├── repository/      # Spring Data repositories
└── service/         # Business logic, JWT provider
src/main/resources/db/migration  # Flyway migrations
```
