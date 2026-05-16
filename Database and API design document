# Week 5 – Database & API Design

## What is this week about?

Designing the **data model** (database schema) and **API endpoints** that will power the Aussie Realestate Website.

---

## Key Questions Answered

| Question | Answer |
|----------|--------|
| **What entities exist?** | Users, Listings, Listing Images, Saved Listings (Favourites), Listing Views (Analytics) |
| **What relationships exist?** | One-to-many (User to Listings, Listing to Images), Many-to-many (User to Favourites) |
| **What APIs are required?** | Auth APIs, Listing CRUD APIs, Search/Filter API, Dashboard APIs |

---

## Activities Done

| Activity | What We Did |
|----------|-------------|
| Define database schema | Created 5 tables with columns, data types, keys, and constraints |
| Define relationships | Mapped foreign keys and cardinality (1:N, M:N) |
| Define REST APIs | Designed 16 endpoints with methods, paths, request/response formats |
| Define validation | Set validation rules for all inputs (email, password, price, etc.) |

---

## 1. Database Schema Design

### Entity Relationship Diagram (ERD)

```mermaid
erDiagram
    users {
        SERIAL id PK
        VARCHAR email UK
        VARCHAR password_hash
        VARCHAR full_name
        VARCHAR role
        VARCHAR phone
        TEXT profile_picture
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    listings {
        SERIAL id PK
        INT agent_id FK
        VARCHAR title
        TEXT description
        DECIMAL price
        VARCHAR address
        VARCHAR suburb
        VARCHAR postcode
        DECIMAL lat
        DECIMAL lng
        INT bedrooms
        DECIMAL bathrooms
        INT car_spaces
        VARCHAR property_type
        VARCHAR status
        INT view_count
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    listing_images {
        SERIAL id PK
        INT listing_id FK
        VARCHAR image_url
        INT display_order
        TIMESTAMP created_at
    }

    saved_listings {
        SERIAL id PK
        INT user_id FK
        INT listing_id FK
        TIMESTAMP saved_at
    }

    listing_views {
        SERIAL id PK
        INT listing_id FK
        VARCHAR viewer_ip_hash
        TIMESTAMP viewed_at
    }

    users ||--o{ listings : "has many (as agent)"
    listings ||--o{ listing_images : "has many"
    users ||--o{ saved_listings : "saves"
    listings ||--o{ saved_listings : "saved by"
    listings ||--o{ listing_views : "has many views"
