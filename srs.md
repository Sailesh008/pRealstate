
# Software Requirements Specification (SRS)
## Aussie Realestate Website – WIL Project

**Project Type:** Web Application (Full Stack)  
**Project Duration:** 12 Weeks  
**Team Size:** 3-6 Students  
**Industry Sponsor:** Skillup Labs  
**Contact:** Nabin Singh – wil@skilluplabs.com.au  
**Version:** 1.0  
**Date:** April 2026

---

## Table of Contents

1. Introduction
   - 1.1 Purpose
   - 1.2 Scope
   - 1.3 Definitions and Acronyms
   - 1.4 References

2. Overall Description
   - 2.1 Product Perspective
   - 2.2 User Characteristics
   - 2.3 Operating Environment
   - 2.4 Design and Implementation Constraints
   - 2.5 Assumptions and Dependencies

3. Functional Requirements
   - 3.1 User Management and Authentication
   - 3.2 Property Listings Management
   - 3.3 Search and Filtering
   - 3.4 Interactive Map
   - 3.5 Dashboard and Analytics
   - 3.6 General Requirements

4. Non-Functional Requirements
   - 4.1 Performance
   - 4.2 Security
   - 4.3 Usability
   - 4.4 Availability and Reliability
   - 4.5 Maintainability
   - 4.6 Portability

5. Required Diagrams
   - 5.1 Use Case Diagram
   - 5.2 System Architecture Diagram
   - 5.3 Database ER Diagram
   - 5.4 Sequence Diagram
   - 5.5 Component Diagram

6. API Endpoints Design

7. Traceability Matrix

8. Acceptance Criteria

9. Appendices
   - 9.1 Assumptions for Students
   - 9.2 Risk Register

---

## 1. Introduction

### 1.1 Purpose

This Software Requirements Specification (SRS) document defines the complete functional and non-functional requirements for the **Aussie Realestate Website**, a full-stack web platform tailored to the Australian property market. This document is intended for the student development team (3-6 members), the industry sponsor (Skillup Labs), project evaluators, and any stakeholders involved in the development and assessment of the project.

### 1.2 Scope

The Aussie Realestate Website is a responsive web application that allows users to browse, search, and filter property listings with rich details and interactive map visualisation. Property owners and agents can manage their own listings. Administrators can view basic usage analytics. The system will be deployed as a working prototype on a public cloud platform using Docker containers.

### 1.3 Definitions and Acronyms

| Term | Definition |
|------|------------|
| PropTech | Property Technology – technology solutions for the real estate industry |
| CRUD | Create, Read, Update, Delete – basic database operations |
| JWT | JSON Web Token – standard for secure authentication |
| SRS | Software Requirements Specification |
| MVP | Minimum Viable Product |
| REST | Representational State Transfer – API architectural style |
| SDLC | Software Development Life Cycle |
| WCAG | Web Content Accessibility Guidelines |

### 1.4 References

| Reference | Source |
|-----------|--------|
| Project Proposal | Skillup Labs – Aussie Realestate Website WIL Project Proposal |
| Industry Context | Australian real estate digital platforms (Domain, realestate.com.au) |
| Munawar, HS, Hosseini, M, Haddad, AN, Kianmehr, A & Tariq, MA 2022, 'Using real-time dashboards to monitor the impact of disruptive events on real estate market: Case of COVID-19 pandemic in Australia', *Real Estate Economics*, vol. 2, no. 1, p. 14 |
| Pressman, RS 2014, *Software Engineering: A Practitioner's Approach*, 8th edn, McGraw-Hill, New York, pp. 120-156 |

---

## 2. Overall Description

### 2.1 Product Perspective

The Aussie Realestate Website is a new standalone system that integrates with the following external services:

| External Service | Purpose |
|------------------|---------|
| Mapbox or Google Maps API | Geospatial data and interactive maps |
| Cloud Platform (AWS/Azure) | Deployment hosting |
| Email Service (optional) | Registration confirmation and password reset |

