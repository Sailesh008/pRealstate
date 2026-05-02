
# Week 4 Deliverable: System Architecture Document
## Aussie Realestate Website – WIL Project


## 1. Architecture Overview

The Aussie Realestate Website will be built as a **monolithic web application** with a clear separation of concerns using a **layered architecture**. This approach is chosen for simplicity, ease of deployment, and suitability for a 12-week student project with a team of 3-6 members.

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Application Style | Monolith | Easier to develop, debug, and deploy as a small team |
| Architecture Pattern | Layered (3-tier) | Clear separation: Presentation, Business Logic, Data |
| Deployment | Docker container | Consistent environment, simple cloud deployment |



## 2. Technology Stack

### Frontend
| Component | Technology | Purpose |
|-----------|------------|---------|
| Framework | React.js | Responsive UI components |
| State Management | React Context API | User auth state, filter state |
| HTTP Client | Axios | API calls to backend |
| Map Integration | Mapbox GL JS | Interactive property maps |
| Styling | Tailwind CSS | Responsive, utility-first styling |

### Backend
| Component | Technology | Purpose |
|-----------|------------|---------|
| Runtime | Node.js | JavaScript backend |
| Framework | Express.js | REST API routing |
| Authentication | JWT + bcrypt | Token-based auth, password hashing |
| Validation | Joi | Request body validation |

### Database
| Component | Technology | Purpose |
|-----------|------------|---------|
| Database | PostgreSQL 14+ | Relational data storage |
| ORM/Query Builder | Knex.js | SQL query building, migrations |

### DevOps & Deployment
| Component | Technology | Purpose |
|-----------|------------|---------|
| Containerization | Docker | Consistent runtime |
| Cloud Platform | AWS EC2 / Azure VM | Public cloud hosting |
| Version Control | GitHub | Source code management |
| API Documentation | Swagger/OpenAPI | Endpoint documentation |



## 3. System Architecture Diagram
<img width="6061" height="3069" alt="deepseek_mermaid_20260418_5c6ff0" src="https://github.com/user-attachments/assets/3ae7b85c-8643-4511-a6de-4f49bbf52ddb" />




## 4. Architecture Decision: Monolith vs Microservices

| Criteria | Monolith (Chosen) | Microservices |
|----------|-------------------|---------------|
| Team Size | 3-6 students ✅ | Requires larger teams |
| Deployment Complexity | Low ✅ | High |
| Development Speed | Fast ✅ | Slower |
| Scalability | Sufficient for prototype | Overkill |
| Learning Curve | Gentle ✅ | Steep |

**Decision:** Monolith with modular internal structure. Each service (Auth, Listings, Users, Dashboard) is a separate module within the same codebase, allowing potential future extraction if needed.



## 5. Authentication Flow


┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  Client  │     │  Server  │     │   DB     │     │  JWT     │
└────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │                │                │
     │ POST /login    │                │                │
     │ {email, pwd}   │                │                │
     │───────────────>│                │                │
     │                │ SELECT user    │                │
     │                │───────────────>│                │
     │                │                │                │
     │                │ user record    │                │
     │                │<───────────────│                │
     │                │                │                │
     │                │ bcrypt compare │                │
     │                │───────────────>│                │
     │                │                │                │
     │                │ generate JWT   │                │
     │                │───────────────────────────────>│
     │                │                │                │
     │                │                │      JWT token │
     │                │<───────────────────────────────│
     │                │                │                │
     │ {token, user}  │                │                │
     │<───────────────│                │                │
     │                │                │                │
     │ Store token    │                │                │
     │ in HTTP-only   │                │                │
     │ cookie         │                │                │
     │                │                │                │


## 4. Architecture Decision: Monolith vs Microservices

| Criteria | Monolith (Chosen) | Microservices |
|----------|-------------------|---------------|
| Team Size | 3-6 students ✅ | Requires larger teams |
| Deployment Complexity | Low ✅ | High |
| Development Speed | Fast ✅ | Slower |
| Scalability | Sufficient for prototype | Overkill |
| Learning Curve | Gentle ✅ | Steep |

**Decision:** Monolith with modular internal structure. Each service (Auth, Listings, Users, Dashboard) is a separate module within the same codebase, allowing potential future extraction if needed.



