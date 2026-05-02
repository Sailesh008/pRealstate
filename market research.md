
# Week 3 – Requirements Gathering

## What is this week about?

Defining **what the system must do** – its features, user personas, and user stories.

---

## Key Questions Answered

| Question | Answer |
|----------|--------|
| **Who are the primary users?** | Guest, Registered User, Property Agent, Admin |
| **What are their goals?** | Find properties, list properties, manage platform |
| **What actions will they perform?** | Search, filter, view maps, create listings, view dashboards |

---

## Activities Done

| Activity | What We Did |
|----------|-------------|
| Identify core features | Listed MVP features (search, filter, map, auth, CRUD, analytics) |
| Define user personas | Created 4 personas with goals and pain points |
| Write user stories | Wrote 15 user stories covering all personas |
| Document requirements | Created 15 functional and 15 non-functional requirements |

---

## User Personas

### Persona 1: Sarah (Buyer/Renter)
| Attribute | Detail |
|-----------|--------|
| Role | Guest / Registered User |
| Age | 28, lives in Melbourne |
| Goal | Find a property to buy or rent quickly |
| Pain Point | Wastes time on listings without prices |
| Actions | Search, filter, view map, save favourites |

### Persona 2: David (Property Agent)
| Attribute | Detail |
|-----------|--------|
| Role | Agent |
| Age | 42, works in Sydney |
| Goal | List and manage properties, track views |
| Pain Point | Pays high portal fees for poor leads |
| Actions | Create, edit, delete listings; view dashboard |

### Persona 3: Admin (System Manager)
| Attribute | Detail |
|-----------|--------|
| Role | Admin |
| Age | 35 |
| Goal | Monitor platform usage, manage users |
| Pain Point | No oversight of inappropriate listings |
| Actions | View analytics, delete any listing, manage users |

### Persona 4: Emma (First-Time Buyer)
| Attribute | Detail |
|-----------|--------|
| Role | Registered User |
| Age | 24, lives in Brisbane |
| Goal | Find affordable first home with transparent pricing |
| Pain Point | Overwhelmed by complex real estate jargon |
| Actions | Search, filter by budget, save favourites, view educational content |

---

## User Stories

### For Guest / Buyer / Renter

| ID | User Story |
|----|------------|
| US-01 | As a **guest**, I want to browse properties without logging in, so that I can quickly see available listings. |
| US-02 | As a **buyer**, I want to filter by price, suburb, bedrooms, and property type, so that I find relevant properties. |
| US-03 | As a **user**, I want to see property locations on a map, so that I understand the neighbourhood. |
| US-04 | As a **registered user**, I want to save favourite properties, so that I can revisit them later. |
| US-05 | As a **renter**, I want to filter by rental price per week, so that I stay within my budget. |
| US-06 | As a **user**, I want to sort search results by price (low to high), so that I see most affordable options first. |

### For Agent

| ID | User Story |
|----|------------|
| US-07 | As an **agent**, I want to create property listings with images and details, so that buyers see accurate information. |
| US-08 | As an **agent**, I want to edit my listings, so that I can update prices or details. |
| US-09 | As an **agent**, I want to delete my sold or rented listings, so that outdated listings are removed. |
| US-10 | As an **agent**, I want to view how many times my listings have been seen, so that I know which properties are popular. |
| US-11 | As an **agent**, I want to upload up to 5 images per listing, so that buyers see multiple angles of the property. |

### For Admin

| ID | User Story |
|----|------------|
| US-12 | As an **admin**, I want to view total users and listings, so that I understand platform growth. |
| US-13 | As an **admin**, I want to delete any inappropriate listing, so that the platform remains trustworthy. |
| US-14 | As an **admin**, I want to view weekly user registration trends, so that I track platform adoption. |
| US-15 | As an **admin**, I want to reset user passwords, so that I can assist locked-out users. |

---

## Core Features (MVP)

| Feature | Priority | Description |
|---------|----------|-------------|
| User Registration & Login | High | Email/password signup and JWT authentication |
| Property Listings CRUD | High | Create, read, update, delete listings with images |
| Search & Filter | High | Text search + filters (price, bedrooms, type, suburb) |
| Interactive Map | High | Show property locations as markers using Mapbox |
| Agent Dashboard | Medium | View own listings and view counts |
| Admin Dashboard | Medium | View platform usage analytics |
| Image Upload | Medium | Upload up to 5 images per listing |
| Password Reset | Low | Email-based password recovery |
| Favourites | Low | Save favourite properties to user account |