### 2.2 User Characteristics

| User Type | Description | Technical Level | Frequency of Use |
|-----------|-------------|-----------------|------------------|
| Guest | Unauthenticated browser | Low | Occasional |
| Registered User | Can save searches and contact agents | Medium | Weekly |
| Property Owner/Agent | Can list and manage properties | Medium | Daily |
| Administrator | System analytics and user management | High | Daily |

### 2.3 Operating Environment

| Component | Environment |
|-----------|-------------|
| Client | Modern browsers (Chrome, Firefox, Safari, Edge) – desktop, tablet, mobile |
| Server | Node.js (Express) on Linux VM or Docker container |
| Database | PostgreSQL 14+ |
| Deployment | Docker + cloud VM (AWS EC2 or Azure) |

### 2.4 Design and Implementation Constraints

| Constraint | Description |
|------------|-------------|
| Technology Stack | Must use React.js or Vue.js, Node.js with Express, PostgreSQL |
| Project Duration | 12 weeks maximum |
| Team Size | 3-6 students |
| Map Integration | Must include interactive map functionality |
| Authentication | Must implement secure authentication (no plaintext passwords) |

### 2.5 Assumptions and Dependencies

| Assumption/Dependency | Description |
|-----------------------|-------------|
| Map API Key | Free tier API key will be available for development |
| Version Control | Team has access to GitHub |
| Cloud Credits | Cloud deployment credits available (AWS Educate or Azure for Students) |
| Internet Connectivity | Reliable internet access for development and deployment |

---

## 3. Functional Requirements

### 3.1 User Management and Authentication

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-01 | System shall allow user registration with email, password, and role selection (buyer/renter/agent) | High |
| FR-02 | System shall authenticate users via email and password using JWT with 24-hour token expiry | High |
| FR-03 | System shall allow password reset via email link | Medium |
| FR-04 | System shall prevent access to agent and admin routes for unauthenticated or unauthorised users | High |
| FR-05 | System shall allow users to update their profile (full name, phone number, profile picture) | Medium |
| FR-06 | System shall allow users to delete their own account | Low |

### 3.2 Property Listings Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-07 | Agents and owners shall be able to create a property listing with title, description, price, address, bedrooms, bathrooms, car spaces, property type, and images | High |
| FR-08 | System shall store latitude and longitude derived from address via geocoding API | High |
| FR-09 | Agents shall be able to edit their own listings only | High |
| FR-10 | Agents shall be able to delete their own listings only | High |
| FR-11 | Administrators shall be able to edit or delete any listing | High |
| FR-12 | Each listing shall display a main image thumbnail, image gallery, and agent contact information | High |
| FR-13 | System shall support image upload for listings (maximum 5 images per listing) | Medium |

### 3.3 Search and Filtering

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-14 | System shall support free-text search on suburb, postcode, or property title | High |
| FR-15 | System shall support multi-criteria filtering: price range (min-max), property type (house/apartment/townhouse/land), bedrooms, bathrooms, car spaces | High |
| FR-16 | System shall allow sorting by price (low to high, high to low) and newest first | High |
| FR-17 | Search results shall be paginated with 12 listings per page | Medium |

### 3.4 Interactive Map

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-18 | System shall display property locations as markers on an interactive map using Mapbox or Google Maps | High |
| FR-19 | Clicking a marker shall display a property card summary with price, address, and thumbnail image | High |
| FR-20 | The map shall update dynamically when search filters are applied | Medium |

### 3.5 Dashboard and Analytics

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-21 | Administrator dashboard shall display total listings, total users, and listings added in the last 7 days | High |
| FR-22 | Agent dashboard shall display the agent's own listings with view counts | Medium |
| FR-23 | System shall track listing views (unique and total) for analytics purposes | Low |

### 3.6 General Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-24 | System shall be fully responsive for mobile, tablet, and desktop devices | High |
| FR-25 | All API responses shall be in JSON format with appropriate HTTP status codes | High |
| FR-26 | System shall log all failed login attempts for security auditing | Low |