## 5. Authentication Flow

<img width="3278" height="5012" alt="deepseek_mermaid_20260418_a34744" src="https://github.com/user-attachments/assets/f1367497-0fd4-4608-8cc8-ab8dc326118b" />

### Authentication Strategy

| Component | Implementation |
|-----------|----------------|
| Token Type | JWT (JSON Web Token) |
| Expiry | 24 hours |
| Storage | HTTP-only cookie (prevents XSS) |
| Password Hashing | bcrypt (salt rounds = 10) |
| Protected Routes | JWT verification middleware |



## 6. Data Flow – Search Properties

'<img width="3704" height="854" alt="deepseek_mermaid_20260418_fb808c" src="https://github.com/user-attachments/assets/7e8ebdc6-4131-4940-b44e-8f9d5827fd8d" />




## 7. Component Breakdown

### 7.1 Frontend Components (React)

| Component | Responsibility |
|-----------|---------------|
| `LoginForm` | Email/password input, authentication |
| `RegisterForm` | New user registration |
| `SearchBar` | Free-text search input |
| `FilterPanel` | Price, bedrooms, property type filters |
| `ListingsGrid` | Displays property cards in responsive grid |
| `ListingCard` | Individual property summary (image, price, address) |
| `MapView` | Interactive map with property markers |
| `ListingDetail` | Full property details with image gallery |
| `DashboardAgent` | Agent's listing management and view counts |
| `DashboardAdmin` | Usage analytics charts |
| `Navbar` | Navigation, logout, role-based menu |

### 7.2 Backend Services (Express)

| Service | Responsibility |
|---------|---------------|
| `AuthService` | Registration, login, logout, password reset |
| `ListingService` | CRUD operations for properties, search, filtering |
| `UserService` | Profile management, role assignment |
| `AnalyticsService` | View tracking, dashboard metrics |
| `GeoService` | Geocoding, map marker aggregation |

### 7.3 Database Tables

| Table | Columns |
|-------|---------|
| `users` | id, email, password_hash, full_name, role, created_at |
| `listings` | id, agent_id, title, description, price, address, lat, lng, bedrooms, bathrooms, car_spaces, property_type, status, created_at |
| `listing_images` | id, listing_id, image_url, display_order |
| `listing_views` | id, listing_id, viewed_at, viewer_ip_hash |



## 8. API Design Overview

| Method | Endpoint | Service | Auth |
|--------|----------|---------|------|
| POST | `/api/auth/register` | AuthService | None |
| POST | `/api/auth/login` | AuthService | None |
| POST | `/api/auth/logout` | AuthService | Required |
| GET | `/api/listings` | ListingService | None |
| GET | `/api/listings/:id` | ListingService | None |
| POST | `/api/listings` | ListingService | Agent |
| PUT | `/api/listings/:id` | ListingService | Agent/Admin |
| DELETE | `/api/listings/:id` | ListingService | Agent/Admin |
| GET | `/api/dashboard/agent` | AnalyticsService | Agent |
| GET | `/api/dashboard/admin` | AnalyticsService | Admin |



## 9. Security Architecture

| Layer | Measure |
|-------|---------|
| Transport | HTTPS only |
| Authentication | JWT in HTTP-only cookies |
| Password Storage | bcrypt (10 salt rounds) |
| Input Validation | Joi schema validation |
| SQL Injection | Parameterized queries (Knex.js) |
| XSS Protection | React escaping, CSP headers |
| CORS | Restricted to frontend origin |
| Rate Limiting | express-rate-limit (100 req/min) |



## 10. Deployment Architecture

<img width="3542" height="653" alt="deepseek_mermaid_20260418_152ae7" src="https://github.com/user-attachments/assets/e7e6ecda-b62f-42d5-ad50-ebb44324f1a0" />


## 11. Deliverable Checklist for Week 4

- [x] Tech stack chosen and documented
- [x] Architecture type defined (Monolith + Layered)
- [x] Authentication flow designed (JWT + HTTP-only cookie)
- [x] Data flow diagram created (Search flow)
- [x] Component breakdown completed (Frontend, Backend, Database)
- [x] API endpoints outlined
- [x] Security measures defined
- [x] Deployment architecture designed

