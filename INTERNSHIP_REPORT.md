# Professional Internship Report

## FreshCart — Online Grocery Delivery Platform

---

**Submitted By:** [Your Name]
**Roll Number:** [Your Roll No.]
**Institution:** [Your College / University]
**Department:** Computer Science & Engineering / Information Technology
**Internship Organization:** [Company Name / Self-Project]
**Duration:** [Start Date] — [End Date] (8 Weeks)
**Mentor / Guide:** [Mentor Name]
**Submission Date:** May 2024

---

## Abstract

This report presents the design, development, and deployment of **FreshCart**, a full-stack grocery delivery platform developed as part of an academic internship project. The application simulates a real-world e-commerce system with three role-based portals—Customer, Admin/Store, and Delivery Agent—built using the **MERN stack** (MongoDB, Express.js, React, Node.js).

The project encompasses core e-commerce functionalities including user authentication, product browsing, shopping cart management, multi-step checkout, delivery slot selection, order lifecycle management, a loyalty rewards system, product ratings, and a real-time admin analytics dashboard. The system serves as a comprehensive demonstration of full-stack web development skills acquired during the internship.

---

## Table of Contents

1. Introduction
2. Problem Statement
3. Objectives
4. System Architecture
5. Technology Stack
6. Database Design
7. API Design
8. Frontend Design
9. Features Implemented
10. Implementation Challenges
11. Testing & Validation
12. Results & Outcomes
13. Learnings
14. Future Enhancements
15. Conclusion
16. References

---

## 1. Introduction

The rapid growth of online grocery shopping, accelerated by the COVID-19 pandemic, has transformed how consumers purchase everyday essentials. Companies like BigBasket, Blinkit, and Swiggy Instamart have proven the viability and demand for on-demand grocery delivery services.

This project, **FreshCart**, is a full-featured grocery delivery platform designed to replicate and understand the technical architecture of such systems. It was developed as a culminating project of the internship program, applying theoretical knowledge of web technologies to a practical, market-relevant application.

FreshCart offers:
- A **customer-facing portal** for browsing, ordering, and tracking groceries
- An **admin portal** for inventory, order, and business analytics management
- A **delivery agent portal** for order fulfillment workflow

---

## 2. Problem Statement

Traditional grocery shopping involves physical visits to stores, which is time-consuming and inconvenient. While several grocery delivery platforms exist, small and medium-sized grocery stores lack the technical infrastructure to offer:
- Online product browsing with advanced filters
- Real-time order tracking
- Flexible delivery scheduling
- Customer loyalty programs
- Analytics for business decisions

FreshCart addresses these gaps by providing a complete, deployable grocery platform.

---

## 3. Objectives

- Design and implement a scalable full-stack grocery delivery application
- Apply RESTful API design principles with proper authentication and authorization
- Implement role-based access control (Customer, Admin, Delivery Agent)
- Build a modern, responsive UI using React and custom CSS
- Develop a loyalty rewards system to improve customer retention
- Create an analytics dashboard for business intelligence
- Demonstrate understanding of database schema design using MongoDB
- Produce GitHub-ready, production-deployable code

---

## 4. System Architecture

### 4.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      CLIENT LAYER                        │
│   React SPA (Customer + Admin + Delivery Portals)       │
│   Vite Build Tool | React Router | Axios HTTP Client    │
└─────────────────────────────┬───────────────────────────┘
                              │ HTTP/REST API
                              │ (JWT Bearer Token)
┌─────────────────────────────▼───────────────────────────┐
│                     API LAYER (Express.js)               │
│   Auth | Products | Cart | Orders | Admin | Delivery    │
│   Ratings | Loyalty | Notifications                      │
│   JWT Middleware | Error Handler | CORS                  │
└─────────────────────────────┬───────────────────────────┘
                              │ Mongoose ODM