---

## 4. Non-Functional Requirements

### 4.1 Performance

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-01 | Search results must render within 2 seconds (95th percentile) | 2 seconds |
| NFR-02 | Page load time for listing detail view | < 1.5 seconds |
| NFR-03 | API response time for authenticated requests | < 500ms |
| NFR-04 | Concurrent user support | 50 concurrent users |

### 4.2 Security

| ID | Requirement | Implementation |
|----|-------------|----------------|
| NFR-05 | Passwords must be stored encrypted | bcrypt with 10 salt rounds |
| NFR-06 | JWT tokens must be stored in HTTP-only cookies | Prevents XSS attacks |
| NFR-07 | All API endpoints must validate input | Joi schema validation |
| NFR-08 | SQL injection prevention | Parameterised queries via Knex.js |
| NFR-09 | HTTPS enforcement | TLS 1.2 or higher |

### 4.3 Usability

| ID | Requirement | Standard |
|----|-------------|----------|
| NFR-10 | WCAG 2.1 AA compliance for colour contrast | Level AA |
| NFR-11 | Keyboard navigation support | All interactive elements |
| NFR-12 | Mobile-first responsive design | Breakpoints at 768px, 1024px |
| NFR-13 | Intuitive navigation (maximum 3 clicks to any feature) | Usability standard |

### 4.4 Availability and Reliability

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-14 | System uptime during assessment period (9am-5pm weekdays) | 99% |
| NFR-15 | Mean Time To Recovery (MTTR) | < 30 minutes |
| NFR-16 | Data backup frequency | Daily automated backups |

### 4.5 Maintainability

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-17 | Code unit test coverage for backend logic | ≥ 60% |
| NFR-18 | Code documentation | Inline comments for complex functions |
| NFR-19 | API documentation | Swagger/OpenAPI specification |

### 4.6 Portability

| ID | Requirement | Implementation |
|----|-------------|----------------|
| NFR-20 | Deployable via docker-compose up | Docker containerisation |
| NFR-21 | Environment configuration via environment variables | .env file support |
| NFR-22 | Cross-browser compatibility | Chrome, Firefox, Safari, Edge |

---

## 5. Required Diagrams

### 5.1 Use Case Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           Aussie Realestate Website                         │
│                                                                             │
│  ┌─────────────┐                                                            │
│  │   Guest     │                                                            │
│  └──────┬──────┘                                                            │
│         │                                                                    │
│         ├──────────────────┐                                                │
│         │                  │                                                │
│         ▼                  ▼                                                │
│  ┌─────────────┐     ┌─────────────┐                                       │
│  │  Register   │     │  Browse     │                                       │
│  │             │     │  Properties │                                       │
│  └─────────────┘     └──────┬──────┘                                       │
│                              │                                              │
│  ┌─────────────┐             │                                              │
│  │ Registered  │◄────────────┘                                              │
│  │    User     │                                                            │
│  └──────┬──────┘                                                            │
│         │                                                                    │
│    ┌────┴────┬─────────────┬─────────────┐                                 │
│    ▼         ▼             ▼             ▼                                 │
│ ┌──────┐ ┌──────┐     ┌──────────┐  ┌──────────┐                           │
│ │Login │ │Filter│     │  View    │  │  Save    │                           │
│ │      │ │Search│     │  Map     │  │ Favourites│                          │
│ └──────┘ └──────┘     └──────────┘  └──────────┘                           │
│                                                                             │
│  ┌─────────────┐                                                            │
│  │   Agent     │                                                            │
│  └──────┬──────┘                                                            │
│         │                                                                    │
│    ┌────┴────┬─────────────┬─────────────┐                                 │
│    ▼         ▼             ▼             ▼                                 │
│ ┌──────┐ ┌──────┐     ┌──────────┐  ┌──────────┐                           │
│ │Create│ │Edit  │     │ Delete   │  │  View    │                           │
│ │Listing│ │Listing│     │ Listing  │  │Dashboard │                          │
│ └──────┘ └──────┘     └──────────┘  └──────────┘                           │
│                                                                             │
│  ┌─────────────┐                                                            │
│  │   Admin     │                                                            │
│  └──────┬──────┘                                                            │
│         │                                                                    │
│    ┌────┴────┬─────────────┐                                                │
│    ▼         ▼             ▼                                                │
│ ┌──────┐ ┌──────┐     ┌──────────┐                                         │
│ │View  │ │Delete│     │  Manage  │                                         │
│ │Analytics│ │Any    │     │  Users   │                                         │
│ │      │ │Listing│     │          │                                         │
│ └──────┘ └──────┘     └──────────┘                                         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 5.2 System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT LAYER                                    │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐         │
│  │   Desktop       │    │   Tablet        │    │   Mobile        │         │
│  │   Browser       │    │   Browser       │    │   Browser       │         │
│  └────────┬────────┘    └────────┬────────┘    └────────┬────────┘         │
│           └──────────────────────┼──────────────────────┘                   │
│                                  │ HTTPS                                    │
└──────────────────────────────────┼──────────────────────────────────────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           PRESENTATION LAYER                                 │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         React SPA                                    │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │   │
│  │  │ Login    │ │ Search   │ │ Filter   │ │ Map      │ │ Listing  │  │   │
│  │  │ Component│ │ Bar      │ │ Panel    │ │ View     │ │ Card     │  │   │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘  │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                   │
                                   │ REST API (JSON)
                                   ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            APPLICATION LAYER                                 │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      Express.js Server                               │   │
