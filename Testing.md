# Week 10 — Testing

## Overview

| Field            | Details              |
|------------------|----------------------|
| **Week**         | 10                   |
| **Focus**        | Testing & QA         |
| **Status**       | In Progress          |
| **Last Updated** | YYYY-MM-DD           |

---

## Expected Deliverable

Ensure system quality through unit tests, integration tests, and manual testing.
Deliver a completed Test Plan and Test Reports.

---

## Activities

- [ ] Write and run unit tests
- [ ] Write and run integration tests
- [ ] Conduct manual / exploratory testing
- [ ] Log and track all bugs found
- [ ] Document test results and coverage

---

## Testing Summary

| Test Type         | Tool                  | Total Cases | Passed | Failed | Skipped | Coverage |
|-------------------|-----------------------|-------------|--------|--------|---------|----------|
| Unit Tests        | Jest / Vitest         | 0           | 0      | 0      | 0       | 0%       |
| Integration Tests | Supertest / Jest      | 0           | 0      | 0      | 0       | 0%       |
| E2E Tests         | Cypress / Playwright  | 0           | 0      | 0      | 0       | 0%       |
| Manual Tests      | Browser / Postman     | 0           | 0      | 0      | 0       | -        |

Note: Update numbers as tests are written and executed.

---

## Test Plan

### Scope

The following areas are in scope for Week 10 testing:

- User authentication (register, login, logout)
- Core API endpoints (CRUD operations)
- Frontend forms and input validation
- Core user journeys end-to-end
- Error handling and edge cases

The following are out of scope:

- Performance / load testing
- Security penetration testing
- Third-party service integrations

---

### Critical Test Cases

#### 1. Authentication

| Test ID  | Test Case                                  | Type        | Priority | Status  |
|----------|--------------------------------------------|-------------|----------|---------|
| TC-A001  | Register with valid credentials            | Integration | High     | Pending |
| TC-A002  | Register with duplicate email              | Integration | High     | Pending |
| TC-A003  | Login with correct credentials             | Integration | High     | Pending |
| TC-A004  | Login with incorrect password              | Integration | High     | Pending |
| TC-A005  | Access protected route without token       | Integration | High     | Pending |
| TC-A006  | Access protected route with expired token  | Integration | High     | Pending |
| TC-A007  | Logout clears session and redirects        | E2E         | High     | Pending |

#### 2. API Endpoints

| Test ID  | Test Case                                  | Type        | Priority | Status  |
|----------|--------------------------------------------|-------------|----------|---------|
| TC-B001  | GET /api/resources returns 200 with data   | Integration | High     | Pending |
| TC-B002  | POST /api/resources creates a new record   | Integration | High     | Pending |
| TC-B003  | POST /api/resources with missing fields    | Integration | High     | Pending |
| TC-B004  | PUT /api/resources/:id updates record      | Integration | Medium   | Pending |
| TC-B005  | PUT /api/resources/:id with invalid ID     | Integration | Medium   | Pending |
| TC-B006  | DELETE /api/resources/:id removes record   | Integration | Medium   | Pending |
| TC-B007  | GET /api/resources/:id not found returns 404| Integration| Medium   | Pending |

#### 3. Frontend Forms

| Test ID  | Test Case                                  | Type        | Priority | Status  |
|----------|--------------------------------------------|-------------|----------|---------|
| TC-C001  | Submit login form with empty fields        | Unit / E2E  | High     | Pending |
| TC-C002  | Submit login form with invalid email format| Unit        | High     | Pending |
| TC-C003  | Register form password mismatch error      | Unit        | High     | Pending |
| TC-C004  | Form shows success message on submit       | E2E         | Medium   | Pending |
| TC-C005  | Form shows error message on API failure    | E2E         | Medium   | Pending |

#### 4. Core User Journeys

| Test ID  | Test Case                                  | Type        | Priority | Status  |
|----------|--------------------------------------------|-------------|----------|---------|
| TC-D001  | New user can register and reach dashboard  | E2E         | High     | Pending |
| TC-D002  | Existing user can log in and view data     | E2E         | High     | Pending |
| TC-D003  | User can edit profile and save changes     | E2E         | High     | Pending |
| TC-D004  | User can create and view a new resource    | E2E         | High     | Pending |
| TC-D005  | User can delete a resource                 | E2E         | Medium   | Pending |
| TC-D006  | Unauthenticated user is redirected to login| E2E         | High     | Pending |

Note: Add rows for your project-specific features and journeys.

---

## Unit Test Results

### Backend