---

## Functional Requirements (15+)

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-01 | Guest can browse property listings without logging in | High |
| FR-02 | Guest can search properties by keyword (suburb, postcode, title) | High |
| FR-03 | User can register with email, password, full name, and role selection | High |
| FR-04 | User can login with email and password using JWT authentication | High |
| FR-05 | User can logout and invalidate the JWT token | High |
| FR-06 | User can update their profile (name, phone, profile picture) | Medium |
| FR-07 | User can reset forgotten password via email link | Low |
| FR-08 | Agent can create a new property listing with title, description, price, address, bedrooms, bathrooms, car spaces, property type | High |
| FR-09 | Agent can upload up to 5 images per listing with display order | Medium |
| FR-10 | Agent can edit their own listings | High |
| FR-11 | Agent can delete their own listings | High |
| FR-12 | Agent can view a dashboard showing their listings and view counts | Medium |
| FR-13 | Admin can view platform analytics (total users, total listings, weekly trends) | Medium |
| FR-14 | Admin can delete any inappropriate listing | High |
| FR-15 | Admin can manage users (view, reset password, deactivate) | Low |
| FR-16 | User can filter properties by price range (min and max) | High |
| FR-17 | User can filter properties by number of bedrooms (1, 2, 3, 4+) | High |
| FR-18 | User can filter properties by property type (house, apartment, townhouse, land) | High |
| FR-19 | User can filter properties by suburb/location | High |
| FR-20 | User can sort search results by price (low to high, high to low) | High |
| FR-21 | User can sort search results by newest first | Medium |
| FR-22 | Search results are paginated (12 listings per page) | Medium |
| FR-23 | System displays property locations as markers on interactive map | High |
| FR-24 | Clicking a map marker shows property summary (price, address, thumbnail) | High |
| FR-25 | Map updates dynamically when search filters are applied | Medium |
| FR-26 | System is fully responsive on mobile, tablet, and desktop | High |
| FR-27 | All API responses return JSON format with appropriate HTTP status codes | High |
| FR-28 | System logs all failed login attempts for security auditing | Low |
| FR-29 | Registered user can save properties to favourites list | Low |
| FR-30 | Registered user can view their saved favourites | Low |

---

## Non-Functional Requirements (15+)

| ID | Category | Requirement | Target |
|----|----------|-------------|--------|
| NFR-01 | Performance | Search results must render within 2 seconds | < 2 sec |
| NFR-02 | Performance | Property detail page loads within 1.5 seconds | < 1.5 sec |
| NFR-03 | Performance | API response time for authenticated requests | < 500ms |
| NFR-04 | Performance | Image gallery carousel responds within 300ms | < 300ms |
| NFR-05 | Scalability | System handles 50 concurrent users without degradation | 50 users |
| NFR-06 | Scalability | Database query execution time for search | < 200ms |
| NFR-07 | Security | Passwords stored using bcrypt with 10 salt rounds | bcrypt |
| NFR-08 | Security | JWT tokens stored in HTTP-only cookies (prevents XSS) | HTTP-only |
| NFR-09 | Security | All API endpoints validate input using Joi schema | Input validation |
| NFR-10 | Security | SQL injection prevented using parameterised queries | Knex.js |
| NFR-11 | Security | HTTPS enforcement with TLS 1.2 or higher | TLS 1.2+ |
| NFR-12 | Security | Rate limiting on login endpoint (100 requests per minute) | Rate limit |
| NFR-13 | Usability | WCAG 2.1 AA compliant for colour contrast and keyboard navigation | Level AA |
| NFR-14 | Usability | Mobile-first responsive design with breakpoints at 768px and 1024px | Responsive |
| NFR-15 | Usability | Maximum 3 clicks to access any core feature | 3 clicks |
| NFR-16 | Availability | System uptime during assessment period (9am-5pm weekdays) | 99% |
| NFR-17 | Availability | Mean Time To Recovery (MTTR) | < 30 min |
| NFR-18 | Availability | Daily automated database backups | Daily |
| NFR-19 | Maintainability | Code unit test coverage for backend logic | ≥ 60% |
| NFR-20 | Maintainability | API documentation using Swagger/OpenAPI | Swagger |
| NFR-21 | Maintainability | Code comments for all complex functions | Required |
| NFR-22 | Portability | Deployable via `docker-compose up` on any Linux VM | Docker |
| NFR-23 | Portability | Environment configuration via `.env` files | .env |
| NFR-24 | Portability | Cross-browser compatibility (Chrome, Firefox, Safari, Edge) | All modern |
| NFR-25 | Reliability | No data loss on unexpected server shutdown | ACID compliant |
| NFR-26 | Reliability | Form auto-save for agents creating long listings | Optional |