┌─────────────────────────────▼───────────────────────────┐
│                   DATABASE LAYER (MongoDB)                │
│   Users | Products | Categories | Cart | Orders         │
│   Ratings | LoyaltyTransactions | Notifications         │
│   DeliverySlots                                          │
└─────────────────────────────────────────────────────────┘
```

### 4.2 Request-Response Flow

1. Client sends HTTP request with JWT Bearer token
2. Express router matches endpoint
3. JWT middleware validates token and attaches user to request
4. Role-based middleware checks authorization
5. Controller executes business logic
6. Mongoose queries MongoDB
7. Response sent as JSON

---

## 5. Technology Stack

| Component | Technology | Version | Rationale |
|---|---|---|---|
| Frontend Framework | React | 18.2 | Component-based, large ecosystem |
| Build Tool | Vite | 5.0 | Fast HMR, ES Module support |
| Routing | React Router | 6.20 | Declarative client-side routing |
| HTTP Client | Axios | 1.6 | Interceptors, error handling |
| State Management | React Context API | — | Sufficient for app scale |
| Charts | Recharts | 2.10 | React-native charting library |
| Icons | React Icons | 4.12 | Feather/Font Awesome icons |
| Notifications | React Hot Toast | 2.4 | Lightweight toast system |
| Backend Framework | Express.js | 4.18 | Minimal, flexible Node.js framework |
| Runtime | Node.js | 18+ | JavaScript server runtime |
| Database | MongoDB | 6.0 | Document-oriented, flexible schema |
| ODM | Mongoose | 8.0 | Schema validation, middleware hooks |
| Authentication | JWT + bcryptjs | 9.0 | Stateless auth, secure password hash |
| Styling | Vanilla CSS | — | Full control, no dependencies |
| Typography | Google Fonts | — | Inter + Poppins fonts |

---

## 6. Database Design

### 6.1 Collections Overview

The application uses **9 MongoDB collections**:

#### users
```javascript
{
  name, email, phone, passwordHash,
  role: ['customer', 'admin', 'delivery'],
  profilePic, savedAddresses: [{ label, street, city, state, pincode, isDefault }],
  loyaltyPoints, isActive, lastLogin, createdAt
}
```

#### categories
```javascript
{ name, slug, icon, image, description, parentCategory, isActive, sortOrder }
```

#### products
```javascript
{
  name, description, category (ref), price, salePrice, unit, unitValue,
  stock, images[], tags[], brand, sku, avgRating, totalReviews,
  isFeatured, isOrganic, isActive, nutritionInfo, totalSold
}
```

#### carts
```javascript
{
  user (ref), items: [{ product (ref), quantity, price, name, image }],
  couponCode, discount
}
```

#### orders
```javascript
{
  user (ref), orderNumber, items[], deliveryAddress{}, deliverySlot{},
  paymentMethod, paymentStatus, status, deliveryAgent (ref),
  subtotal, deliveryFee, discount, loyaltyPointsUsed, total,
  timeline: [{ status, message, timestamp }], specialInstructions
}
```

#### ratings
```javascript
{ user (ref), product (ref), order (ref), rating, title, review, images[], isVerifiedPurchase }
```

#### loyaltyTransactions
```javascript
{ user (ref), points, type: ['earned','redeemed','bonus'], description, order (ref), balance }
```

#### notifications
```javascript
{ user (ref), title, message, type, isRead, link, icon }
```

#### deliverySlots
```javascript
{ date, timeRange, maxOrders, bookedCount, isActive, deliveryFee, isExpress }
```

### 6.2 Database Indexes
- `users.email` — unique index for fast lookup
- `products` — text index on `name, description, tags` for full-text search
- `ratings` — compound unique index on `{product, user}`

---

## 7. API Design

### 7.1 Design Principles
- **RESTful** resource-based URLs
- **JWT Bearer token** authentication
- **Consistent response format**: `{ success, data/message, [pagination] }`
- **Role-based authorization** via middleware
- **Global error handler** with Mongoose error normalization

### 7.2 API Endpoints Summary

**Auth (5 endpoints):** signup, login, getMe, updateProfile, addAddress, deleteAddress, changePassword

**Products (4 endpoints):** getAll (with filters), getOne, getByCategory, getRecommendations

**Cart (5 endpoints):** get, addItem, updateItem, removeItem, clearCart

**Orders (5 endpoints):** create, getMyOrders, getOne, cancel, getDeliverySlots

**Admin (10 endpoints):** analytics, orders CRUD, products CRUD, categories CRUD, inventory, agents

**Delivery (4 endpoints):** getAssigned, getHistory, getStats, updateStatus

**Ratings (2 endpoints):** create, getForProduct

**Loyalty (2 endpoints):** getBalance, getHistory

**Notifications (3 endpoints):** getAll, markRead, markAllRead

---

## 8. Frontend Design

### 8.1 Design System
A custom CSS design system was built from scratch with:
- **CSS Custom Properties** for consistent theming (colors, spacing, shadows, radii)
- **Component classes** for Buttons, Cards, Badges, Forms, Tables, Modals
- **Animation utilities** for micro-interactions and page transitions
- **Skeleton loaders** for improved perceived performance
- **Responsive breakpoints** at 480px, 768px, 1024px

### 8.2 Color Palette
| Token | Value | Usage |
|---|---|---|
| `--primary` | `#00a862` | Brand color, CTAs, active states |
| `--secondary` | `#ff6b35` | Sale badges, discounts |
| `--accent` | `#ffd700` | Loyalty, highlights |
| `--bg` | `#f7f9fc` | Page background |
| `--bg-dark` | `#0d1b2a` | Header, admin sidebar |

