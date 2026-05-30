# Week 8 — Frontend Development

## Overview

| Field           | Details              |
|-----------------|----------------------|
| **Week**        | 8                    |
| **Focus**       | Frontend Development |
| **Status**      | In Progress          |
| **Last Updated**| YYYY-MM-DD           |

---

## Expected Deliverable

Build the user interface including page implementation, API integration, and form handling.

---

## Activities

- [ ] Implement pages and layouts
- [ ] Connect frontend to backend APIs
- [ ] Implement forms with validation

---

## Pages Implemented

| Page              | Route           | API Connected | Status      |
|-------------------|-----------------|---------------|-------------|
| Landing / Home    | /               | No            | Done        |
| Login             | /login          | Yes           | Done        |
| Register          | /register       | Yes           | Done        |
| Dashboard         | /dashboard      | Yes           | In Progress |
| Profile           | /profile        | No            | Pending     |
| Settings          | /settings       | No            | Pending     |

Note: Replace with your actual pages and routes.

---

## Forms Implemented

| Form              | Page       | Validation | API Connected | Status      |
|-------------------|------------|------------|---------------|-------------|
| Login Form        | /login     | Yes        | Yes           | Done        |
| Registration Form | /register  | Yes        | Yes           | Done        |
| Profile Edit Form | /profile   | Partial    | No            | In Progress |
| Settings Form     | /settings  | No         | No            | Pending     |

### Validation Rules Applied

- [x] Required field checks
- [x] Email format validation
- [x] Password strength / confirmation match
- [ ] Phone number format
- [ ] File upload type and size limits

---

## API Connections

| Endpoint            | Method | Page / Component    | Status      |
|---------------------|--------|---------------------|-------------|
| /api/auth/login     | POST   | Login Page          | Done        |
| /api/auth/register  | POST   | Register Page       | Done        |
| /api/users/me       | GET    | Dashboard / Profile | In Progress |
| /api/users/:id      | PUT    | Profile Edit        | Pending     |
| /api/settings       | GET    | Settings Page       | Pending     |

Note: Replace with your actual endpoints and components.

---

## Core User Journeys

| Journey                        | Steps Covered                                      | Status      |
|--------------------------------|----------------------------------------------------|-------------|
| User Registration              | Fill form > Submit > Redirect to dashboard         | Done        |
| User Login                     | Fill form > Authenticate > Redirect to dashboard   | Done        |
| View Dashboard                 | Load data > Display summary cards                  | In Progress |
| Edit Profile                   | Load profile > Edit fields > Save changes          | Pending     |
| Logout                         | Click logout > Clear session > Redirect to login   | Pending     |

---

## Component Library / UI

- Framework:     e.g., React / Vue / Angular / Next.js
- Styling:       e.g., Tailwind CSS / Bootstrap / CSS Modules
- UI Components: e.g., ShadCN / Material UI / Ant Design
- State Management: e.g., Redux / Zustand / Context API

### Reusable Components Built

| Component       | Description                    | Status      |
|-----------------|--------------------------------|-------------|
| Navbar          | Top navigation bar             | Done        |
| Sidebar         | Side navigation menu           | In Progress |
| Button          | Primary / secondary variants   | Done        |
| Input Field     | Text, email, password types    | Done        |
| Modal           | Confirmation / alert dialogs   | Pending     |
| Toast / Alert   | Success and error notifications| Pending     |
| Loading Spinner | API call feedback indicator    | In Progress |

---

## Known Issues / Blockers

- List any current blockers or bugs here
- None at this time.

---

## Repository Structure

frontend/
    src/
        pages/          - Route-level page components
        components/     - Reusable UI components
        services/       - API call functions
        hooks/          - Custom React/Vue hooks
        store/          - State management
        styles/         - Global styles and themes
        utils/          - Helper functions
    public/             - Static assets
    .env.example        - Environment variable template
    package.json
    README.md

---

## Testing

| Test Type        | Tool                      | Status      |
|------------------|---------------------------|-------------|
| Unit Tests       | Jest / Vitest             | Pending     |
| Component Tests  | React Testing Library     | Pending     |
| E2E Tests        | Cypress / Playwright      | Pending     |

---

## Notes & Next Steps

- [ ] Complete Dashboard page with live API data
- [ ] Implement Profile Edit form and connect to API
- [ ] Build Settings page
- [ ] Add toast notifications for form success/error states
- [ ] Implement logout flow and session management
- [ ] Begin E2E testing for core user journeys

---

## References

- Figma / Design File  : Link to design mockups
- Backend API Docs     : Link to Swagger / Postman collection
- Component Storybook  : Link to Storybook (if applicable)
- Project Board        : Link to GitHub Issues / Jira

