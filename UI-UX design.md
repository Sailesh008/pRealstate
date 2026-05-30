
> **Deliverable:** Wireframes and Screen Designs  
> **Activities:** Create wireframes · Define user flows · Design UI components

---

## Table of Contents

- [Overview](#overview)
- [Design Principles](#design-principles)
- [User Personas](#user-personas)
- [Information Architecture](#information-architecture)
- [User Flows](#user-flows)
- [Main Screens & Wireframes](#main-screens--wireframes)
- [UI Component Library](#ui-component-library)
- [Navigation Structure](#navigation-structure)
- [Responsive Breakpoints](#responsive-breakpoints)
- [Accessibility Standards](#accessibility-standards)
- [Design Tokens](#design-tokens)
- [Tooling & Assets](#tooling--assets)

---

## Overview

| Item               | Details                                      |
|--------------------|----------------------------------------------|
| **Design Tool**    | Figma / Adobe XD / Sketch                   |
| **Prototype URL**  | https://figma.com/proto/your-project-link   |
| **Design System**  | Custom / Tailwind / Material Design 3        |
| **Platforms**      | Web (Desktop + Mobile), iOS, Android         |
| **Primary Users**  | Customers, Admins                            |
| **Accessibility**  | WCAG 2.1 Level AA                            |

---

## Design Principles

| #  | Principle         | Description                                                                 |
|----|-------------------|-----------------------------------------------------------------------------|
| 1  | **Clarity**       | Every screen has one primary action. No cognitive overload.                  |
| 2  | **Consistency**   | Components, spacing, and language behave the same across all screens.        |
| 3  | **Feedback**      | Every user action receives immediate visual or textual feedback.             |
| 4  | **Accessibility** | Legible contrast, keyboard navigation, and screen-reader-friendly markup.    |
| 5  | **Mobile-first**  | Designed for small screens first, progressively enhanced for larger ones.    |
| 6  | **Progressive Disclosure** | Show only what users need at each step; reveal complexity on demand. |

---

## User Personas

### Persona 1 — The Everyday Customer

```
┌─────────────────────────────────────────────────────┐
│  👤  Jamie, 28 — Online Shopper                     │
│─────────────────────────────────────────────────────│
│  Goals:                                             │
│   • Quickly find and purchase what they need        │
│   • Track order status easily                       │
│   • Leave feedback on purchases                     │
│                                                     │
│  Pain Points:                                       │
│   • Long checkout flows                             │
│   • Confusing navigation                            │
│   • Poor mobile experience                         │
│                                                     │
│  Device:  Primarily mobile (iPhone)                 │
│  Tech:    Comfortable with apps; avoids manuals     │
└─────────────────────────────────────────────────────┘
```

### Persona 2 — The Admin / Store Manager

```
┌─────────────────────────────────────────────────────┐
│  👤  Morgan, 35 — Operations Manager                │
│─────────────────────────────────────────────────────│
│  Goals:                                             │
│   • Manage products and inventory quickly           │
│   • Monitor order pipeline                         │
│   • View sales metrics at a glance                  │
│                                                     │
│  Pain Points:                                       │
│   • Too many clicks to update a product             │
│   • No clear overview of pending orders             │
│   • Lack of bulk action support                     │
│                                                     │
│  Device:  Desktop (Chrome on Windows)               │
│  Tech:    Power user; comfortable with dashboards   │
└─────────────────────────────────────────────────────┘
```

---

## Information Architecture

```
App
├── Public (Unauthenticated)
│   ├── Home / Landing Page
│   ├── Product Listing
│   ├── Product Detail
│   ├── Search Results
│   ├── Login
│   └── Register
│
├── Customer (Authenticated)
│   ├── Dashboard / Home Feed
│   ├── Browse Products
│   │   ├── Category Filter
│   │   └── Search
│   ├── Product Detail
│   │   └── Write Review
│   ├── Cart
│   ├── Checkout
│   │   ├── Shipping Details
│   │   ├── Payment
│   │   └── Order Confirmation
│   ├── My Orders
│   │   └── Order Detail
│   └── My Profile
│       └── Edit Profile
│
└── Admin (Authenticated + Admin Role)
    ├── Admin Dashboard
    ├── Product Management
    │   ├── Product List
    │   ├── Add Product
    │   └── Edit Product
    ├── Order Management
    │   ├── Order List
    │   └── Order Detail / Status Update
    ├── User Management
    │   └── User List
    └── Settings
```

---

## User Flows

### Flow 1 — New User Registration & First Purchase

```
[Landing Page]
      │
      ▼
[Register Page] ──► Enter email + password + name
      │
      ▼
[Email Verified] ──► (optional confirmation step)
      │
      ▼
[Home / Product Listing]
      │
      ▼
[Product Detail] ──► View details, read reviews
      │
      ▼
[Add to Cart] ──► Cart updates in header
      │
      ▼
[Cart Page] ──► Review items, adjust quantities
      │
      ▼
[Checkout — Step 1: Shipping]
      │
      ▼
[Checkout — Step 2: Payment]
      │
      ▼
[Order Confirmation] ──► Email sent, order ID shown
      │
      ▼
[My Orders] ──► Track order status
```

---

### Flow 2 — Returning Customer Re-order

```
[Login Page] ──► Enter credentials
      │
      ▼
[My Orders] ──► Find previous order
      │
      ▼
[Order Detail] ──► Click "Re-order"
      │
      ▼
[Cart] ──► Items pre-filled
      │
      ▼
[Checkout] ──► Saved address pre-filled
      │
      ▼
[Order Confirmation]
```

---

### Flow 3 — Admin Publishes a New Product

```
[Admin Dashboard]
      │
      ▼
[Product Management → Product List]
      │
      ▼
[Add Product] ──► Fill name, price, category, description
      │
      ▼
[Upload Images] ──► Drag & drop or file picker
      │
      ▼
[Set Tags & Stock] ──► Add searchable tags, enter quantity
      │
      ▼
[Preview] ──► See how product appears publicly
      │
      ▼
[Publish] ──► is_published = true → live on site
```

---

### Flow 4 — Order Status Update (Admin)

```
[Admin Dashboard] ──► Pending orders badge
      │
      ▼
[Order List] ──► Filter by "pending"
      │
      ▼
[Order Detail] ──► Review items + customer info
      │
      ▼
[Update Status] ──► Select: confirmed → shipped → delivered
      │
      ▼
[Confirmation Toast] ──► "Order #1234 marked as Shipped"
      │
      ▼
[Customer notified] ──► Email / in-app notification sent
```

---

## Main Screens & Wireframes

> ASCII wireframes illustrate layout and hierarchy. Replace with Figma link for hi-fi designs.

---

### Screen 1 — Landing / Home Page

```
┌──────────────────────────────────────────────────────────────┐
│ LOGO                          [Search ____]  [Login] [Cart🛒]│
├──────────────────────────────────────────────────────────────┤
│                                                              │
│              ┌────────────────────────────────┐             │
│              │                                │             │
│              │       HERO BANNER IMAGE        │             │
│              │   "Find What You're Looking For"│             │
│              │    [Shop Now]  [Browse Deals]  │             │
│              │                                │             │
│              └────────────────────────────────┘             │
│                                                              │
│  FEATURED CATEGORIES                                         │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  [icon]  │  │  [icon]  │  │  [icon]  │  │  [icon]  │   │
│  │Electronics│  │ Clothing │  │   Home   │  │  Sports  │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│                                                              │
│  TRENDING PRODUCTS                            [View All →]  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  [img]   │  │  [img]   │  │  [img]   │  │  [img]   │   │
│  │ Product  │  │ Product  │  │ Product  │  │ Product  │   │
│  │  $29.99  │  │  $49.99  │  │  $19.99  │  │  $89.99  │   │
│  │[Add Cart]│  │[Add Cart]│  │[Add Cart]│  │[Add Cart]│   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│                                                              │
│  [Footer: Links · Social · Copyright]                        │
└──────────────────────────────────────────────────────────────┘
```

---

### Screen 2 — Product Listing Page

```
┌──────────────────────────────────────────────────────────────┐
│ LOGO                          [Search ____]  [Login] [Cart🛒]│
├──────────────────────────────────────────────────────────────┤
│  Electronics (120 results)          Sort by: [Newest ▼]      │
│  ┌─────────────────┬──────────────────────────────────────┐  │
│  │  FILTERS        │  PRODUCT GRID                        │  │
│  │                 │  ┌────────┐  ┌────────┐  ┌────────┐ │  │
│  │  Category       │  │ [img]  │  │ [img]  │  │ [img]  │ │  │
│  │  ○ Electronics  │  │ Name   │  │ Name   │  │ Name   │ │  │
│  │  ○ Clothing     │  │ ★★★★☆ │  │ ★★★☆☆ │  │ ★★★★★ │ │  │
│  │  ○ Home         │  │ $29.99 │  │ $49.99 │  │ $19.99 │ │  │
│  │                 │  │[+ Cart]│  │[+ Cart]│  │[+ Cart]│ │  │
│  │  Price Range    │  └────────┘  └────────┘  └────────┘ │  │
│  │  $[___] – $[___]│                                      │  │
│  │                 │  ┌────────┐  ┌────────┐  ┌────────┐ │  │
│  │  Rating         │  │ [img]  │  │ [img]  │  │ [img]  │ │  │
│  │  ☆☆☆☆☆ & up   │  │ Name   │  │ Name   │  │ Name   │ │  │
│  │  ★☆☆☆☆ & up   │  │ ★★★★☆ │  │ ★★★☆☆ │  │ ★★★★★ │ │  │
│  │  ★★☆☆☆ & up   │  │ $89.99 │  │ $14.99 │  │ $59.99 │ │  │
│  │                 │  │[+ Cart]│  │[+ Cart]│  │[+ Cart]│ │  │
│  │  Tags           │  └────────┘  └────────┘  └────────┘ │  │
│  │  [ ] Sale       │                                      │  │
│  │  [ ] Featured   │       ← 1  2  3  4  5 →             │  │
│  │  [ ] New        │                                      │  │
│  └─────────────────┴──────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

---

### Screen 3 — Product Detail Page

```
┌──────────────────────────────────────────────────────────────┐
│ LOGO                          [Search ____]  [Login] [Cart🛒]│
├──────────────────────────────────────────────────────────────┤
│  Home > Electronics > Product Name                           │
│                                                              │
│  ┌─────────────────────┐   ┌──────────────────────────────┐  │
│  │                     │   │  Product Name                │  │
│  │   MAIN PRODUCT      │   │  ★★★★☆  (43 reviews)        │  │
│  │      IMAGE          │   │                              │  │
│  │                     │   │  $49.99                      │  │
│  │                     │   │  ✅ In Stock (28 remaining)  │  │
│  ├─────┬──────┬────────┤   │                              │  │
│  │[img]│ [img]│  [img] │   │  Qty: [−] 1 [+]             │  │
│  └─────┴──────┴────────┘   │                              │  │
│                             │  [    Add to Cart    ]       │  │
│                             │  [      Wishlist ♡   ]       │  │
│                             │                              │  │
│                             │  Category: Electronics       │  │
│                             │  Tags: #sale #featured       │  │
│                             └──────────────────────────────┘  │
│                                                              │
│  DESCRIPTION                                                 │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  Full product description text goes here. Multiple     │  │
│  │  paragraphs, bullet features, technical specifications. │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│  CUSTOMER REVIEWS  ★★★★☆ 4.3 avg  (43 reviews)             │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  ★★★★★  "Great product!" — Jamie, 2 days ago          │  │
│  │  ★★★★☆  "Good value for money" — Alex, 1 week ago     │  │
│  │  ★★★☆☆  "Decent but slow shipping" — Sam, 2 weeks ago │  │
│  │                                    [Load more reviews] │  │
│  └────────────────────────────────────────────────────────┘  │
│  [Write a Review]                                            │
└──────────────────────────────────────────────────────────────┘
```

---

### Screen 4 — Cart Page

```
┌──────────────────────────────────────────────────────────────┐
│ LOGO                          [Search ____]  [Login] [Cart🛒]│
├──────────────────────────────────────────────────────────────┤
│  🛒 Your Cart (3 items)                                      │
│  ┌──────────────────────────────────┬─────────────────────┐  │
│  │  CART ITEMS                      │  ORDER SUMMARY      │  │
│  │                                  │─────────────────────│  │
│  │  ┌────┬──────────────┬──┬──────┐ │  Subtotal    $89.97 │  │
│  │  │img │ Product A    │  │$29.99│ │  Shipping    $5.00  │  │
│  │  │    │ Qty: [−] 2[+]│  │      │ │  Tax         $8.99  │  │
│  │  │    │ [🗑 Remove]  │  │      │ │─────────────────────│  │
│  │  └────┴──────────────┴──┴──────┘ │  Total      $103.96 │  │
│  │                                  │                     │  │
│  │  ┌────┬──────────────┬──┬──────┐ │  [  Proceed to      │  │
│  │  │img │ Product B    │  │$49.99│ │     Checkout  ]     │  │
│  │  │    │ Qty: [−] 1[+]│  │      │ │                     │  │
│  │  │    │ [🗑 Remove]  │  │      │ │  [Continue Shopping]│  │
│  │  └────┴──────────────┴──┴──────┘ │                     │  │
│  │                                  │  🔒 Secure checkout  │  │
│  │  Promo Code: [__________] [Apply]│  Visa 💳 Mastercard  │  │
│  └──────────────────────────────────┴─────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

---

### Screen 5 — Checkout (3-Step)

```
Step Indicator:
  [1 Shipping] ──── [2 Payment] ──── [3 Confirm]
       ●                ○                ○

┌──────────────────────────────────────────────────────────────┐
│  STEP 1: Shipping Details                                    │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  First Name [________________]                         │  │
│  │  Last Name  [________________]                         │  │
│  │  Address    [________________________________]          │  │
│  │  City       [______________]  State [_______]          │  │
│  │  Postcode   [________]  Country [Australia ▼]          │  │
│  │  Phone      [________________]                         │  │
│  │                                                        │  │
│  │  Delivery Method                                       │  │
│  │  ● Standard (3–5 days)  $5.00                         │  │
│  │  ○ Express  (1–2 days)  $14.99                        │  │
│  │                                                        │  │
│  │                    [Continue to Payment →]             │  │
│  └────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

---

### Screen 6 — Order Confirmation

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│                    ✅                                        │
│           Order Placed Successfully!                        │
│                                                              │
│           Order #ORD-2026-00847                             │
│           Confirmation sent to jane@example.com             │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  2× Product A                               $59.98     │  │
│  │  1× Product B                               $49.99     │  │
│  │  Shipping (Standard)                         $5.00     │  │
│  │──────────────────────────────────────────────────────  │  │
│  │  Total Charged                             $114.97     │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│             [  Track My Order  ]                             │
│             [  Continue Shopping  ]                          │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

### Screen 7 — Customer Order History

```
┌──────────────────────────────────────────────────────────────┐
│ LOGO                          [Search ____]  [Jane ▼][Cart🛒]│
├──────────────────────────────────────────────────────────────┤
│  My Orders          Filter: [All Status ▼]  [Search orders] │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  #ORD-2026-00847    30 May 2026    $114.97             │  │
│  │  Status: ● Shipped                    [View Details →] │  │
│  ├────────────────────────────────────────────────────────┤  │
│  │  #ORD-2026-00721    18 May 2026     $29.99             │  │
│  │  Status: ● Delivered                  [View Details →] │  │
│  ├────────────────────────────────────────────────────────┤  │
│  │  #ORD-2026-00610    05 May 2026     $49.99             │  │
│  │  Status: ✗ Cancelled                  [View Details →] │  │
│  └────────────────────────────────────────────────────────┘  │
│                          ← 1  2 →                            │
└──────────────────────────────────────────────────────────────┘
```

---

### Screen 8 — Admin Dashboard

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  AdminPanel    Products  Orders  Users  Settings   Morgan ▼│
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Good morning, Morgan 👋  Here's today's snapshot.          │
│                                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  $8,420  │  │    142   │  │    18    │  │    4     │   │
│  │  Revenue │  │  Orders  │  │ Pending  │  │Low Stock │   │
│  │  Today   │  │  Today   │  │  Orders  │  │ Items    │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│                                                              │
│  RECENT ORDERS                          [View All Orders →] │
│  ┌───────────┬──────────────┬──────────┬─────────┬────────┐ │
│  │ Order ID  │ Customer     │ Total    │ Status  │ Action │ │
│  ├───────────┼──────────────┼──────────┼─────────┼────────┤ │
│  │ #847      │ Jane Doe     │ $114.97  │ Shipped │ [View] │ │
│  │ #846      │ Alex Smith   │  $29.99  │ Pending │ [View] │ │
│  │ #845      │ Sam Lee      │  $79.00  │ Confirmed│[View] │ │
│  └───────────┴──────────────┴──────────┴─────────┴────────┘ │
│                                                              │
│  SALES CHART (Last 7 Days)                                   │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  $                                                     │  │
│  │  10k │          ████                                   │  │
│  │   8k │     ████ ████ ████                              │  │
│  │   6k │████ ████ ████ ████ ████                         │  │
│  │   4k │████ ████ ████ ████ ████ ████                    │  │
│  │      └─────────────────────────────────                │  │
│  │       Mon  Tue  Wed  Thu  Fri  Sat  Sun                │  │
│  └────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

---

### Screen 9 — Admin Product Management

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  AdminPanel    Products  Orders  Users  Settings   Morgan ▼│
├──────────────────────────────────────────────────────────────┤
│  Products                                 [+ Add Product]    │
│  [Search products ________]  Filter: [All Categories ▼]      │
│  ┌──────┬────────────────┬──────┬───────┬───────────┬──────┐ │
│  │ IMG  │ Name           │ Price│ Stock │ Status    │ Act  │ │
│  ├──────┼────────────────┼──────┼───────┼───────────┼──────┤ │
│  │ [▪]  │ Product Alpha  │$49.99│  85   │ ● Live    │✏️ 🗑│ │
│  │ [▪]  │ Product Beta   │$29.99│   4   │ ⚠ Low Stock│✏️ 🗑│ │
│  │ [▪]  │ Product Gamma  │$89.99│   0   │ ✗ Draft   │✏️ 🗑│ │
│  │ [▪]  │ Product Delta  │$14.99│ 200   │ ● Live    │✏️ 🗑│ │
│  └──────┴────────────────┴──────┴───────┴───────────┴──────┘ │
│  Showing 1–4 of 24                     ← 1  2  3  4  5  6 → │
└──────────────────────────────────────────────────────────────┘
```

---

### Screen 10 — Mobile Home (375px)

```
┌─────────────────────┐
│ ☰ LOGO        🔍 🛒 │
├─────────────────────┤
│ ┌─────────────────┐ │
│ │  HERO BANNER    │ │
│ │  Shop Now       │ │
│ │  [Shop Now]     │ │
│ └─────────────────┘ │
│                     │
│ CATEGORIES          │
│ ┌────┐ ┌────┐       │
│ │icon│ │icon│  →    │
│ │Elec│ │Clth│       │
│ └────┘ └────┘       │
│                     │
│ TRENDING            │
│ ┌──────────┐        │
│ │  [img]   │        │
│ │ Product  │        │
│ │  $29.99  │        │
│ │[Add Cart]│        │
│ └──────────┘        │
│ ┌──────────┐        │
│ │  [img]   │        │
│ │ Product  │        │
│ │  $49.99  │        │
│ │[Add Cart]│        │
│ └──────────┘        │
│                     │
│ 🏠  🔍  🛒  👤     │
└─────────────────────┘
```

---

## UI Component Library

### Buttons

| Variant     | Usage                              | Style                               |
|-------------|------------------------------------|-------------------------------------|
| Primary     | Main CTA (Add to Cart, Checkout)   | Filled, brand colour, rounded       |
| Secondary   | Cancel, go back actions            | Outlined, transparent fill          |
| Destructive | Delete, cancel order               | Red fill or red border              |
| Ghost       | Low-emphasis actions               | No border, text only                |
| Icon        | Cart, wishlist, delete row         | Icon only, square, accessible label |

---

### Form Inputs

| Component      | States                                    |
|----------------|-------------------------------------------|
| Text Input     | Default · Focus · Error · Disabled        |
| Select/Dropdown| Default · Open · Selected · Disabled      |
| Checkbox       | Unchecked · Checked · Indeterminate       |
| Radio Button   | Unselected · Selected · Disabled          |
| Textarea       | Default · Focus · Character count         |
| File Upload    | Idle · Drag-over · Uploading · Uploaded   |

---

### Feedback Components

| Component       | Trigger                                    |
|-----------------|--------------------------------------------|
| Toast / Snackbar| Success actions, error alerts (auto-dismiss)|
| Inline Error    | Form validation failures (below field)     |
| Empty State     | No results, no orders, no reviews          |
| Loading Spinner | API calls, page loads                      |
| Skeleton Screen | Content loading placeholder                |
| Progress Bar    | Checkout steps, file upload progress       |

---

### Data Display

| Component       | Used On                                    |
|-----------------|--------------------------------------------|
| Product Card    | Listing page, search results, home feed    |
| Order Row       | Order history, admin order list            |
| Review Card     | Product detail page                        |
| Stats Card      | Admin dashboard metrics                    |
| Data Table      | Admin product/order/user lists             |
| Badge / Pill    | Order status, stock status, tags           |
| Star Rating     | Product cards, review list                 |
| Pagination      | All list views                             |

---

### Navigation

| Component       | Used On                                    |
|-----------------|--------------------------------------------|
| Top Navigation  | All pages (desktop)                        |
| Bottom Tab Bar  | All pages (mobile)                         |
| Sidebar         | Admin panel                                |
| Breadcrumbs     | Product detail, order detail               |
| Dropdown Menu   | User profile menu                          |
| Mobile Drawer   | Category filters, mobile nav               |

---

## Navigation Structure

### Desktop Navigation (Top Bar)

```
[Logo]   [Home]  [Products]  [Categories ▼]  [Search ________]  [Login / User ▼]  [Cart 🛒 3]
```

### Mobile Navigation (Bottom Tab Bar)

```
[🏠 Home]   [🔍 Search]   [🛒 Cart]   [📦 Orders]   [👤 Profile]
```

### Admin Sidebar

```
┌──────────────┐
│  AdminPanel  │
├──────────────┤
│ 📊 Dashboard │
│ 📦 Products  │
│ 🛒 Orders    │
│ 👤 Users     │
│ ⚙️  Settings  │
├──────────────┤
│  Morgan ▼    │
│  [Logout]    │
└──────────────┘
```

---

## Responsive Breakpoints

| Breakpoint | Width       | Layout Description                                    |
|------------|-------------|-------------------------------------------------------|
| **xs**     | < 480px     | Single column · Bottom nav · Stacked cards             |
| **sm**     | 480–767px   | Single column · 2-col product grid                    |
| **md**     | 768–1023px  | Two column · Top nav · Sidebar filters collapsible    |
| **lg**     | 1024–1279px | Three column product grid · Persistent sidebar         |
| **xl**     | ≥ 1280px    | Four column product grid · Full admin dashboard        |

---

## Accessibility Standards

> Target: **WCAG 2.1 Level AA**

| Requirement          | Implementation                                               |
|----------------------|--------------------------------------------------------------|
| **Colour Contrast**  | Minimum 4.5:1 ratio for normal text, 3:1 for large text      |
| **Keyboard Nav**     | All interactive elements reachable and operable via keyboard  |
| **Focus Indicators** | Visible focus ring on all focusable elements                  |
| **Screen Readers**   | Semantic HTML, ARIA labels, alt text on all images            |
| **Form Labels**      | Every input has an associated `<label>` element               |
| **Error Messages**   | Errors are linked to their field via `aria-describedby`       |
| **Touch Targets**    | Minimum 44×44px tap target on mobile                          |
| **Skip Links**       | "Skip to main content" link at top of every page              |
| **Motion**           | Animations respect `prefers-reduced-motion` media query       |

---

## Design Tokens

```css
/* ── Colours ──────────────────────────────────────────── */
--color-primary:        #2563EB;   /* Blue 600 — buttons, links   */
--color-primary-hover:  #1D4ED8;   /* Blue 700 — hover state      */
--color-success:        #16A34A;   /* Green 600 — confirmed, live */
--color-warning:        #D97706;   /* Amber 600 — low stock       */
--color-danger:         #DC2626;   /* Red 600 — errors, delete    */
--color-neutral-50:     #F9FAFB;   /* Backgrounds                 */
--color-neutral-100:    #F3F4F6;   /* Card backgrounds            */
--color-neutral-700:    #374151;   /* Body text                   */
--color-neutral-900:    #111827;   /* Headings                    */

/* ── Typography ───────────────────────────────────────── */
--font-sans:     'Inter', system-ui, sans-serif;
--text-xs:       0.75rem;    /* 12px */
--text-sm:       0.875rem;   /* 14px */
--text-base:     1rem;       /* 16px */
--text-lg:       1.125rem;   /* 18px */
--text-xl:       1.25rem;    /* 20px */
--text-2xl:      1.5rem;     /* 24px */
--text-3xl:      1.875rem;   /* 30px */

/* ── Spacing ──────────────────────────────────────────── */
--space-1:  0.25rem;   /* 4px  */
--space-2:  0.5rem;    /* 8px  */
--space-3:  0.75rem;   /* 12px */
--space-4:  1rem;      /* 16px */
--space-6:  1.5rem;    /* 24px */
--space-8:  2rem;      /* 32px */
--space-12: 3rem;      /* 48px */
--space-16: 4rem;      /* 64px */

/* ── Radius ───────────────────────────────────────────── */
--radius-sm:   0.25rem;   /* Tags, badges     */
--radius-md:   0.5rem;    /* Inputs, cards    */
--radius-lg:   0.75rem;   /* Modals, panels   */
--radius-full: 9999px;    /* Pills, avatars   */

/* ── Shadows ──────────────────────────────────────────── */
--shadow-sm:  0 1px 2px rgba(0,0,0,0.05);
--shadow-md:  0 4px 6px rgba(0,0,0,0.07);
--shadow-lg:  0 10px 15px rgba(0,0,0,0.1);
```

---

## Tooling & Assets

| Tool / Resource       | Purpose                                             |
|-----------------------|-----------------------------------------------------|
| **Figma**             | Hi-fi designs, prototyping, component library       |
| **Figma Tokens**      | Sync design tokens to code                          |
| **Storybook**         | Component documentation and visual testing          |
| **Unsplash / Pexels** | Placeholder images for wireframes                   |
| **Heroicons**         | Open-source SVG icon set                            |
| **Lottie**            | Micro-animations (empty states, success screens)    |
| **Contrast Checker**  | https://webaim.org/resources/contrastchecker        |
| **WAVE**              | Accessibility audit tool                            |
| **Lighthouse**        | Performance, accessibility, and SEO audit           |

### Figma File Structure

```
📁 UI/UX Project
 ├── 📄 Cover & Index
 ├── 📄 Design Tokens
 ├── 📄 Component Library
 │    ├── Buttons
 │    ├── Inputs
 │    ├── Cards
 │    ├── Navigation
 │    └── Feedback
 ├── 📄 Wireframes (Lo-Fi)
 ├── 📄 Screen Designs (Hi-Fi)
 │    ├── Public Screens
 │    ├── Customer Screens
 │    └── Admin Screens
 ├── 📄 User Flows
 └── 📄 Prototype Links
```

---

*Week 6 Deliverable · Last updated: May 2026 · Maintained by the design team*