│  │                                                                      │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │   │
│  │  │ Auth        │  │ Listings    │  │ Users       │  │ Dashboard   │ │   │
│  │  │ Controller  │  │ Controller  │  │ Controller  │  │ Controller  │ │   │
│  │  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘ │   │
│  │         │                │                │                │       │   │
│  │  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐ │   │
│  │  │ Auth       │  │ Listing     │  │ User       │  │ Analytics   │ │   │
│  │  │ Service    │  │ Service     │  │ Service    │  │ Service     │ │   │
│  │  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘ │   │
│  └─────────┼────────────────┼────────────────┼────────────────┼────────┘   │
│            │                │                │                │            │
│  ┌─────────▼────────────────▼────────────────▼────────────────▼────────┐   │
│  │                      Middleware Layer                                │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │   │
│  │  │ JWT Verify   │  │ Request      │  │ Error        │               │   │
│  │  │ Middleware   │  │ Logger       │  │ Handler      │               │   │
│  │  └──────────────┘  └──────────────┘  └──────────────┘               │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                   │
                                   │ SQL Queries
                                   ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              DATA LAYER                                      │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         PostgreSQL Database                          │   │
│  │                                                                      │   │
│  │  ┌────────────┐  ┌────────────┐  ┌─────────────────┐               │   │
│  │  │ users      │  │ listings   │  │ listing_images  │               │   │
│  │  │ table      │  │ table      │  │ table           │               │   │
│  │  └────────────┘  └────────────┘  └─────────────────┘               │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                   │
                                   │ HTTPS
                                   ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         EXTERNAL SERVICES                                    │
│  ┌─────────────────────────┐      ┌─────────────────────────┐              │
│  │   Mapbox / Google Maps  │      │   Cloud Storage         │              │
│  │   API                   │      │   (Images)              │              │
│  └─────────────────────────┘      └─────────────────────────┘              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 5.3 Database ER Diagram