---

## User Actions Summary

| Persona | Key Actions (Flow) |
|---------|---------------------|
| **Sarah (Buyer)** | Search → Filter → View Map → Click Listing → Save Favourite → Contact Agent |
| **Emma (First-Time Buyer)** | Register → Search by budget → Filter bedrooms → Save favourites → Compare |
| **David (Agent)** | Login → Add Listing → Upload Images → View Dashboard → Edit/Delete |
| **Admin** | Login → View Analytics → Remove Inappropriate Content → Manage Users |

---

## Deliverable Checklist

- [x] Core features identified (10+ features)
- [x] User personas defined (4 personas)
- [x] User stories written (15 stories)
- [x] Functional requirements listed (30 requirements - FR-01 to FR-30)
- [x] Non-functional requirements listed (26 requirements - NFR-01 to NFR-26)

## 8. Conclusion

The Australian property portal market presents a classic duopoly dynamic where two major players (realestate.com.au and Domain) command the vast majority of traffic and revenue. However, persistent consumer pain points—particularly price opacity affecting 63% of listings—combined with rising agent fees and the threat of AI-powered search disruption, create genuine opportunity for a well-positioned challenger.

Emerging platforms like Soho demonstrate that agents will adopt free-to-list models, while consumer demand for deeper, verified data continues to grow. The Aussie Realestate Website project can succeed by focusing on transparency, simplicity, and a clear value proposition that addresses the most significant gap in the current market: **price visibility**.

## Next Step

Proceed to **Week 4 – System Architecture** to design the technical architecture, choose tech stack, and define components.





## 9. References

1. Crazy Domains 2025, *10 Best Real Estate Websites: Discover Your Dream Home*, viewed 16 April 2026, <https://www.crazydomains.com.au/learn/best-real-estate-websites-top-10-platforms-to-find-your-dream-home/>

2. McCabe, J 2026, 'How ChatGPT and social media are dumbing down property investing', *Australian Financial Review*, 23 February, viewed 16 April 2026, <https://afr-prod.ffxblue.com.au/wealth/personal-finance/how-chatgpt-and-social-media-are-dumbing-down-property-investing-20260217-p5o33u>

3. Domain Group 2025, *Domain surges ahead as Australians embrace a fresh alternative in property search*, media release, 19 November, viewed 16 April 2026, <https://www.domain.com.au/group/media-releases/domain-surges-ahead-as-australians-embrace-a-fresh-alternative-in-property-search/>

4. Soho Real Estate 2025, *Free Real Estate Listing Sites in Australia: A 2025 Guide*, viewed 16 April 2026, <https://soho.com.au/articles/free-real-estate-listing-sites-in-australia-a-2025-guide>

5. realestate.com.au 2026, *‘Buying blind’: Expert slams Aussie property tactic*, viewed 16 April 2026, <https://www.realestate.com.au/news/aussie-home-buyers-buying-blind-as-twothirds-of-property-listings-hide-prices/>

6. Coraly 2026, *realestate.com.au vs Domain Group Australia | GPPI Independent Comparison*, viewed 16 April 2026, <https://coraly.ai/gppi/compare/realestate-vs-domai
7. Djolic, M 2026, *The Danger of AI Property Advice: Why 106 Metrics Beat 3 – A Salisbury North Case Study*, HTAG, viewed 16 April 2026, <https://www.htag.com.au/ai-property-investment-tool/>

9. Online Marketplaces 2024, *Analysis: Can Zoopla, Realtor.com and Domain See Off Challengers and Close the Gap to Market Leaders?*, viewed 16 April 2026, <https://www.onlinemarketplaces.com/articles/analysis-can-zoopla-realtor-com-and-domain-see-off-challengers-and-close-the-gap/>

10. uhomes.com 2026, *Which Is the Best Rental Accommodation Website in Australia?*, viewed 16 April 2026, <https://en.uhomes.com/blog/best-rental-websites-australia>

