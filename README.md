# Cab Booking System Backend

A Spring Boot REST backend for a cab booking platform. It supports rider and driver registration and login, JWT authentication, ride booking and status management, fares, ratings, payments and PDF receipts, email delivery, and WebSocket ride events.

## Technology

- Java 21
- Spring Boot 3.5.6
- Spring Web, Security, Data JPA, Validation, WebSocket, and Mail
- MySQL for the application database
- H2 for tests
- Maven Wrapper
- JWT (jjwt), Lombok, ModelMapper, iText 7, and SpringDoc OpenAPI

## Prerequisites

- JDK 21
- Maven is optional; use the included Maven Wrapper
- MySQL 8 or a compatible MySQL server
- An SMTP account for email receipts (optional for API startup)

## Configuration

The application reads configuration from `src/main/resources/application.properties` and environment variables.

| Environment variable | Purpose | Default |
|---|---|---|
| `DB_HOST` | MySQL host | `localhost` |
| `DB_USERNAME` | MySQL username | `root` |
| `DB_PASSWORD` | MySQL password | none |
| `JWT_SECRET` | JWT signing secret (use a strong value of at least 32 bytes) | local placeholder only |
| `SERVER_PORT` | HTTP port | `8081` |
| `MAIL_HOST` | SMTP host | `smtp.gmail.com` |
| `MAIL_PORT` | SMTP port | `587` |
| `MAIL_USERNAME` | Sender email address | `noreply@example.com` |
| `MAIL_PASSWORD` | SMTP password or app password | none |

Create the database if it does not exist:

```sql
CREATE DATABASE cbsdb;
```

Set local variables on Windows PowerShell:

```powershell
$env:DB_HOST='localhost'
$env:DB_USERNAME='root'
$env:DB_PASSWORD='<your-local-mysql-password>'
$env:JWT_SECRET='<a-random-secret-at-least-32-bytes>'
$env:MAIL_USERNAME='<your-sender-address>'
$env:MAIL_PASSWORD='<your-smtp-password>'
```

Do not commit real database passwords, JWT secrets, SMTP credentials, private keys, or production configuration.

## Setup and Run

From the repository root:

```powershell
.\mvnw.cmd clean spring-boot:run
```

On Unix-like systems:

```bash
./mvnw clean spring-boot:run
```

The API listens on `http://localhost:8081` by default.

- Swagger UI: `http://localhost:8081/swagger-ui.html`
- OpenAPI JSON: `http://localhost:8081/v3/api-docs`

## API Overview

The main endpoint groups are:

- `/api/auth` — rider and driver registration, and login
- `/api/riders` — rider profile and history
- `/api/drivers` — driver profile, status, availability, history, and dashboard
- `/api/rides` — ride booking and ride status
- `/api/ratings` — rider and driver feedback and ratings
- `/api/payments` — payments and PDF receipt generation/email delivery
- `/rides/ws/**` — WebSocket ride events

JWT-protected endpoints expect an `Authorization: Bearer <token>` header. Rider and driver routes enforce role-based access. CORS is currently configured for `http://localhost:4200/`.

## Testing

The test suite uses an in-memory H2 database and Spring test configuration:

```powershell
.\mvnw.cmd test
```

Generate a JaCoCo coverage report (the report is written under `target/site/jacoco`):

```powershell
.\mvnw.cmd test jacoco:report
```

## Build and Deployment

Build an executable JAR:

```powershell
.\mvnw.cmd clean package
```

Run the packaged application:

```powershell
java -jar target\vector-0.0.1-SNAPSHOT.jar
```

For deployment, provide MySQL and SMTP settings through environment variables or your platform's secret manager. Use a strong, environment-specific `JWT_SECRET`; do not use the local placeholder in production. Configure HTTPS, restrict CORS to the deployed frontend origin, and keep database and mail credentials out of source control.

## License

No license file is currently included in this repository. Third-party dependencies retain their respective licenses.
