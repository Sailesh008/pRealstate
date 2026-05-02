
# Week 3 Deliverable:SRS Document
## Aussie Realestate Website – WIL Project



## 1. Primary Users & Their Goals

| User Persona | Role | Primary Goal |
|--------------|------|--------------|
| Sarah (Buyer/Renter) | Guest / Registered User | Find a property to buy or rent quickly using filters and maps |
| David (Property Agent) | Agent | List and manage properties, track how many views each listing gets |
| Admin (System Manager) | Admin | Monitor platform usage, manage users, ensure system health |



## 2. User Stories

### For Sarah (Buyer/Renter)

- *As a* **guest user**, *I want to* search properties without logging in, *so that* I can quickly browse listings.
- *As a* **registered user**, *I want to* save my favourite properties, *so that* I can revisit them later.
- *As a* **buyer**, *I want to* filter properties by price, suburb, bedrooms, and property type, *so that* I find only relevant listings.
- *As a* **user**, *I want to* see property locations on an interactive map, *so that* I understand the neighbourhood.

### For David (Property Agent)

- *As an* **agent**, *I want to* create and edit my property listings with images and details, *so that* buyers see accurate information.
- *As an* **agent**, *I want to* view how many times each of my listings has been seen, *so that* I know which properties are popular.
- *As an* **agent**, *I want to* delete my own listings when sold or rented, *so that* outdated listings are removed.

### For Admin

- *As an* **admin**, *I want to* view total users, total listings, and weekly trends, *so that* I understand platform growth.
- *As an* **admin**, *I want to* delete any inappropriate listing, *so that* the platform remains trustworthy.



## 3. Software Requirements Specification (SRS) – Week 3 Version

### 3.1 Functional Requirements

| ID | Requirement |
|----|-------------|
| FR-01 | Guest can browse and search property listings |
| FR-02 | User can register and login with email/password |
| FR-03 | Agent can create, edit, delete their own listings |
| FR-04 | User can filter by price, location, bedrooms, property type |
| FR-05 | System displays property locations on interactive map |
| FR-06 | Admin can view dashboard with usage analytics |
| FR-07 | System is fully responsive (mobile/desktop) |

### 3.2 Non-Functional Requirements

| ID | Requirement |
|----|-------------|
| NFR-01 | Search results load within 2 seconds |
| NFR-02 | Passwords encrypted using bcrypt |
| NFR-03 | 99% uptime during assessment period |
| NFR-04 | WCAG 2.1 AA accessibility compliant |



## 4. Actions Users Will Perform

| Persona | Key Actions |
|---------|-------------|
| Sarah | Search → Filter → View map → Click listing → Contact agent |
| David | Login → Add listing → Upload images → View dashboard → Edit/Delete |
| Admin | Login → View analytics → Remove inappropriate content |



## 5. Deliverable Checklist for Week 3

- [x] Core features identified (search, filter, map, auth, CRUD listings, analytics)
- [x] User personas defined (Sarah, David, Admin)
- [x] User stories written (9 stories covering all personas)
- [x] Functional requirements listed (FR-01 to FR-07)
- [x] Non-functional requirements listed (NFR-01 to NFR-04)

