# Week 9 — Feature Integration

## Overview

| Field            | Details              |
|------------------|----------------------|
| **Week**         | 9                    |
| **Focus**        | Feature Integration  |
| **Status**       | In Progress          |
| **Last Updated** | YYYY-MM-DD           |

---

## Expected Deliverable

Connect the frontend and backend into a working MVP by integrating APIs, implementing authentication, and resolving integration issues.

---

## Activities

- [ ] Integrate frontend with backend APIs
- [ ] Implement end-to-end authentication flow
- [ ] Identify and fix integration issues
- [ ] Validate MVP features are fully working

---

## API Integration Status

| Endpoint               | Method | Frontend Page / Component | Integration Status | Notes                          |
|------------------------|--------|---------------------------|--------------------|--------------------------------|
| /api/auth/login        | POST   | Login Page                | Done               | JWT token stored in cookies    |
| /api/auth/register     | POST   | Register Page             | Done               | Redirects to dashboard         |
| /api/auth/logout       | POST   | Navbar                    | In Progress        | Session clear pending          |
| /api/users/me          | GET    | Dashboard / Profile       | In Progress        | Loading state implemented      |
| /api/users/:id         | PUT    | Profile Edit              | Pending            |                                |
| /api/resources         | GET    | Dashboard                 | Pending            |                                |
| /api/resources/:id     | DELETE | Dashboard                 | Pending            |                                |

Note: Replace with your actual endpoints and pages.

---

## Authentication Flow

### Implementation Checklist

- [x] User registration with hashed password
- [x] User login returning JWT / session token
- [x] Token stored securely (HttpOnly cookie / localStorage)
- [x] Protected routes on the frontend (redirect if unauthenticated)
- [x] Auth middleware on protected backend routes
- [ ] Token refresh / expiry handling
- [ ] Logout clears token and redirects to login
- [ ] Persistent login on page reload

### Auth Flow Diagram

User visits protected route
        |
        v
Is token present?
   |          |
  YES         NO
   |          |
   v          v
Validate   Redirect
 token     to /login
   |
   v
Token valid?
   |          |
  YES         NO
   |          |
   v          v
Load page  Clear token
          Redirect to
            /login

---

## MVP Features Status

| Feature                  | Frontend | Backend | Integrated | Tested | Status      |
|--------------------------|----------|---------|------------|--------|-------------|
| User Registration        | Done     | Done    | Yes        | Yes    | Working     |
| User Login               | Done     | Done    | Yes        | Yes    | Working     |
| User Logout              | Done     | Done    | Partial    | No     | In Progress |
| View Dashboard           | Done     | Done    | Partial    | No     | In Progress |
| Edit Profile             | Done     | Done    | No         | No     | Pending     |
| Create Resource          | Done     | Done    | No         | No     | Pending     |
| View Resource List       | Done     | Done    | No         | No     | Pending     |
| Delete Resource          | Pending  | Done    | No         | No     | Pending     |

Note: Replace Feature column entries with your actual MVP features.

---

## Core User Journeys — Working End-to-End

| Journey                     | Status      | Notes                                  |
|-----------------------------|-------------|----------------------------------------|
| Register a new account      | Working     | Fully integrated and tested            |
| Log in with existing account| Working     | JWT issued, frontend stores token      |
| View personal dashboard     | In Progress | API connected, data rendering partial  |
| Edit and save profile       | Pending     | Form built, PUT endpoint not wired     |
| Log out and clear session   | In Progress | Backend endpoint ready, frontend WIP   |
| Access protected page       | In Progress | Guard implemented, refresh needed      |

---

## Integration Issues Log

| Issue ID | Description                              | Affected Area       | Priority | Status      |
|----------|------------------------------------------|---------------------|----------|-------------|
| INT-001  | CORS error on /api/users/me              | Frontend - Backend  | High     | Fixed       |
| INT-002  | JWT not sent in request headers          | Auth Middleware     | High     | Fixed       |
| INT-003  | Dashboard does not re-fetch on login     | Dashboard Page      | Medium   | In Progress |
| INT-004  | Token expiry not handled on frontend     | Auth Flow           | Medium   | Pending     |
| INT-005  | Form submission returns 500 on empty bio | Profile Edit        | Low      | Pending     |

Note: Add new rows as issues are discovered and resolved.

---

## Environment Configuration

| Variable             | Frontend | Backend | Description                        |
|----------------------|----------|---------|------------------------------------|
| VITE_API_BASE_URL    | Yes      | No      | Base URL pointing to backend       |
| JWT_SECRET           | No       | Yes     | Secret key for signing tokens      |
| DATABASE_URL         | No       | Yes     | Database connection string         |
| PORT                 | No       | Yes     | Backend server port                |
| NODE_ENV             | Yes      | Yes     | development / production           |

Note: Never commit actual values — use .env.example files only.

---

## Repository Structure

project/
    frontend/
        src/
            pages/          - Route-level page components
            components/     - Reusable UI components
            services/       - API call functions (axios / fetch)
            hooks/          - Custom hooks (useAuth, useFetch)
            store/          - Auth and global state
        .env.example
        package.json

    backend/
        src/
            routes/         - API route definitions
            controllers/    - Request handlers
            middleware/      - Auth, validation, error handling
            models/         - Database models / schemas
            config/         - DB and environment config
        .env.example
        package.json

    README.md

---

## Testing

| Test Type           | Tool                  | Coverage Area              | Status      |
|---------------------|-----------------------|----------------------------|-------------|
| Unit Tests          | Jest / Vitest         | Services, utilities        | In Progress |
| Integration Tests   | Supertest             | API endpoints              | In Progress |
| E2E Tests           | Cypress / Playwright  | Core user journeys         | Pending     |
| Manual Testing      | Postman / Browser     | All MVP features           | In Progress |

---

## Known Issues / Blockers

- List any current blockers or bugs here
- None at this time.

---

## Notes & Next Steps

- [ ] Complete logout flow on frontend
- [ ] Wire Profile Edit form to PUT /api/users/:id
- [ ] Connect Dashboard to GET /api/resources
- [ ] Handle token expiry with refresh or re-login redirect
- [ ] Run E2E tests across all core user journeys
- [ ] Conduct full manual QA pass on MVP features
- [ ] Prepare demo build for Week 9 review

---

## References

- Backend API Docs     : Link to Swagger / Postman collection
- Frontend Repo        : Link to frontend repository
- Backend Repo         : Link to backend repository
- Design File          : Link to Figma / design mockups
- Project Board        : Link to GitHub Issues / Jira
- Week 7 Progress      : Link to WEEK7_PROGRESS.md
- Week 8 Progress      : Link to WEEK8_PROGRESS.md

