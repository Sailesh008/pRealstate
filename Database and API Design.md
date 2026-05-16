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

Table Details
Table 1: users
Column	Type	Constraints	Description
id	SERIAL	PRIMARY KEY	Unique user ID
email	VARCHAR(255)	UNIQUE, NOT NULL	User email address
password_hash	VARCHAR(255)	NOT NULL	Bcrypt hashed password
full_name	VARCHAR(100)	NOT NULL	User's full name
role	VARCHAR(20)	DEFAULT 'guest'	guest / agent / admin
phone	VARCHAR(20)	NULL	Contact number
profile_picture	TEXT	NULL	Profile image URL
created_at	TIMESTAMP	DEFAULT NOW()	Account creation time
updated_at	TIMESTAMP	NULL	Last update time
Table 2: listings
Column	Type	Constraints	Description
id	SERIAL	PRIMARY KEY	Unique listing ID
agent_id	INT	FOREIGN KEY (users.id), NOT NULL	Agent who created listing
title	VARCHAR(255)	NOT NULL	Property title
description	TEXT	NULL	Detailed description
price	DECIMAL(12,2)	NOT NULL	Property price
address	VARCHAR(500)	NOT NULL	Full street address
suburb	VARCHAR(100)	NOT NULL	Suburb name
postcode	VARCHAR(10)	NOT NULL	Postcode
lat	DECIMAL(10,8)	NULL	Latitude for map
lng	DECIMAL(11,8)	NULL	Longitude for map
bedrooms	INT	DEFAULT 0	Number of bedrooms
bathrooms	DECIMAL(3,1)	DEFAULT 0	Number of bathrooms
car_spaces	INT	DEFAULT 0	Number of car spaces
property_type	VARCHAR(20)	NOT NULL	house / apartment / townhouse / land
status	VARCHAR(20)	DEFAULT 'active'	active / sold / rented
view_count	INT	DEFAULT 0	Total number of views
created_at	TIMESTAMP	DEFAULT NOW()	Listing creation time
updated_at	TIMESTAMP	NULL	Last update time
Table 3: listing_images
Column	Type	Constraints	Description
id	SERIAL	PRIMARY KEY	Unique image ID
listing_id	INT	FOREIGN KEY (listings.id), NOT NULL	Associated listing
image_url	VARCHAR(1000)	NOT NULL	Cloud storage URL
display_order	INT	DEFAULT 0	Order of image display
created_at	TIMESTAMP	DEFAULT NOW()	Upload time
Table 4: saved_listings (Favourites)
Column	Type	Constraints	Description
id	SERIAL	PRIMARY KEY	Unique favourite ID
user_id	INT	FOREIGN KEY (users.id), NOT NULL	User saving the listing
listing_id	INT	FOREIGN KEY (listings.id), NOT NULL	Listing being saved
saved_at	TIMESTAMP	DEFAULT NOW()	Time saved
Table 5: listing_views (Analytics)
Column	Type	Constraints	Description
id	SERIAL	PRIMARY KEY	Unique view ID
listing_id	INT	FOREIGN KEY (listings.id), NOT NULL	Viewed listing
viewer_ip_hash	VARCHAR(64)	NOT NULL	Anonymized IP address
viewed_at	TIMESTAMP	DEFAULT NOW()	View timestamp
Indexes for Performance
sql
-- Users
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);

-- Listings
CREATE INDEX idx_listings_agent_id ON listings(agent_id);
CREATE INDEX idx_listings_status ON listings(status);
CREATE INDEX idx_listings_price ON listings(price);
CREATE INDEX idx_listings_suburb ON listings(suburb);
CREATE INDEX idx_listings_postcode ON listings(postcode);
CREATE INDEX idx_listings_property_type ON listings(property_type);
CREATE INDEX idx_listings_bedrooms ON listings(bedrooms);
CREATE INDEX idx_listings_created_at ON listings(created_at);

-- Listing Images
CREATE INDEX idx_listing_images_listing_id ON listing_images(listing_id);

-- Saved Listings
CREATE INDEX idx_saved_listings_user_id ON saved_listings(user_id);
CREATE INDEX idx_saved_listings_listing_id ON saved_listings(listing_id);

-- Listing Views
CREATE INDEX idx_listing_views_listing_id ON listing_views(listing_id);
CREATE INDEX idx_listing_views_viewed_at ON listing_views(viewed_at);
2. API Design
API Base URL
text
https://api.aussierealestate.com/v1
Authentication
All protected endpoints require a JWT token sent as an HTTP-only cookie.