### 8.3 Component Architecture
- **AuthContext** — JWT storage, user state, login/logout
- **CartContext** — Cart CRUD, live count, subtotal calculation
- **Header** — Search, cart badge, user dropdown, category nav
- **ProductCard** — Image, badges, inline quantity controls
- **NotificationPanel** — Dropdown with unread indicators

---

## 9. Features Implemented

### 9.1 Authentication & Authorization
- **JWT-based** stateless authentication
- **bcryptjs** for password hashing with salt factor 10
- **Role-based access control**: Customer, Admin, Delivery agent
- Token stored in `localStorage` with auto-attachment via Axios interceptors
- 401 auto-redirect to login

### 9.2 Product Discovery
- **Full-text search** using MongoDB text index
- **Multi-filter**: category, price range, organic, featured
- **Sort options**: popular, rating, newest, price ascending/descending
- **Pagination**: 12-16 items per page
- **Skeleton loading** during data fetch

### 9.3 Shopping Cart
- **Persistent cart** tied to user account in MongoDB
- **Real-time quantity controls** on product cards
- **Stock validation** on add-to-cart
- **Delivery fee**: Free above ₹500, else ₹40

### 9.4 Multi-Step Checkout
**Step 1 — Address**: Select from saved addresses or enter new
**Step 2 — Delivery Slot**: 7-day calendar with 5 time windows per day
**Step 3 — Payment**: COD, UPI, Card, Wallet selection + loyalty redemption
**Step 4 — Review**: Order summary before confirmation

### 9.5 Loyalty Points System
- **Earning**: 5% of order total converted to points on delivery
- **Redemption**: Up to 20% of order value, 1pt = ₹0.25
- **Welcome bonus**: 100 points on signup
- **Tiers**: Bronze (0) → Silver (500) → Gold (2000) → Platinum (5000)
- **Transaction history** with balance tracking

### 9.6 Order Management
- **Order number generation**: Unique alphanumeric code
- **Status timeline**: placed → confirmed → preparing → out_for_delivery → delivered
- **Live timeline** with timestamps
- **Cancellation**: Available for placed/confirmed orders with stock restoration
- **Notifications**: Sent at every status change