```
┌─────────────────┐
│      users      │
├─────────────────┤
│ PK │ id         │
│    │ email      │
│    │ password_hash│
│    │ full_name  │
│    │ role       │
│    │ created_at │
└────────┬────────┘
         │ 1
         │
         │ has many
         │
         ▼ N
┌─────────────────┐
│    listings     │
├─────────────────┤
│ PK │ id         │
│ FK │ agent_id   │
│    │ title      │
│    │ description│
│    │ price      │
│    │ address    │
│    │ lat        │
│    │ lng        │
│    │ bedrooms   │
│    │ bathrooms  │
│    │ car_spaces │
│    │ property_type│
│    │ status     │
│    │ created_at │
└────────┬────────┘
         │ 1
         │
         │ has many
         │
         ▼ N
┌─────────────────┐
│ listing_images  │
├─────────────────┤
│ PK │ id         │
│ FK │ listing_id │
│    │ image_url  │
│    │ display_order│
└─────────────────┘

┌─────────────────┐
│  listing_views  │
├─────────────────┤
│ PK │ id         │
│ FK │ listing_id │
│    │ viewed_at  │
│    │ viewer_ip_hash│
└─────────────────┘
```

### 5.4 Sequence Diagram – Search Properties

```
┌───────┐     ┌─────────┐     ┌─────────┐     ┌───────┐     ┌─────────┐
│ User  │     │ React   │     │ Express │     │  DB   │     │ Mapbox  │
│       │     │ Frontend│     │ Backend │     │       │     │  API    │
└───┬───┘     └────┬────┘     └────┬────┘     └───┬───┘     └────┬────┘
    │              │               │               │              │
    │ 1. Enter     │               │               │              │
    │ filters      │               │               │              │
    │─────────────>│               │               │              │
    │              │               │               │              │
    │              │ 2. GET /api/  │               │              │
    │              │ listings?     │               │              │
    │              │ suburb=Surry  │               │              │
    │              │ &minPrice=500k│               │              │
    │              │──────────────>│               │              │
    │              │               │               │              │
    │              │               │ 3. SELECT *   │              │
    │              │               │ FROM listings │              │
    │              │               │ WHERE...      │              │
    │              │               │──────────────>│              │
    │              │               │               │              │
    │              │               │ 4. Rows       │              │
    │              │               │<──────────────│              │
    │              │               │               │              │
    │              │               │ 5. Geocode if │              │
    │              │               │ new bounds    │              │
    │              │               │─────────────────────────────>│
    │              │               │               │              │
    │              │               │ 6. Coordinates│              │
    │              │               │<─────────────────────────────│
    │              │               │               │              │
    │              │ 7. JSON       │               │              │
    │              │ listings[]    │               │              │
    │              │<──────────────│               │              │
    │              │               │               │              │
    │ 8. Render    │               │               │              │
    │ cards + map  │               │               │              │
    │<─────────────│               │               │              │
    │              │               │               │              │
```

### 5.5 Component Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              UI LAYER                                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │  LoginForm  │  │RegisterForm │  │ SearchBar   │  │ FilterPanel │        │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │ ListingsGrid│  │ ListingCard │  │  MapView    │  │ListingDetail│        │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘        │
│  ┌─────────────┐  ┌─────────────┐                                         │
│  │AgentDashboard│ │AdminDashboard│                                         │
│  └─────────────┘  └─────────────┘                                         │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           API GATEWAY LAYER                                  │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │ /api/auth   │  │/api/listings│  │ /api/users  │  │/api/dashboard│       │
│  │ Routes      │  │ Routes      │  │ Routes      │  │ Routes      │        │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘        │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           SERVICE LAYER                                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │AuthService  │  │ListingService│ │UserService  │  │AnalyticsService│      │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘        │
│  ┌─────────────┐                                                            │
│  │ GeoService  │                                                            │
│  └─────────────┘                                                            │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         DATA ACCESS LAYER                                    │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    PostgreSQL Client (Knex.js)                       │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         EXTERNAL SERVICES                                    │
│  ┌─────────────────┐              ┌─────────────────┐                      │
│  │  Maps API       │              │  Cloud Storage  │                      │
│  └─────────────────┘              └─────────────────┘                      │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 6. API Endpoints Design

