
> **Deliverable:** Database Schema and API Specification  
> **Activities:** Define database schema · Define REST APIs · Define validation rules

---

## Table of Contents

- [Overview](#overview)
- [Entities & Relationships](#entities--relationships)
- [Database Schema](#database-schema)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- [REST API Specification](#rest-api-specification)
- [Validation Rules](#validation-rules)
- [Error Handling](#error-handling)
- [Glossary](#glossary)

---

## Overview

| Item                | Details                                      |
|---------------------|----------------------------------------------|
| **Database**        | PostgreSQL 16                                |
| **ORM**             | Prisma / TypeORM / Sequelize                 |
| **API Style**       | REST over HTTPS                              |
| **Auth**            | JWT Bearer Token                             |
| **Base URL**        | `https://api.yourdomain.com/v1`              |
| **Data Format**     | JSON (`Content-Type: application/json`)      |
| **API Version**     | v1                                           |

---

## Entities & Relationships

### What Entities Exist?

| Entity       | Description                                           |
|--------------|-------------------------------------------------------|
| `User`       | Registered application user (customer or admin)       |
| `Profile`    | Extended user details (one-to-one with User)          |
| `Product`    | Item available for purchase or interaction            |
| `Category`   | Grouping label for products                           |
| `Order`      | A purchase transaction placed by a user               |
| `OrderItem`  | Individual line item within an order                  |
| `Review`     | User-submitted rating and comment on a product        |
| `Tag`        | Searchable keyword associated with products           |

> Replace or extend these entities to match your actual domain.

---

### What Relationships Exist?

```
User ──────────── Profile          (One-to-One)
User ──────────── Order            (One-to-Many)
User ──────────── Review           (One-to-Many)
Order ─────────── OrderItem        (One-to-Many)
Product ──────── OrderItem         (One-to-Many)
Product ──────── Review            (One-to-Many)
Product ──────── Category          (Many-to-One)
Product ──────── Tag               (Many-to-Many)
```

---

## Database Schema

### `users`

```sql
CREATE TABLE users (
  id            UUID          PRIMARY KEY DEFAULT gen_random_uuid(),
  email         VARCHAR(255)  NOT NULL UNIQUE,
  password_hash VARCHAR(255)  NOT NULL,
  role          VARCHAR(20)   NOT NULL DEFAULT 'customer'
                              CHECK (role IN ('customer', 'admin')),
  is_active     BOOLEAN       NOT NULL DEFAULT TRUE,
  created_at    TIMESTAMPTZ   NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMPTZ   NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
```

---

### `profiles`

```sql
CREATE TABLE profiles (
  id           UUID          PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id      UUID          NOT NULL UNIQUE REFERENCES users(id) ON DELETE CASCADE,
  first_name   VARCHAR(100),
  last_name    VARCHAR(100),
  phone        VARCHAR(20),
  avatar_url   TEXT,
  bio          TEXT,
  created_at   TIMESTAMPTZ   NOT NULL DEFAULT NOW(),
  updated_at   TIMESTAMPTZ   NOT NULL DEFAULT NOW()
);
```

---

### `categories`

```sql
CREATE TABLE categories (
  id          UUID          PRIMARY KEY DEFAULT gen_random_uuid(),
  name        VARCHAR(100)  NOT NULL UNIQUE,
  slug        VARCHAR(120)  NOT NULL UNIQUE,
  description TEXT,
  created_at  TIMESTAMPTZ   NOT NULL DEFAULT NOW()
);
```

---

### `products`

```sql
CREATE TABLE products (
  id            UUID           PRIMARY KEY DEFAULT gen_random_uuid(),
  category_id   UUID           REFERENCES categories(id) ON DELETE SET NULL,
  name          VARCHAR(255)   NOT NULL,
  slug          VARCHAR(280)   NOT NULL UNIQUE,
  description   TEXT,
  price         NUMERIC(10,2)  NOT NULL CHECK (price >= 0),
  stock         INTEGER        NOT NULL DEFAULT 0 CHECK (stock >= 0),
  is_published  BOOLEAN        NOT NULL DEFAULT FALSE,
  created_at    TIMESTAMPTZ    NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMPTZ    NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_products_category ON products(category_id);
CREATE INDEX idx_products_slug     ON products(slug);
```

---

### `tags`

```sql
CREATE TABLE tags (
  id    UUID         PRIMARY KEY DEFAULT gen_random_uuid(),
  name  VARCHAR(50)  NOT NULL UNIQUE
);
```

---

### `product_tags` *(join table)*

```sql
CREATE TABLE product_tags (
  product_id  UUID  NOT NULL REFERENCES products(id) ON DELETE CASCADE,
  tag_id      UUID  NOT NULL REFERENCES tags(id)     ON DELETE CASCADE,
  PRIMARY KEY (product_id, tag_id)
);
```

---

### `orders`

```sql
CREATE TABLE orders (
  id            UUID           PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id       UUID           NOT NULL REFERENCES users(id) ON DELETE RESTRICT,
  status        VARCHAR(30)    NOT NULL DEFAULT 'pending'
                               CHECK (status IN ('pending','confirmed','shipped','delivered','cancelled')),
  total_amount  NUMERIC(12,2)  NOT NULL CHECK (total_amount >= 0),
  notes         TEXT,
  created_at    TIMESTAMPTZ    NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMPTZ    NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_orders_user   ON orders(user_id);
CREATE INDEX idx_orders_status ON orders(status);
```

---

### `order_items`

```sql
CREATE TABLE order_items (
  id          UUID           PRIMARY KEY DEFAULT gen_random_uuid(),
  order_id    UUID           NOT NULL REFERENCES orders(id)   ON DELETE CASCADE,
  product_id  UUID           NOT NULL REFERENCES products(id) ON DELETE RESTRICT,
  quantity    INTEGER        NOT NULL CHECK (quantity > 0),
  unit_price  NUMERIC(10,2)  NOT NULL CHECK (unit_price >= 0),
  subtotal    NUMERIC(12,2)  GENERATED ALWAYS AS (quantity * unit_price) STORED
);

CREATE INDEX idx_order_items_order   ON order_items(order_id);
CREATE INDEX idx_order_items_product ON order_items(product_id);
```

---

### `reviews`

```sql
CREATE TABLE reviews (
  id          UUID         PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     UUID         NOT NULL REFERENCES users(id)    ON DELETE CASCADE,
  product_id  UUID         NOT NULL REFERENCES products(id) ON DELETE CASCADE,
  rating      SMALLINT     NOT NULL CHECK (rating BETWEEN 1 AND 5),
  title       VARCHAR(150),
  body        TEXT,
  created_at  TIMESTAMPTZ  NOT NULL DEFAULT NOW(),
  UNIQUE (user_id, product_id)   -- one review per user per product
);

CREATE INDEX idx_reviews_product ON reviews(product_id);
```

---

## Entity Relationship Diagram

```
┌──────────┐         ┌──────────┐
│   User   │ 1─────1 │ Profile  │
│──────────│         │──────────│
│ id (PK)  │         │ id (PK)  │
│ email    │         │ user_id  │
│ role     │         │ name     │
└──┬───────┘         └──────────┘
   │ 1
   │
   ├──────────────────────────────────────────────┐
   │ 1..N                                         │ 1..N
   ▼                                              ▼
┌──────────┐   1..N  ┌─────────────┐     ┌──────────────┐
│  Order   │─────────│  OrderItem  │     │   Review     │
│──────────│         │─────────────│     │──────────────│
│ id (PK)  │         │ id (PK)     │     │ id (PK)      │
│ user_id  │         │ order_id    │     │ user_id      │
│ status   │         │ product_id  │     │ product_id   │
│ total    │         │ quantity    │     │ rating 1–5   │
└──────────┘         │ unit_price  │     └──────┬───────┘
                     └──────┬──────┘            │ N
                            │ N                 │
                            │              ┌────┴─────┐    N..N  ┌──────────┐
                            └──────────────│  Product │──────────│   Tag    │
                                           │──────────│          │──────────│
                                           │ id (PK)  │          │ id (PK)  │
                                           │ name     │          │ name     │
                                           │ price    │          └──────────┘
                                           │ stock    │
                                           │ cat_id   │──── N:1 ──── Category
                                           └──────────┘
```

---

## REST API Specification

> **Base URL:** `https://api.yourdomain.com/v1`  
> **Auth Header:** `Authorization: Bearer <token>`

---

### Auth Endpoints

#### `POST /auth/register`

Register a new user account.

**Request Body**

```json
{
  "email": "user@example.com",
  "password": "S3cur3P@ss!",
  "first_name": "Jane",
  "last_name": "Doe"
}
```

**Response `201 Created`**

```json
{
  "id": "uuid",
  "email": "user@example.com",
  "role": "customer",
  "created_at": "2026-05-30T10:00:00Z"
}
```

---

#### `POST /auth/login`

Authenticate and receive a JWT token.

**Request Body**

```json
{
  "email": "user@example.com",
  "password": "S3cur3P@ss!"
}
```

**Response `200 OK`**

```json
{
  "access_token": "eyJhbGci...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

---

#### `POST /auth/logout`

Invalidate the current session token.

**Auth required:** Yes  
**Response:** `204 No Content`

---

### User Endpoints

#### `GET /users/me`

Retrieve the authenticated user's profile.

**Auth required:** Yes  
**Response `200 OK`**

```json
{
  "id": "uuid",
  "email": "user@example.com",
  "role": "customer",
  "profile": {
    "first_name": "Jane",
    "last_name": "Doe",
    "phone": "+61400000000",
    "avatar_url": "https://cdn.example.com/avatars/uuid.jpg"
  }
}
```

---

#### `PATCH /users/me`

Update the authenticated user's profile.

**Auth required:** Yes

**Request Body** *(all fields optional)*

```json
{
  "first_name": "Jane",
  "last_name": "Smith",
  "phone": "+61400000001",
  "bio": "Updated bio text."
}
```

**Response `200 OK`** — returns updated profile object.

---

### Product Endpoints

#### `GET /products`

List all published products with pagination and filtering.

**Auth required:** No

**Query Parameters**

| Parameter    | Type    | Description                          | Default |
|--------------|---------|--------------------------------------|---------|
| `page`       | integer | Page number                          | `1`     |
| `limit`      | integer | Items per page (max 100)             | `20`    |
| `category`   | string  | Filter by category slug              | —       |
| `tag`        | string  | Filter by tag name                   | —       |
| `min_price`  | number  | Minimum price filter                 | —       |
| `max_price`  | number  | Maximum price filter                 | —       |
| `sort`       | string  | `price_asc`, `price_desc`, `newest`  | `newest`|
| `search`     | string  | Full-text search on name/description | —       |

**Response `200 OK`**

```json
{
  "data": [
    {
      "id": "uuid",
      "name": "Product Name",
      "slug": "product-name",
      "price": 29.99,
      "stock": 50,
      "category": { "id": "uuid", "name": "Electronics" },
      "tags": ["sale", "featured"],
      "average_rating": 4.3,
      "review_count": 12
    }
  ],
  "meta": {
    "total": 120,
    "page": 1,
    "limit": 20,
    "total_pages": 6
  }
}
```

---

#### `GET /products/:id`

Retrieve a single product by ID.

**Auth required:** No  
**Response `200 OK`** — full product object including reviews summary.

---

#### `POST /products` *(Admin only)*

Create a new product.

**Auth required:** Yes (role: `admin`)

**Request Body**

```json
{
  "name": "New Product",
  "description": "Product description.",
  "price": 49.99,
  "stock": 100,
  "category_id": "uuid",
  "tags": ["new", "featured"],
  "is_published": false
}
```

**Response `201 Created`** — full product object.

---

#### `PATCH /products/:id` *(Admin only)*

Partially update a product.

**Auth required:** Yes (role: `admin`)  
**Request Body** — any subset of product fields.  
**Response `200 OK`** — updated product object.

---

#### `DELETE /products/:id` *(Admin only)*

Soft-delete a product (sets `is_published = false`).

**Auth required:** Yes (role: `admin`)  
**Response:** `204 No Content`

---

### Order Endpoints

#### `GET /orders`

List orders for the authenticated user.

**Auth required:** Yes

**Query Parameters**

| Parameter | Type    | Description                                              | Default |
|-----------|---------|----------------------------------------------------------|---------|
| `status`  | string  | Filter by status (`pending`, `confirmed`, etc.)          | —       |
| `page`    | integer | Page number                                              | `1`     |
| `limit`   | integer | Items per page                                           | `10`    |

**Response `200 OK`**

```json
{
  "data": [
    {
      "id": "uuid",
      "status": "confirmed",
      "total_amount": 89.97,
      "item_count": 3,
      "created_at": "2026-05-30T09:00:00Z"
    }
  ],
  "meta": { "total": 5, "page": 1, "limit": 10, "total_pages": 1 }
}
```

---

#### `GET /orders/:id`

Retrieve a single order with all line items.

**Auth required:** Yes (owner or admin)

**Response `200 OK`**

```json
{
  "id": "uuid",
  "status": "confirmed",
  "total_amount": 89.97,
  "notes": "Leave at door",
  "items": [
    {
      "product_id": "uuid",
      "product_name": "Product Name",
      "quantity": 2,
      "unit_price": 29.99,
      "subtotal": 59.98
    }
  ],
  "created_at": "2026-05-30T09:00:00Z"
}
```

---

#### `POST /orders`

Create a new order.

**Auth required:** Yes

**Request Body**

```json
{
  "items": [
    { "product_id": "uuid", "quantity": 2 },
    { "product_id": "uuid", "quantity": 1 }
  ],
  "notes": "Leave at door"
}
```

**Response `201 Created`** — full order object.

---

#### `PATCH /orders/:id/status` *(Admin only)*

Update order status.

**Auth required:** Yes (role: `admin`)

**Request Body**

```json
{ "status": "shipped" }
```

**Response `200 OK`** — updated order object.

---

#### `DELETE /orders/:id`

Cancel a pending order.

**Auth required:** Yes (owner only, status must be `pending`)  
**Response:** `204 No Content`

---

### Review Endpoints

#### `GET /products/:id/reviews`

List all reviews for a product.

**Auth required:** No

**Query Parameters**

| Parameter | Type    | Description             | Default  |
|-----------|---------|-------------------------|----------|
| `page`    | integer | Page number             | `1`      |
| `limit`   | integer | Reviews per page        | `10`     |
| `sort`    | string  | `newest` or `top_rated` | `newest` |

**Response `200 OK`**

```json
{
  "data": [
    {
      "id": "uuid",
      "user": { "id": "uuid", "first_name": "Jane" },
      "rating": 5,
      "title": "Excellent product",
      "body": "Really happy with this purchase.",
      "created_at": "2026-05-28T08:00:00Z"
    }
  ],
  "meta": { "total": 12, "average_rating": 4.3 }
}
```

---

#### `POST /products/:id/reviews`

Submit a review for a product.

**Auth required:** Yes (must have purchased the product)

**Request Body**

```json
{
  "rating": 5,
  "title": "Excellent product",
  "body": "Really happy with this purchase."
}
```

**Response `201 Created`** — full review object.

---

#### `DELETE /reviews/:id`

Delete own review.

**Auth required:** Yes (owner or admin)  
**Response:** `204 No Content`

---

## Validation Rules

### User / Auth

| Field        | Rules                                                                 |
|--------------|-----------------------------------------------------------------------|
| `email`      | Required · Valid email format · Max 255 chars · Unique                |
| `password`   | Required · Min 8 chars · ≥1 uppercase · ≥1 digit · ≥1 special char  |
| `first_name` | Optional · Max 100 chars · Letters, hyphens, spaces only             |
| `last_name`  | Optional · Max 100 chars · Letters, hyphens, spaces only             |
| `phone`      | Optional · E.164 format (e.g. `+61400000000`)                        |

---

### Product

| Field         | Rules                                                   |
|---------------|---------------------------------------------------------|
| `name`        | Required · Min 2 chars · Max 255 chars                  |
| `price`       | Required · Numeric · ≥ 0 · Max 2 decimal places         |
| `stock`       | Required · Integer · ≥ 0                                |
| `category_id` | Optional · Must reference valid category UUID            |
| `description` | Optional · Max 5000 chars                               |
| `tags`        | Optional · Array of strings · Each tag max 50 chars     |
| `is_published`| Optional · Boolean · Default `false`                    |

---

### Order

| Field        | Rules                                                          |
|--------------|----------------------------------------------------------------|
| `items`      | Required · Non-empty array · Min 1 item                        |
| `product_id` | Required per item · Valid UUID · Product must be published      |
| `quantity`   | Required per item · Integer · ≥ 1 · ≤ available stock          |
| `notes`      | Optional · Max 500 chars                                       |
| `status`     | Required for updates · One of allowed enum values              |

---

### Review

| Field    | Rules                                          |
|----------|------------------------------------------------|
| `rating` | Required · Integer · Between 1 and 5           |
| `title`  | Optional · Max 150 chars                       |
| `body`   | Optional · Max 2000 chars                      |
| Unique   | One review per user per product (enforced in DB)|

---

## Error Handling

All errors follow a consistent JSON structure:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed.",
    "details": [
      { "field": "email", "message": "Must be a valid email address." },
      { "field": "password", "message": "Must be at least 8 characters." }
    ]
  }
}
```

### Standard HTTP Status Codes

| Code  | Meaning                  | When Used                                              |
|-------|--------------------------|--------------------------------------------------------|
| `200` | OK                       | Successful GET / PATCH                                 |
| `201` | Created                  | Successful POST (resource created)                     |
| `204` | No Content               | Successful DELETE                                      |
| `400` | Bad Request              | Malformed JSON or missing required fields              |
| `401` | Unauthorized             | Missing or invalid JWT token                           |
| `403` | Forbidden                | Authenticated but insufficient role/permission         |
| `404` | Not Found                | Resource does not exist                                |
| `409` | Conflict                 | Duplicate record (e.g. email already registered)       |
| `422` | Unprocessable Entity     | Validation rules failed (see `details`)                |
| `429` | Too Many Requests        | Rate limit exceeded                                    |
| `500` | Internal Server Error    | Unexpected server error                                |

### Error Code Reference

| `code`                | Description                                   |
|-----------------------|-----------------------------------------------|
| `VALIDATION_ERROR`    | One or more fields failed validation          |
| `UNAUTHORIZED`        | Authentication required                       |
| `FORBIDDEN`           | Action not permitted for this role            |
| `NOT_FOUND`           | Requested resource not found                  |
| `DUPLICATE_ENTRY`     | Unique constraint violation                   |
| `INSUFFICIENT_STOCK`  | Order quantity exceeds available product stock |
| `RATE_LIMITED`        | Too many requests — try again later           |
| `INTERNAL_ERROR`      | Unexpected server-side failure                |

---

## Glossary

| Term          | Definition                                                                 |
|---------------|----------------------------------------------------------------------------|
| **UUID**      | Universally Unique Identifier — used as primary key for all entities        |
| **JWT**       | JSON Web Token — stateless auth token returned on login                    |
| **Slug**      | URL-friendly version of a name (e.g. `my-product-name`)                   |
| **Soft Delete**| Marking a record as inactive rather than removing it from the database    |
| **TIMESTAMPTZ**| PostgreSQL timezone-aware timestamp (stored in UTC)                      |
| **E.164**     | International phone number format standard (e.g. `+61400000000`)          |

---

*Week 5 Deliverable · Last updated: May 2026 · Maintained by the engineering team*

