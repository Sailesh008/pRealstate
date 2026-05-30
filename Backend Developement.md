# Week 7 — Backend Development

## Overview

| Field        | Details          |
|--------------|------------------|
| **Week**     | 7                |
| **Focus**    | Backend Development |
| **Status**   | In Progress      |
| **Last Updated** | YYYY-MM-DD   |

---

## Expected Deliverable

Develop backend services including server setup, API implementation, and database integration.

---

## Activities

- [ ] Set up server environment
- [ ] Implement API endpoints
- [ ] Connect and configure database

---

## API Endpoints

### Completed Endpoints

| Method   | Endpoint            | Description         | Validation               | Status      |
|----------|---------------------|---------------------|--------------------------|-------------|
| GET      | /api/resource       | Fetch all records   | Query params validated   | Done        |
| POST     | /api/resource       | Create new record   | Body schema validated    | Done        |
| GET      | /api/resource/:id   | Fetch single record | ID format validated      | Done        |
| PUT      | /api/resource/:id   | Update record       | Partial update supported | In Progress |
| DELETE   | /api/resource/:id   | Delete record       | Auth check required      | Pending     |

Note: Replace /api/resource with your actual endpoint names.

---

## Validations Implemented

- [x] Request body schema validation (e.g., required fields, data types)
- [x] URL parameter format checks (e.g., valid UUID / integer ID)
- [x] Authentication middleware applied to protected routes
- [ ] Rate limiting
- [ ] Input sanitization for SQL/NoSQL injection prevention

---

## Database

- Database:        e.g., PostgreSQL / MongoDB / MySQL
- ORM / Driver:    e.g., Prisma / Mongoose / Sequelize
- Connection Status: Connected / Pending

### Schema / Collections

| Table / Collection | Description           | Status      |
|--------------------|-----------------------|-------------|
| users              | Stores user accounts  | Done        |
| sessions           | Auth session tokens   | Done        |
| resources          | Core data model       | In Progress |

---

## Known Issues / Blockers

- List any current blockers or bugs here
- None at this time.

---

## Repository Structure

backend/
    src/
        routes/         - API route definitions
        controllers/    - Request handlers
        models/         - Database models/schemas
        middleware/     - Auth, validation, error handling
        config/         - Environment and DB config
    tests/              - Unit and integration tests
    .env.example        - Environment variable template
    package.json
    README.md

---

## Testing

| Test Type              | Tool              | Status      |
|------------------------|-------------------|-------------|
| Unit Tests             | Jest / Mocha      | In Progress |
| API Integration Tests  | Supertest/Postman | Pending     |

---

## Notes & Next Steps

- [ ] Complete remaining endpoints (see table above)
- [ ] Add input sanitization middleware
- [ ] Write integration tests for all routes
- [ ] Document API with Swagger / OpenAPI

---

## References

- API Documentation    : Link to Swagger/Postman collection
- Database Schema      : Link to ERD or schema diagram
- Project Board        : Link to GitHub Issues / Jira