API Endpoints Summary
Method	Endpoint	Description	Auth
POST	/auth/register	Register new user	None
POST	/auth/login	Login and get JWT	None
POST	/auth/logout	Logout and clear cookie	Required
GET	/auth/me	Get current user profile	Required
PUT	/auth/me	Update user profile	Required
GET	/listings	Search and filter listings	None
GET	/listings/:id	Get single listing details	None
POST	/listings	Create new listing	Agent
PUT	/listings/:id	Update listing	Agent/Admin
DELETE	/listings/:id	Delete listing	Agent/Admin
POST	/listings/:id/images	Upload image to listing	Agent
DELETE	/listings/:id/images/:imageId	Delete listing image	Agent
GET	/favourites	Get user's saved listings	Required
POST	/favourites/:listingId	Save listing to favourites	Required
DELETE	/favourites/:listingId	Remove from favourites	Required
GET	/dashboard/agent	Agent dashboard stats	Agent
GET	/dashboard/admin	Admin dashboard stats	Admin
Detailed API Specifications
1. Register User
http
POST /auth/register
Request Body:

json
{
    "email": "john@example.com",
    "password": "SecurePass123",
    "full_name": "John Doe",
    "role": "agent"
}
Validation Rules:

Field	Rule
email	Required, valid email format, max 255 chars
password	Required, min 8 chars, at least 1 number
full_name	Required, min 2 chars, max 100 chars
role	Optional, must be 'guest', 'agent', or 'admin'
Response (201 Created):

json
{
    "success": true,
    "message": "User registered successfully",
    "user": {
        "id": 1,
        "email": "john@example.com",
        "full_name": "John Doe",
        "role": "agent",
        "created_at": "2026-05-16T10:00:00Z"
    }
}
2. Login
http
POST /auth/login
Request Body:

json
{
    "email": "john@example.com",
    "password": "SecurePass123"
}
Response (200 OK):

json
{
    "success": true,
    "message": "Login successful",
    "user": {
        "id": 1,
        "email": "john@example.com",
        "full_name": "John Doe",
        "role": "agent"
    }
}
Cookie Set: token=<JWT>; HttpOnly; Secure; SameSite=Strict; Max-Age=86400

3. Search Listings
http
GET /listings?keyword=Surry&minPrice=500000&maxPrice=1000000&bedrooms=2&propertyType=apartment&sortBy=price&sortOrder=asc&page=1&limit=12
Query Parameters:

Parameter	Type	Required	Description
keyword	string	No	Search in title, suburb, postcode
minPrice	decimal	No	Minimum price
maxPrice	decimal	No	Maximum price
suburb	string	No	Suburb name
bedrooms	int	No	Minimum bedrooms
propertyType	string	No	house / apartment / townhouse / land
sortBy	string	No	price / created_at
sortOrder	string	No	asc / desc
page	int	No	Page number (default 1)
limit	int	No	Items per page (default 12, max 50)
Response (200 OK):

json
{
    "success": true,
    "data": {
        "listings": [
            {
                "id": 101,
                "title": "Modern Apartment in Surry Hills",
                "price": 750000,
                "address": "123 Main Street, Surry Hills NSW 2010",
                "suburb": "Surry Hills",
                "bedrooms": 2,
                "bathrooms": 1,
                "property_type": "apartment",
                "images": ["https://cdn.cloud.com/img1.jpg"],
                "created_at": "2026-05-15T10:00:00Z"
            }
        ],
        "pagination": {
            "page": 1,
            "limit": 12,
            "total": 45,
            "total_pages": 4
        }
    }
}
4. Create Listing
http
POST /listings
Request Body:

json
{
    "title": "Beautiful Family Home",
    "description": "3 bedroom house with large backyard",
    "price": 850000,
    "address": "456 Oak Street, Parramatta NSW 2150",
    "bedrooms": 3,
    "bathrooms": 2,
    "car_spaces": 2,
    "property_type": "house"
}
Validation Rules:

Field	Rule
title	Required, min 5 chars, max 255 chars
description	Optional, max 5000 chars
price	Required, must be > 0
address	Required, max 500 chars
bedrooms	Required, must be >= 0
bathrooms	Required, must be >= 0
car_spaces	Required, must be >= 0
property_type	Required, must be valid type
Response (201 Created):

