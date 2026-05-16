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
Migrations Executed
Migration	Table	Status
001_create_users	users	✅ Executed
002_create_listings	listings	✅ Executed
003_create_listing_images	listing_images	✅ Executed
004_create_saved_listings	saved_listings	✅ Executed
005_create_listing_views	listing_views	✅ Executed
Indexes Created
sql
-- Performance indexes
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_listings_agent_id ON listings(agent_id);
CREATE INDEX idx_listings_status ON listings(status);
CREATE INDEX idx_listings_price ON listings(price);
CREATE INDEX idx_listings_suburb ON listings(suburb);
CREATE INDEX idx_listings_property_type ON listings(property_type);
4. Environment Variables
env
# Server
PORT=5000
NODE_ENV=development

# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=aussie_realestate
DB_USER=postgres
DB_PASSWORD=yourpassword

# JWT
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRY=24h

# Mapbox
MAPBOX_ACCESS_TOKEN=your_mapbox_token

# Cloud Storage (for images)
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
5. Folder Structure
text
backend/
├── config/
│   ├── database.js
│   └── cloudinary.js
├── controllers/
│   ├── authController.js
│   ├── listingController.js
│   ├── favouriteController.js
│   └── dashboardController.js
├── middleware/
│   ├── authMiddleware.js
│   ├── uploadMiddleware.js
│   └── errorMiddleware.js
├── models/
│   ├── User.js
│   ├── Listing.js
│   └── Favourite.js
├── routes/
│   ├── authRoutes.js
│   ├── listingRoutes.js
│   ├── favouriteRoutes.js
│   └── dashboardRoutes.js
├── services/
│   ├── authService.js
│   ├── listingService.js
│   └── geocodingService.js
├── validators/
│   ├── authValidator.js
│   └── listingValidator.js
├── utils/
│   ├── jwtHelper.js
│   └── bcryptHelper.js
├── tests/
│   ├── auth.test.js
│   └── listing.test.js
├── migrations/
├── seeds/
├── .env
├── app.js
└── server.js
6. Sample Code – Auth Controller
javascript
// controllers/authController.js
const authService = require('../services/authService');
const { validateRegister, validateLogin } = require('../validators/authValidator');

const register = async (req, res, next) => {
  try {
    // Validate input
    const { error } = validateRegister(req.body);
    if (error) {
      return res.status(400).json({ 
        success: false, 
        message: error.details[0].message 
      });
    }

    // Create user
    const { email, password, full_name, role } = req.body;
    const result = await authService.register(email, password, full_name, role);
    
    res.status(201).json({
      success: true,
      message: 'User registered successfully',
      user: result.user
    });
  } catch (err) {
    next(err);
  }
};

const login = async (req, res, next) => {
  try {
    const { error } = validateLogin(req.body);
    if (error) {
      return res.status(400).json({ 
        success: false, 
        message: error.details[0].message 
      });
    }

    const { email, password } = req.body;
    const result = await authService.login(email, password);
    
    // Set JWT as HTTP-only cookie
    res.cookie('token', result.token, {
      httpOnly: true,
      secure: process.env.NODE_ENV === 'production',
      sameSite: 'strict',
      maxAge: 24 * 60 * 60 * 1000 // 24 hours
    });

    res.json({
      success: true,
      message: 'Login successful',
      user: result.user
    });
  } catch (err) {
    next(err);
  }
};

const logout = async (req, res) => {
  res.clearCookie('token');
  res.json({ success: true, message: 'Logout successful' });
};

module.exports = { register, login, logout };
7. Sample Code – Listing Controller
javascript
// controllers/listingController.js
const listingService = require('../services/listingService');

const searchListings = async (req, res, next) => {
  try {
    const {
      keyword,
      minPrice,
      maxPrice,
      suburb,
      bedrooms,
      propertyType,
      sortBy = 'created_at',
      sortOrder = 'desc',
      page = 1,
      limit = 12
    } = req.query;

    const result = await listingService.search({
      keyword,
      minPrice,
      maxPrice,
      suburb,
      bedrooms,
      propertyType,
      sortBy,
      sortOrder,
      page: parseInt(page),
      limit: parseInt(limit)
    });

    res.json({ success: true, data: result });
  } catch (err) {
    next(err);
  }
};

const createListing = async (req, res, next) => {
  try {
    const listingData = {
      ...req.body,
      agent_id: req.user.id
    };
    
    const listing = await listingService.create(listingData);
    
    res.status(201).json({
      success: true,
      message: 'Listing created successfully',
      listing
    });
  } catch (err) {
    next(err);
  }
};

const updateListing = async (req, res, next) => {
  try {
    const { id } = req.params;
    const listing = await listingService.update(id, req.body, req.user);
    
    res.json({
      success: true,
      message: 'Listing updated successfully',
      listing
    });
  } catch (err) {
    next(err);
  }
};

const deleteListing = async (req, res, next) => {
  try {
    const { id } = req.params;
    await listingService.delete(id, req.user);
    
    res.json({
      success: true,
      message: 'Listing deleted successfully'
    });
  } catch (err) {
    next(err);
  }
};

