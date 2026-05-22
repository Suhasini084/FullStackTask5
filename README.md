# 🛒 FreshCart — Grocery Delivery Platform

> A full-stack, production-grade **Grocery Delivery Application** with three portals: **Customer**, **Admin/Store**, and **Delivery Agent**. Built with React, Node.js, Express, and MongoDB.

![FreshCart](https://images.unsplash.com/photo-1542838132-92c53300491e?w=1200&h=400&fit=crop)

---

## ✨ Features

### 🧑‍💼 Customer Portal
| Feature | Description |
|---|---|
| Auth | JWT-based Signup / Login with bcrypt hashing |
| Browse | 5000+ products with filters, search, and sort |
| Categories | 12 categories with icon navigation |
| Cart | Persistent cart with real-time qty controls |
| Checkout | Multi-step: Address → Slot → Payment → Review |
| Delivery Slots | 7-day slot picker with availability |
| Payment | COD, UPI, Card, Wallet support |
| Order History | Status tracking with live timeline |
| Saved Addresses | Multiple saved delivery addresses |
| Loyalty | Bronze → Silver → Gold → Platinum tier system |
| Ratings | Product reviews with verified purchase badge |
| Notifications | Real-time order and offer notifications |
| Recommendations | Personalized based on order history |

### 🔐 Admin Portal
| Feature | Description |
|---|---|
| Analytics | Revenue charts, order trends, category breakdown |
| Products | Full CRUD with image, pricing, stock management |
| Orders | Status management with agent assignment |
| Inventory | Low stock alerts with inline stock editing |
| Categories | Emoji-based category management |
| Delivery Agents | Assign agents to orders |

### 🚚 Delivery Portal
| Feature | Description |
|---|---|
| Agent Dashboard | Stats: today's deliveries, pending, total |
| Active Orders | Assigned orders with customer details |
| Status Updates | One-click Out for Delivery → Delivered |
| History | Complete delivery history |

---

## 🏗️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 18 + Vite, React Router, Recharts |
| **Backend** | Node.js + Express 4 |
| **Database** | MongoDB + Mongoose ODM |
| **Auth** | JWT + bcryptjs |
| **State** | React Context API |
| **HTTP Client** | Axios with interceptors |
| **UI** | Custom CSS Design System (no framework) |
| **Icons** | React Icons |
| **Charts** | Recharts |
| **Notifications** | React Hot Toast |

---

## 📁 Project Structure

```
FreshCart/
├── backend/
│   ├── config/
│   │   ├── db.js              # MongoDB connection
│   │   └── seed.js            # Database seeder
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── productController.js
│   │   ├── cartController.js
│   │   ├── orderController.js
│   │   ├── adminController.js
│   │   ├── deliveryController.js
│   │   ├── ratingController.js
│   │   ├── loyaltyController.js
│   │   └── notificationController.js
│   ├── middleware/
│   │   ├── auth.js            # JWT middleware + RBAC
│   │   └── error.js           # Global error handler
│   ├── models/
│   │   ├── User.js
│   │   ├── Product.js
│   │   ├── Category.js
│   │   ├── Cart.js
│   │   ├── Order.js
│   │   ├── Rating.js
│   │   ├── LoyaltyTransaction.js
│   │   ├── Notification.js
│   │   └── DeliverySlot.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── products.js
│   │   ├── categories.js
│   │   ├── cart.js
│   │   ├── orders.js
│   │   ├── ratings.js
│   │   ├── loyalty.js
│   │   ├── notifications.js
│   │   ├── admin.js
│   │   └── delivery.js
│   ├── utils/
│   │   └── generateToken.js
│   ├── .env
│   ├── .env.example
│   ├── package.json
│   └── server.js
│
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── admin/
│   │   │   ├── AdminDashboard.jsx
│   │   │   ├── AdminProducts.jsx
│   │   │   ├── AdminOrders.jsx
│   │   │   ├── AdminInventory.jsx
│   │   │   ├── AdminCategories.jsx
│   │   │   └── AdminSidebar.jsx
│   │   ├── components/
│   │   │   ├── Header.jsx
│   │   │   ├── ProductCard.jsx
│   │   │   └── NotificationPanel.jsx
│   │   ├── context/
│   │   │   ├── AuthContext.jsx
│   │   │   └── CartContext.jsx
│   │   ├── delivery/
│   │   │   └── DeliveryDashboard.jsx
│   │   ├── pages/
│   │   │   ├── HomePage.jsx
│   │   │   ├── LoginPage.jsx
│   │   │   ├── SignupPage.jsx
│   │   │   ├── ProductsPage.jsx
│   │   │   ├── ProductDetailPage.jsx
│   │   │   ├── CartPage.jsx
│   │   │   ├── CheckoutPage.jsx
│   │   │   ├── OrdersPage.jsx
│   │   │   ├── OrderDetailPage.jsx
│   │   │   ├── ProfilePage.jsx
│   │   │   └── LoyaltyPage.jsx
│   │   ├── services/
│   │   │   └── api.js         # Axios API layer
│   │   ├── App.jsx
│   │   ├── index.css          # Design System
│   │   └── main.jsx
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
│
├── docs/
│   └── INTERNSHIP_REPORT.md
├── .gitignore
├── DEPLOYMENT.md
└── README.md
```

---

## 🚀 Quick Start

### Prerequisites
- Node.js v18+
- MongoDB (local or Atlas)
- npm v9+

### 1. Clone & Install

```bash
git clone https://github.com/yourusername/freshcart.git
cd freshcart

# Backend
cd backend
npm install
cp .env.example .env
# Edit .env with your MongoDB URI and JWT secret

# Frontend
cd ../frontend
npm install
```

### 2. Seed Database

```bash
cd backend
npm run seed
```

This creates:
- **12 categories** with icons
- **35+ real products** with images
- **Admin account**: `admin@freshcart.com` / `Admin@123`
- **Delivery agent**: `delivery@freshcart.com` / `Delivery@123`

### 3. Run Development Servers

```bash
# Terminal 1 — Backend
cd backend
npm run dev      # http://localhost:5000

# Terminal 2 — Frontend
cd frontend
npm run dev      # http://localhost:5173
```

---

## 🔑 Demo Accounts

| Role | Email | Password |
|---|---|---|
| **Admin** | admin@freshcart.com | Admin@123 |
| **Delivery Agent** | delivery@freshcart.com | Delivery@123 |
| **Customer** | Register a new account | Any password |

---

## 📡 API Reference

### Base URL: `http://localhost:5000/api`

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/auth/signup` | ❌ | Register customer |
| POST | `/auth/login` | ❌ | Login |
| GET | `/auth/me` | ✅ | Get current user |
| GET | `/products` | ❌ | List products (filters) |
| GET | `/products/:id` | ❌ | Single product |
| GET | `/categories` | ❌ | All categories |
| GET | `/cart` | ✅ | Get cart |
| POST | `/cart/add` | ✅ | Add to cart |
| POST | `/orders` | ✅ Customer | Place order |
| GET | `/orders` | ✅ | My orders |
| GET | `/orders/slots` | ✅ | Delivery slots |
| POST | `/ratings` | ✅ | Submit review |
| GET | `/loyalty/balance` | ✅ | Loyalty balance |
| GET | `/admin/analytics` | ✅ Admin | Analytics data |
| GET | `/admin/orders` | ✅ Admin | All orders |
| PUT | `/admin/orders/:id/status` | ✅ Admin | Update status |
| GET | `/delivery/orders` | ✅ Delivery | Assigned orders |
| PUT | `/delivery/orders/:id/status` | ✅ Delivery | Update delivery |

---

## 🗄️ Database Schema

### Collections: User, Category, Product, Cart, Order, Rating, LoyaltyTransaction, Notification, DeliverySlot

---

## 🎨 Design System

The app uses a custom CSS design system with:
- **Color Palette**: Fresh green (#00a862) primary, warm orange secondary
- **Typography**: Inter + Poppins from Google Fonts
- **Components**: Cards, Badges, Buttons, Forms, Modals, Tables
- **Animations**: Smooth transitions, skeleton loading, page enter effects
- **Responsive**: Mobile-first with breakpoints at 480px, 768px, 1024px

---

## 📄 License

MIT License — Free to use for learning and internship projects.

---

**Made with ❤️ by the FreshCart Team**