json
{
    "success": true,
    "message": "Listing created successfully",
    "listing": {
        "id": 101,
        "title": "Beautiful Family Home",
        "price": 850000,
        "address": "456 Oak Street, Parramatta NSW 2150",
        "lat": -33.8170,
        "lng": 151.0035,
        "status": "active",
        "created_at": "2026-05-16T10:00:00Z"
    }
}
5. Update Listing
http
PUT /listings/:id
Request Body: (all fields optional)

json
{
    "title": "Updated Title",
    "price": 875000,
    "status": "sold"
}
Response (200 OK):

json
{
    "success": true,
    "message": "Listing updated successfully",
    "listing": {
        "id": 101,
        "title": "Updated Title",
        "price": 875000,
        "status": "sold",
        "updated_at": "2026-05-16T11:00:00Z"
    }
}
6. Delete Listing
http
DELETE /listings/:id
Response (200 OK):

json
{
    "success": true,
    "message": "Listing deleted successfully"
}
7. Upload Listing Image
http
POST /listings/:id/images
Request: multipart/form-data with file field image

Constraints:

Max file size: 10MB

Allowed formats: JPEG, PNG, WebP

Max 5 images per listing

Response (201 Created):

json
{
    "success": true,
    "message": "Image uploaded successfully",
    "image": {
        "id": 501,
        "url": "https://cdn.cloud.com/listings/101/image1.jpg",
        "display_order": 1
    }
}
8. Get Favourites
http
GET /favourites
Response (200 OK):

json
{
    "success": true,
    "data": {
        "favourites": [
            {
                "id": 201,
                "listing": {
                    "id": 101,
                    "title": "Beautiful Family Home",
                    "price": 850000,
                    "suburb": "Parramatta"
                },
                "saved_at": "2026-05-16T09:00:00Z"
            }
        ]
    }
}
9. Save Listing to Favourites
http
POST /favourites/:listingId
Response (201 Created):

json
{
    "success": true,
    "message": "Listing saved to favourites"
}
10. Remove from Favourites
http
DELETE /favourites/:listingId
Response (200 OK):

json
{
    "success": true,
    "message": "Listing removed from favourites"
}
11. Agent Dashboard
http
GET /dashboard/agent
Response (200 OK):

json
{
    "success": true,
    "data": {
        "total_listings": 12,
        "active_listings": 8,
        "sold_listings": 3,
        "rented_listings": 1,
        "total_views": 2450,
        "recent_listings": [
            {
                "id": 101,
                "title": "Modern Apartment",
                "views": 320,
                "created_at": "2026-05-10T00:00:00Z"
            }
        ]
    }
}
12. Admin Dashboard
http
GET /dashboard/admin
Response (200 OK):

json
{
    "success": true,
    "data": {
        "total_users": 150,
        "total_agents": 25,
        "total_listings": 320,
        "active_listings": 280,
        "total_views": 18750,
        "weekly_trend": {
            "users_registered": [5, 8, 12, 7, 10, 15, 9],
            "listings_created": [10, 12, 8, 14, 11, 13, 16]
        }
    }
}
3. HTTP Status Codes
Status Code	Meaning	When Used
200	OK	Successful GET, PUT, DELETE requests
201	Created	Successful POST requests
400	Bad Request	Invalid input, validation failed
401	Unauthorized	Missing or invalid JWT token
403	Forbidden	Authenticated but not authorised
404	Not Found	Resource does not exist
409	Conflict	Duplicate email, etc.
500	Internal Server Error	Server-side error
4. Error Response Format
json
{
    "success": false,
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "Invalid input provided",
        "details": [
            {
                "field": "email",
                "message": "Email is required"
            },
            {
                "field": "password",
                "message": "Password must be at least 8 characters"
            }
        ]
    }
}
5. Validation Rules Summary
Field	Validation
email	Required, valid format, max 255
password	Required, min 8 chars, at least 1 number
full_name	Required, min 2 chars, max 100
title	Required, min 5 chars, max 255
price	Required, > 0
address	Required, max 500
bedrooms	Required, >= 0
bathrooms	Required, >= 0
property_type	Required, must be valid type
image	Max 10MB, JPEG/PNG/WebP
6. Deliverable Checklist
Database schema designed (5 tables)

Entity relationships defined (1:N and M:N)

Indexes for performance defined

REST API endpoints designed (16 endpoints)

Request/response formats documented

Validation rules defined

HTTP status codes documented

Error response format defined

Next Step
Proceed to Weeks 6-7 – Backend Development to implement the database schema and API endpoints.
