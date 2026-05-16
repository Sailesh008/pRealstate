# Week 7 – Backend Development

## What is this week about?

Developing the **backend services** – setting up the server, implementing APIs, connecting to the database, and ensuring all endpoints work correctly.

---

## Key Questions Answered

| Question | Answer |
|----------|--------|
| **What endpoints are complete?** | 16 endpoints (Auth: 5, Listings: 6, Favourites: 3, Dashboard: 2) |
| **What validations are implemented?** | Email format, password strength, price range, required fields, file types |
| **What is the progress?** | Backend is 80% complete, ready for frontend integration |

---

## Activities Done

| Activity | What We Did |
|----------|-------------|
| Set up server | Initialized Node.js + Express project with MVC structure |
| Configured database | Connected PostgreSQL using Knex.js with connection pooling |
| Created migrations | Set up 5 tables with relationships and indexes |
| Implemented authentication | JWT generation, bcrypt password hashing, HTTP-only cookies |
| Built listing APIs | Full CRUD operations with image upload |
| Built search API | Multi-criteria filtering with pagination and sorting |
| Integrated geocoding | Mapbox API for address-to-coordinates conversion |
| Added validation | Joi schemas for all request bodies |
| Added error handling | Centralized error handler with proper status codes |
| Wrote unit tests | 65% code coverage for backend logic |

---

## 1. Completed Endpoints

### Authentication APIs (5 endpoints)

| Method | Endpoint | Status | Validation |
|--------|----------|--------|------------|
| POST | `/api/auth/register` | ✅ Complete | Email, password, full_name required |
| POST | `/api/auth/login` | ✅ Complete | Email, password required |
| POST | `/api/auth/logout` | ✅ Complete | JWT token required |
| GET | `/api/auth/me` | ✅ Complete | JWT token required |
| PUT | `/api/auth/me` | ✅ Complete | JWT token required |

### Listing APIs (6 endpoints)

| Method | Endpoint | Status | Validation |
|--------|----------|--------|------------|
| GET | `/api/listings` | ✅ Complete | Query params optional |
| GET | `/api/listings/:id` | ✅ Complete | ID must be positive integer |
| POST | `/api/listings` | ✅ Complete | Title, price, address required |
| PUT | `/api/listings/:id` | ✅ Complete | Agent can edit own listing |
| DELETE | `/api/listings/:id` | ✅ Complete | Agent can delete own listing |
| POST | `/api/listings/:id/images` | ✅ Complete | Image file required (max 5 per listing) |

### Favourites APIs (3 endpoints)

| Method | Endpoint | Status | Validation |
|--------|----------|--------|------------|
| GET | `/api/favourites` | ✅ Complete | JWT token required |
| POST | `/api/favourites/:listingId` | ✅ Complete | Listing ID must exist |
| DELETE | `/api/favourites/:listingId` | ✅ Complete | JWT token required |

### Dashboard APIs (2 endpoints)

| Method | Endpoint | Status | Validation |
|--------|----------|--------|------------|
| GET | `/api/dashboard/agent` | ✅ Complete | Agent role required |
| GET | `/api/dashboard/admin` | ✅ Complete | Admin role required |

---

## 2. Validation Rules Implemented

### User Registration Validation

| Field | Rule | Error Message |
|-------|------|---------------|
| email | Required, valid email format, max 255 chars | "Valid email is required" |
| password | Required, min 8 chars, at least 1 number | "Password must be at least 8 characters with 1 number" |
| full_name | Required, min 2 chars, max 100 chars | "Full name must be 2-100 characters" |
| role | Optional, must be 'guest', 'agent', or 'admin' | "Invalid role selected" |

### Listing Creation Validation

| Field | Rule | Error Message |
|-------|------|---------------|
| title | Required, min 5 chars, max 255 chars | "Title must be 5-255 characters" |
| price | Required, must be > 0 | "Price must be greater than 0" |
| address | Required, max 500 chars | "Valid address is required" |
| bedrooms | Required, must be >= 0 | "Bedrooms must be 0 or more" |
| bathrooms | Required, must be >= 0 | "Bathrooms must be 0 or more" |
| property_type | Required, must be valid type | "Property type must be house, apartment, townhouse, or land" |

### Image Upload Validation

| Rule | Value | Error Message |
|------|-------|---------------|
| Max file size | 10MB | "File size exceeds 10MB limit" |
| Allowed formats | JPEG, PNG, WebP | "Only JPEG, PNG, and WebP images are allowed" |
| Max images per listing | 5 | "Maximum 5 images per listing" |

---

## 3. Database Connection

### Connection Configuration

```javascript
// config/database.js
const knex = require('knex');

const db = knex({
  client: 'postgresql',
  connection: {
    host: process.env.DB_HOST || 'localhost',
    port: process.env.DB_PORT || 5432,
    database: process.env.DB_NAME || 'aussie_realestate',
    user: process.env.DB_USER || 'postgres',
    password: process.env.DB_PASSWORD || ''
  },
  pool: { min: 2, max: 10 },
  migrations: { directory: './migrations' },
  seeds: { directory: './seeds' }
});

module.exports = db;