module.exports = { searchListings, createListing, updateListing, deleteListing };
8. Search Query Implementation
javascript
// services/listingService.js - Search Logic
const search = async (filters) => {
  let query = db('listings')
    .select('listings.*', 'users.full_name as agent_name')
    .leftJoin('users', 'listings.agent_id', 'users.id')
    .where('listings.status', 'active');

  // Keyword search (title, suburb, postcode)
  if (filters.keyword) {
    query = query.where(function() {
      this.where('listings.title', 'ilike', `%${filters.keyword}%`)
        .orWhere('listings.suburb', 'ilike', `%${filters.keyword}%`)
        .orWhere('listings.postcode', 'ilike', `%${filters.keyword}%`);
    });
  }

  // Price range filter
  if (filters.minPrice) {
    query = query.where('listings.price', '>=', filters.minPrice);
  }
  if (filters.maxPrice) {
    query = query.where('listings.price', '<=', filters.maxPrice);
  }

  // Bedrooms filter
  if (filters.bedrooms) {
    query = query.where('listings.bedrooms', '>=', filters.bedrooms);
  }

  // Suburb filter
  if (filters.suburb) {
    query = query.where('listings.suburb', 'ilike', `%${filters.suburb}%`);
  }

  // Property type filter
  if (filters.propertyType) {
    query = query.where('listings.property_type', filters.propertyType);
  }

  // Sorting
  query = query.orderBy(filters.sortBy, filters.sortOrder);

  // Pagination
  const total = await query.clone().count('* as count').first();
  const listings = await query
    .limit(filters.limit)
    .offset((filters.page - 1) * filters.limit);

  return {
    listings,
    pagination: {
      page: filters.page,
      limit: filters.limit,
      total: parseInt(total.count),
      total_pages: Math.ceil(total.count / filters.limit)
    }
  };
};
9. Unit Test Results
Test Suite	Tests Passed	Coverage
Auth API	8/8	100%
Listing API	12/12	100%
Favourite API	6/6	100%
Dashboard API	4/4	100%
Validation	10/10	100%
Total	40/40	65%
Sample Test Output
text
PASS  tests/auth.test.js
  ✓ POST /api/auth/register - success (45ms)
  ✓ POST /api/auth/register - duplicate email (32ms)
  ✓ POST /api/auth/register - invalid email (28ms)
  ✓ POST /api/auth/login - success (38ms)
  ✓ POST /api/auth/login - wrong password (25ms)
  ✓ POST /api/auth/login - user not found (22ms)
  ✓ GET /api/auth/me - requires auth (30ms)
  ✓ POST /api/auth/logout - success (20ms)

Test Suites: 4 passed, 4 total
Tests:       40 passed, 40 total
Coverage:    65% statements
10. API Testing (Postman Results)
Endpoint	Method	Status	Response Time
/api/auth/register	POST	201 Created	145ms
/api/auth/login	POST	200 OK	98ms
/api/auth/me	GET	200 OK	52ms
/api/listings	GET	200 OK	112ms
/api/listings?bedrooms=2	GET	200 OK	89ms
/api/listings/101	GET	200 OK	45ms
/api/listings	POST	201 Created	167ms
/api/listings/101	PUT	200 OK	78ms
/api/listings/101	DELETE	200 OK	65ms
/api/listings/101/images	POST	201 Created	234ms
/api/favourites	GET	200 OK	56ms
/api/dashboard/agent	GET	200 OK	88ms
/api/dashboard/admin	GET	200 OK	92ms
11. Blockers / Challenges
Issue	Status	Mitigation
Mapbox API key not provisioned	🟡 In Progress	Using mock coordinates for now
Image upload size limit (10MB)	✅ Resolved	Increased to 10MB on backend
Search query performance slow	✅ Resolved	Added indexes on filtered columns
12. Backend Progress Summary
Module	Progress	Notes
Server Setup	100%	Express with middleware
Database Connection	100%	Knex.js + connection pooling
Migrations	100%	5 tables created
Authentication	100%	JWT + bcrypt + cookies
Listing CRUD	100%	Create, read, update, delete
Search & Filter	100%	Multi-criteria with pagination
Image Upload	100%	Cloudinary integration
Favourites	100%	Save/remove saved listings
Dashboard APIs	100%	Agent + admin stats
Validation	100%	Joi schemas for all endpoints
Error Handling	100%	Centralized error handler
Unit Tests	65%	Coverage target met
Geocoding	90%	Awaiting Mapbox key
13. Deliverable Checklist
Server set up (Node.js + Express)

Database connected (PostgreSQL + Knex.js)

Migrations executed (5 tables)

Authentication APIs completed (5 endpoints)

Listing CRUD APIs completed (6 endpoints)

Search and filter API completed

Favourites APIs completed (3 endpoints)

Dashboard APIs completed (2 endpoints)

Validation implemented for all inputs

Image upload implemented

Geocoding integrated (awaiting API key)

Unit tests written (65% coverage)

Postman testing completed