| Method | Endpoint | Description | Authentication | Role |
|--------|----------|-------------|----------------|------|
| POST | `/api/auth/register` | Register new user | None | Guest |
| POST | `/api/auth/login` | Login and receive JWT | None | Guest |
| POST | `/api/auth/logout` | Invalidate current session | Required | Any |
| GET | `/api/auth/me` | Get current user profile | Required | Any |
| PUT | `/api/auth/me` | Update user profile | Required | Any |
| GET | `/api/listings` | Search and filter listings | None | Guest/User |
| GET | `/api/listings/:id` | Get single listing details | None | Guest/User |
| POST | `/api/listings` | Create new listing | Required | Agent |
| PUT | `/api/listings/:id` | Update listing | Required | Agent/Admin |
| DELETE | `/api/listings/:id` | Delete listing | Required | Agent/Admin |
| GET | `/api/dashboard/agent` | Agent analytics dashboard | Required | Agent |
| GET | `/api/dashboard/admin` | Admin usage analytics | Required | Admin |
| GET | `/api/listings/:id/views` | Get view count for listing | Required | Agent/Admin |

---

## 7. Traceability Matrix

| Requirement ID | Related NFR | Test Case | Priority |
|----------------|-------------|-----------|----------|
| FR-01, FR-02 | NFR-05, NFR-06 | TC-AUTH-01: User registration and login | High |
| FR-04 | NFR-06 | TC-AUTH-02: Unauthorised access prevention | High |
| FR-07 to FR-10 | NFR-17 | TC-LIST-01: Listing CRUD operations | High |
| FR-14 to FR-16 | NFR-01 | TC-SEARCH-01: Search and filtering performance | High |
| FR-18 to FR-20 | NFR-10 | TC-MAP-01: Map interaction and markers | High |
| FR-21, FR-22 | NFR-02 | TC-DASH-01: Dashboard data accuracy | Medium |
| FR-24 | NFR-12 | TC-UI-01: Responsive design on all devices | High |
| NFR-01 | FR-14 to FR-16 | TC-PERF-01: Search response time < 2 seconds | High |
| NFR-05 | FR-02 | TC-SEC-01: Password hashing verification | High |

---

## 8. Acceptance Criteria

For the final submission to be accepted, the following criteria must be met:

| ID | Acceptance Criterion | Verification Method |
|----|---------------------|---------------------|
| AC-01 | Guest can view properties on map and filter by price/suburb | Manual testing |
| AC-02 | Agent can log in, add a new property with images, and see it appear on the map | Manual testing |
| AC-03 | Admin dashboard displays at least three metrics (listings, users, weekly trend) | Manual testing |
| AC-04 | All API endpoints return proper HTTP status codes (200, 400, 401, 403, 404) | API testing |
| AC-05 | Application runs via `docker-compose up` on a fresh Ubuntu VM | Deployment test |
| AC-06 | No plaintext passwords in database or logs | Code review |
| AC-07 | Application is responsive on mobile, tablet, and desktop | Cross-device testing |
| AC-08 | Unit test coverage for backend logic is at least 60% | Test coverage report |

---

## 9. Appendices

### 9.1 Assumptions for Students

| Assumption | Description |
|------------|-------------|
| Map API Free Tier | Mapbox or Google Maps free tier allows ~50,000 requests per month |
| Image Storage | Local uploads folder or Cloudinary free tier |
| Email Service | Password reset email can be simulated (print link to console) for MVP |
| Deployment | Cloud credits available through AWS Educate or Azure for Students |

### 9.2 Risk Register

| Risk | Probability | Impact | Mitigation Strategy |
|------|-------------|--------|---------------------|
| Map API rate limiting | Medium | Medium | Implement client-side caching of geocodes |
| Team member absence | Medium | High | Daily standups, shared GitHub project board |
| Incomplete deployment | Medium | High | Start deployment in Week 6 (not Week 11) |
| Scope creep | High | Medium | Strict MVP feature prioritisation |
| Integration delays | Medium | Medium | Weekly integration sprints |

---

## Document Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Lead | Student Team Lead | | |
| Industry Sponsor | Nabin Singh | | |
| Academic Supervisor | | | |

---