### 9.7 Admin Analytics Dashboard
- **Revenue chart**: 30-day bar chart using Recharts
- **Order status pie chart**: Real-time distribution
- **KPI cards**: Monthly revenue, orders, users, products
- **Top products**: By total sold with ratings
- **Low stock alerts**: Products with stock < 10

### 9.8 Delivery Agent Portal
- **Assigned orders**: With customer contact and address
- **Status updates**: One-click status progression
- **Daily stats**: Today's deliveries, pending, total
- **Delivery history**: Completed orders

### 9.9 Product Ratings
- **Star rating**: 1–5 with visual star selector
- **Verified purchase badge**: Only for delivered orders
- **Rating distribution bar chart**: Per product
- **Avg rating recalculation**: Automatic after each review

---

## 10. Implementation Challenges

### Challenge 1: Role-Based Routing
**Problem**: React Router doesn't natively support role-based route protection.
**Solution**: Created `ProtectedRoute` and `PublicRoute` wrapper components that read from AuthContext and redirect unauthorized users.

### Challenge 2: Cart State Synchronization
**Problem**: Cart state needed to be consistent between the database and UI without excessive API calls.
**Solution**: Used CartContext with optimistic updates and fetchCart refresh after mutations.

### Challenge 3: MongoDB Aggregation for Analytics
**Problem**: Complex analytics queries (revenue by category, daily trends) required multi-stage aggregation pipelines.
**Solution**: Implemented `$lookup`, `$unwind`, `$group`, and `$sort` stages in the admin analytics controller.

### Challenge 4: Loyalty Points Consistency
**Problem**: Loyalty points deduction/earning needed to be atomic with order creation.
**Solution**: Handled all loyalty operations within the order creation controller function in sequence, with rollback logic for cancellations.

### Challenge 5: Delivery Slot Management
**Problem**: Preventing overbooking of delivery slots.
**Solution**: Stored `bookedCount` and `maxOrders` in DeliverySlot collection, filtered slots with remaining capacity, and incremented count atomically on order creation.

---

## 11. Testing & Validation

### 11.1 Manual Testing Performed

| Test Case | Status |
|---|---|
| User signup with welcome bonus | ✅ Pass |
| Login with invalid credentials | ✅ Pass |
| Add to cart while unauthenticated → redirect to login | ✅ Pass |
| Place order → stock decremented | ✅ Pass |
| Cancel order → stock restored | ✅ Pass |
| Admin update order status → customer notification | ✅ Pass |
| Delivery agent update status | ✅ Pass |
| Loyalty points earned on order | ✅ Pass |
| Loyalty points redeemed at checkout | ✅ Pass |
| Product text search | ✅ Pass |
| Category filter on products | ✅ Pass |
| Admin create product → visible on frontend | ✅ Pass |
| Rating submitted → avg rating updated | ✅ Pass |
| Responsive layout on mobile | ✅ Pass |

### 11.2 API Health Check

```bash
curl http://localhost:5000/api/health
# Response: { "success": true, "message": "FreshCart API is running 🚀" }
```

---

## 12. Results & Outcomes

### 12.1 Deliverables Produced

| Deliverable | Status |
|---|---|
| Backend REST API (30+ endpoints) | ✅ Complete |
| MongoDB Schema (9 collections) | ✅ Complete |
| Customer Portal (11 pages) | ✅ Complete |
| Admin Portal (5 pages + sidebar) | ✅ Complete |
| Delivery Portal (1 page, 3 tabs) | ✅ Complete |
| Database Seed Script | ✅ Complete |
| README.md | ✅ Complete |
| Deployment Guide | ✅ Complete |
| Internship Report | ✅ Complete |

### 12.2 Technical Metrics

| Metric | Value |
|---|---|
| Total source files | 45+ |
| Total lines of code | ~6,000 |
| API endpoints | 35+ |
| React components/pages | 25+ |
| Database collections | 9 |
| Seed products | 35+ |
| CSS custom properties | 40+ |

---

## 13. Learnings

### Technical Skills Gained