| Module / Function         | Test File                  | Cases | Passed | Failed | Status  |
|---------------------------|----------------------------|-------|--------|--------|---------|
| authController.register   | auth.controller.test.js    | 0     | 0      | 0      | Pending |
| authController.login      | auth.controller.test.js    | 0     | 0      | 0      | Pending |
| userModel.findById        | user.model.test.js         | 0     | 0      | 0      | Pending |
| validateBody middleware   | validate.middleware.test.js| 0     | 0      | 0      | Pending |

### Frontend

| Component / Hook          | Test File                  | Cases | Passed | Failed | Status  |
|---------------------------|----------------------------|-------|--------|--------|---------|
| LoginForm                 | LoginForm.test.jsx         | 0     | 0      | 0      | Pending |
| RegisterForm              | RegisterForm.test.jsx      | 0     | 0      | 0      | Pending |
| useAuth hook              | useAuth.test.js            | 0     | 0      | 0      | Pending |
| apiService                | apiService.test.js         | 0     | 0      | 0      | Pending |

---

## Integration Test Results

| Endpoint                  | Method | Test File                  | Cases | Passed | Failed | Status  |
|---------------------------|--------|----------------------------|-------|--------|--------|---------|
| /api/auth/register        | POST   | auth.routes.test.js        | 0     | 0      | 0      | Pending |
| /api/auth/login           | POST   | auth.routes.test.js        | 0     | 0      | 0      | Pending |
| /api/users/me             | GET    | user.routes.test.js        | 0     | 0      | 0      | Pending |
| /api/resources            | GET    | resource.routes.test.js    | 0     | 0      | 0      | Pending |
| /api/resources            | POST   | resource.routes.test.js    | 0     | 0      | 0      | Pending |
| /api/resources/:id        | DELETE | resource.routes.test.js    | 0     | 0      | 0      | Pending |

---

## Manual Test Results

| Test ID  | Test Case                                  | Tester  | Date       | Result  | Notes                    |
|----------|--------------------------------------------|---------|------------|---------|--------------------------|
| TC-D001  | New user registers and reaches dashboard   |         | YYYY-MM-DD | Pending |                          |
| TC-D002  | Existing user logs in and views data       |         | YYYY-MM-DD | Pending |                          |
| TC-A007  | Logout clears session and redirects        |         | YYYY-MM-DD | Pending |                          |

---

## Bug Report

| Bug ID  | Title                                      | Area            | Severity | Status      | Reported   | Resolved   |
|---------|--------------------------------------------|-----------------|----------|-------------|------------|------------|
| BUG-001 | Describe the bug briefly                   | Frontend/Backend| High     | Open        | YYYY-MM-DD |            |
| BUG-002 | Describe the bug briefly                   | Frontend/Backend| Medium   | In Progress | YYYY-MM-DD |            |
| BUG-003 | Describe the bug briefly                   | Frontend/Backend| Low      | Resolved    | YYYY-MM-DD | YYYY-MM-DD |

### Severity Guide

- High    : Blocks a core user journey or causes data loss
- Medium  : Affects functionality but a workaround exists
- Low     : Minor UI or cosmetic issue

---

## Test Coverage Report

| Area                | Target Coverage | Current Coverage | Status      |
|---------------------|-----------------|------------------|-------------|
| Backend Controllers | 80%             | 0%               | Pending     |
| Backend Middleware  | 80%             | 0%               | Pending     |
| Backend Routes      | 80%             | 0%               | Pending     |
| Frontend Components | 70%             | 0%               | Pending     |
| Frontend Services   | 80%             | 0%               | Pending     |

Note: Run coverage reports with:
  Backend  - npm run test:coverage
  Frontend - npm run test:coverage

---

## Known Issues / Blockers

- List any current blockers or bugs here
- None at this time.

---

## Notes & Next Steps

- [ ] Write unit tests for all controllers and middleware
- [ ] Write unit tests for all frontend form components
- [ ] Complete integration tests for all API endpoints
- [ ] Execute all manual test cases and log results
- [ ] Resolve all High severity bugs before MVP sign-off
- [ ] Achieve minimum 70-80% code coverage
- [ ] Finalise and submit Test Report

---

## References

- Test Cases Spreadsheet : Link to Google Sheets / Notion test tracker
- Bug Tracker            : Link to GitHub Issues / Jira
- API Docs               : Link to Swagger / Postman collection
- Week 9 Progress        : Link to WEEK9_PROGRESS.md
- Frontend Repo          : Link to frontend repository
- Backend Repo           : Link to backend repository