**Backend Development:**
- Designing scalable REST APIs with Express.js
- MongoDB schema design, indexing, and aggregation pipelines
- JWT-based stateless authentication and role-based access control
- Password hashing with bcryptjs
- Mongoose virtuals, middleware (pre-save hooks), and population
- Centralized error handling with custom error normalization

**Frontend Development:**
- React component architecture and composition patterns
- Context API for global state management (Auth, Cart)
- Axios interceptors for token attachment and 401 handling
- React Router with protected and public route wrappers
- Building a CSS design system from scratch with custom properties
- Responsive design with mobile-first CSS
- Data visualization with Recharts

**System Design:**
- Designing role-based multi-portal applications
- E-commerce domain modeling (cart, orders, inventory)
- Loyalty program mechanics and gamification
- Analytics aggregation for business intelligence

**Software Engineering:**
- Git version control best practices
- Environment variable management
- API documentation conventions
- Production deployment strategies (PM2, Nginx, SSL)

### Soft Skills
- Project planning with a phased implementation approach
- Breaking complex features into manageable tasks
- Self-directed learning of new technologies (Recharts, React Router 6)
- Writing clear technical documentation

---

## 14. Future Enhancements

| Enhancement | Priority | Description |
|---|---|---|
| Real-time notifications | High | WebSocket / Socket.io for live order updates |
| Payment gateway | High | Razorpay / Stripe integration for real payments |
| Image upload | High | Multer + Cloudinary for product images |
| Push notifications | Medium | Firebase FCM for mobile push |
| Coupon/Promo system | Medium | Discount codes with usage limits |
| Search optimization | Medium | Elasticsearch for advanced search |
| Product recommendations | Medium | Collaborative filtering ML model |
| Mobile app | Low | React Native version |
| Multi-vendor support | Low | Allow multiple store owners |
| Review photos | Low | Image upload on product reviews |
| Subscription orders | Low | Weekly auto-repeat grocery orders |
| Dark mode | Low | System preference-aware dark theme |

---

## 15. Conclusion

The FreshCart project successfully demonstrates the design and implementation of a production-grade, full-stack grocery delivery platform. The application covers the complete e-commerce lifecycle—from product discovery and cart management to order placement, delivery tracking, and business analytics.

Through this internship project, I developed practical proficiency in the MERN stack, RESTful API design, database schema architecture, and modern React development patterns. The project also provided exposure to real-world challenges such as concurrency (slot booking), data consistency (loyalty transactions), and role-based system design.

FreshCart is a comprehensive, GitHub-ready codebase that demonstrates industry-standard practices in full-stack web development and serves as a strong portfolio piece for software engineering roles.

---

## 16. References

1. MongoDB Official Documentation — https://docs.mongodb.com
2. Express.js API Reference — https://expressjs.com/en/api.html
3. React Documentation — https://react.dev
4. Vite Guide — https://vitejs.dev/guide
5. React Router v6 Documentation — https://reactrouter.com/en/main
6. JSON Web Tokens (JWT) — https://jwt.io/introduction
7. Recharts Documentation — https://recharts.org/en-US/api
8. Mongoose ODM Guide — https://mongoosejs.com/docs/guide.html
9. React Hot Toast — https://react-hot-toast.com
10. Axios Documentation — https://axios-http.com/docs/intro
11. Node.js Best Practices — https://github.com/goldbergyoni/nodebestpractices
12. MDN Web Docs CSS — https://developer.mozilla.org/en-US/docs/Web/CSS
13. bcryptjs Package — https://www.npmjs.com/package/bcryptjs
14. PM2 Process Manager — https://pm2.keymetrics.io/docs
15. Nginx Documentation — https://nginx.org/en/docs

---

*This report was prepared as part of the Full-Stack Web Development Internship Program.*

**Declaration:** I hereby declare that this internship report is my own work and has not been submitted elsewhere for academic credit. All external references have been cited appropriately.

**Signature:** ___________________
**Date:** ___________________
