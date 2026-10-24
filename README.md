<div align="center">

<img src="https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=white" />
<img src="https://img.shields.io/badge/Spring_Boot-3.2-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white" />
<img src="https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white" />
<img src="https://img.shields.io/badge/Razorpay-Payment-02042B?style=for-the-badge&logo=razorpay&logoColor=white" />
<img src="https://img.shields.io/badge/JWT-Auth-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white" />
<img src="https://img.shields.io/badge/Tailwind_CSS-3.0-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white" />

# 🏠 ComfortPG — PG Booking & Management Platform

### A full-stack paying guest accommodation platform with dedicated portals for Students, Owners, and Admins

**[🌐 Live Demo](https://comfortpg-frontend.onrender.com)** &nbsp;|&nbsp;
</div>

---

## 📌 The Problem

Students and working professionals relocating to new cities face a major trust problem when searching for PG accommodations:

- **Fake listings** — anyone can list a PG on most platforms with no verification
- **No transparency** — hidden charges, misleading photos, unverified owners
- **Poor booking experience** — no real-time confirmation, no payment security
- **No accountability** — once money is paid, there is no tracking or dispute system

**ComfortPG solves this** by introducing a mandatory admin verification layer, secure Razorpay payments, and a real-time booking lifecycle that keeps both students and owners informed at every step.

---

## ✨ Key Features

### 👨‍🎓 For Students
- Browse verified PGs with search and filter by city, category, price, amenities
- View detailed PG pages with image gallery, amenities list, and ratings
- Book PGs with a 3-step checkout — fill details → pay via Razorpay → instant confirmation
- Track all bookings with real-time status updates (Pending / Confirmed / Rejected)
- Receive in-app notifications for every booking update
- Write reviews and rate PGs after staying
- Student dashboard with booking stats and quick actions

### 🏠 For Owners
- Register as an owner and list multiple PG properties
- Each listing goes through admin verification before going live — builds student trust
- View all bookings for your properties with student details
- Confirm or reject booking requests
- Track total earnings with automatic 90/10 revenue split (owner gets 90%)
- Receive notifications when students book your PGs
- Owner dashboard with listing stats, booking counts, and earnings overview

### 🛡️ For Admins
- Review all pending PG listings before they go live
- Approve or reject listings with one click
- Full booking audit trail — see which student booked which PG owned by which owner
- Revenue overview — total platform income, owner payouts, platform fee (10%)
- User management — view all students, owners, and admins
- Owner earnings breakdown per owner

---

## 🎯 Real World Problem Solved

| Problem | ComfortPG Solution |
|---|---|
| Fake PG listings | Admin verifies every listing before it goes live |
| Payment fraud | Razorpay + HMAC-SHA256 signature verification |
| No booking tracking | Full lifecycle — Pending → Confirmed → Notified |
| Opaque money flow | 90/10 split tracked per booking in database |
| No accountability | Every booking links student + PG + owner permanently |
| Unsafe accommodations | Verified badge only after admin approval |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    React Frontend                        │
│         (Student Portal / Owner Portal / Admin Panel)    │
└──────────────────────┬──────────────────────────────────┘
                       │ REST API (JWT Bearer Token)
┌──────────────────────▼──────────────────────────────────┐
│                  Spring Boot Backend                     │
│   ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │
│   │   Auth   │ │    PG    │ │ Booking  │ │ Payment  │  │
│   │Controller│ │Controller│ │Controller│ │Controller│  │
│   └──────────┘ └──────────┘ └──────────┘ └──────────┘  │
│   ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │
│   │  Review  │ │  Notif.  │ │  Owner   │ │  Admin   │  │
│   │Controller│ │Controller│ │Controller│ │Controller│  │
│   └──────────┘ └──────────┘ └──────────┘ └──────────┘  │
│                  Spring Security + JWT                   │
└──────────────────────┬──────────────────────────────────┘
                       │ JPA / Hibernate
┌──────────────────────▼──────────────────────────────────┐
│                    MySQL Database                        │
│   users · pgs · bookings · reviews · notifications      │
│                     · bills                             │
└─────────────────────────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────┐
│                  Razorpay Gateway                        │
│         Order Creation + HMAC-SHA256 Verification        │
└─────────────────────────────────────────────────────────┘
```

---

## 🔄 Complete User Flows

### Student Booking Flow
```
Sign Up (Student) → Login → Browse Verified PGs
→ Select PG → Choose Dates → Fill Details
→ Pay via Razorpay → Signature Verified on Backend
→ Booking CONFIRMED → Student Notified → Owner Notified
→ Booking visible in My Bookings with full breakdown
```

### Owner Listing Flow
```
Sign Up (Owner) → Login → Add PG Form
→ PG saved with verified=false, status=PENDING
→ Admin Reviews → Approves or Rejects
→ If Approved → PG visible to all students
→ Student Books → Owner Dashboard shows request
→ Owner Confirms → Student gets notification
→ Earnings tracked (90% of rent)
```

### Admin Verification Flow
```
Login (Admin) → Admin Dashboard
→ PG Verification Tab → Review listing details
→ Click Approve → verified=true → PG goes live
→ Bookings Tab → See all bookings platform-wide
→ Owners Tab → See earnings breakdown per owner
→ Users Tab → Manage all platform users
```

### Payment Flow
```
Student clicks Pay
→ POST /api/payments/create-order
→ Backend creates Booking (PENDING) + Razorpay order
→ Razorpay checkout popup opens
→ Student pays with card/UPI
→ Razorpay returns orderId + paymentId + signature
→ POST /api/payments/verify
→ Backend generates HMAC-SHA256 hash
→ Signature matches → Booking = CONFIRMED
→ Both student and owner notified
→ 90/10 earnings split recorded
```

---

## 🗄️ Database Schema

```
users
├── id, name, email, password (BCrypt), role (USER/OWNER/ADMIN)

pgs
├── id, name, address, city, description, rent
├── available_rooms, type, category, size
├── primary_image (TEXT), rating, review_count
├── verified (boolean), verification_status
└── owner_id → users.id

pg_amenities
└── pg_id, amenity

pg_images  
└── pg_id, image_url

bookings
├── id, user_id → users.id, pg_id → pgs.id
├── check_in_date, check_out_date, status
├── total_amount, platform_fee (10%), owner_earnings (90%)
└── booking_date

reviews
├── id, rating, comment, created_at
├── pg_id → pgs.id, user_id → users.id

notifications
├── id, title, message, read_flag, created_at
└── user_id → users.id

bills
├── id, amount, description, created_at
└── user_id → users.id
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, React Router v6, Tailwind CSS, Context API |
| Backend | Spring Boot 3.2, Spring Security, Spring Data JPA |
| Database | MySQL 8.0 with Hibernate ORM |
| Authentication | JWT (JJWT 0.11.5) + BCrypt password hashing |
| Payment | Razorpay with HMAC-SHA256 signature verification |
| Deployment | Render (Frontend + Backend), Railway (MySQL), Docker |

---

## 📁 Project Structure

```
pgbookingsystem/
├── pg-frontend/                        # React application
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Home.jsx                # Landing page
│   │   │   ├── Rooms.jsx               # Browse all verified PGs
│   │   │   ├── RoomDetails.jsx         # PG detail + booking + reviews
│   │   │   ├── Login.jsx               # Role-based login + redirect
│   │   │   ├── Signup.jsx              # 2-step role selection signup
│   │   │   ├── UserDashboard.jsx       # Student dashboard
│   │   │   ├── MyBookings.jsx          # Student booking tracker
│   │   │   ├── Payment.jsx             # 3-step Razorpay checkout
│   │   │   ├── Notifications.jsx       # In-app notifications
│   │   │   ├── FindBroker.jsx          # Broker directory + chat
│   │   │   ├── admin/
│   │   │   │   └── AdminDashboard.jsx  # 5-tab admin panel
│   │   │   └── owner/
│   │   │       ├── OwnerDashboard.jsx  # Owner stats + bookings
│   │   │       ├── AddPG.jsx           # PG listing form
│   │   │       └── MyPGs.jsx           # Owner's listings
│   │   ├── components/
│   │   │   ├── Layout/
│   │   │   │   ├── Navbar.jsx          # Role-specific navigation
│   │   │   │   ├── ProtectedRoute.jsx  # Auth + role guard
│   │   │   │   └── Footer.jsx
│   │   │   ├── Home/                   # Hero, Features, Stats, Testimonials
│   │   │   ├── Rooms/                  # RoomCard, SearchBar, CategoryFilter
│   │   │   └── Brokers/               # BrokerCard, ChatModal
│   │   ├── context/
│   │   │   └── AppContext.jsx          # Global state management
│   │   ├── services/
│   │   │   └── api.js                  # All API calls (15+ endpoints)
│   │   └── App.jsx                     # Routes with role-based guards
│   └── public/
│       └── _redirects                  # React Router SPA fix
│
└── pg-backend/                         # Spring Boot application
    ├── Dockerfile                       # Docker config for Render
    └── src/main/java/com/pgbooking/
        ├── model/                       # JPA entities
        │   ├── User.java, Role.java
        │   ├── PG.java
        │   ├── Booking.java, BookingStatus.java
        │   ├── Review.java
        │   ├── Notification.java
        │   └── Bill.java
        ├── repository/                  # Spring Data JPA repos
        ├── service/                     # Business logic layer
        │   ├── AuthService.java
        │   ├── PGService.java
        │   ├── BookingService.java
        │   ├── PaymentService.java
        │   ├── ReviewService.java
        │   └── NotificationService.java
        ├── web/                         # REST Controllers
        │   ├── AuthController.java      # /api/auth/**
        │   ├── PGController.java        # /api/pgs/**
        │   ├── OwnerController.java     # /api/owner/**
        │   ├── BookingController.java   # /api/bookings/**
        │   ├── PaymentController.java   # /api/payments/**
        │   ├── ReviewController.java    # /api/reviews/**
        │   ├── NotificationController   # /api/notifications/**
        │   └── AdminController.java     # /api/admin/**
        ├── security/
        │   ├── TokenService.java        # JWT create + verify
        │   └── TokenAuthenticationFilter.java
        └── config/
            └── SecurityConfig.java      # CORS + route security
```

---

## 🚀 Getting Started

### Prerequisites
- Java 17
- Node.js 18+
- MySQL 8.0
- Maven

### Backend Setup

```bash
# Clone the repo
git clone https://github.com/yourusername/pgbookingsystem.git
cd pgbookingsystem/pg-backend

# Configure application.properties
# src/main/resources/application.properties
spring.datasource.url=jdbc:mysql://localhost:3306/pgbooking
spring.datasource.username=root
spring.datasource.password=yourpassword
jwt.secret=YourSecretKey
razorpay.key.id=rzp_test_xxx
razorpay.key.secret=xxx

# Create database
mysql -u root -p
CREATE DATABASE pgbooking;

# Run the backend
mvn spring-boot:run
```

Backend starts at `http://localhost:8082`

Seed data is loaded automatically on first run:
```
Admin:   admin@comfortpg.com   / admin123
Owner:   owner@example.com     / owner123
Student: student@example.com   / student123
```

### Frontend Setup

```bash
cd pgbookingsystem/pg-frontend

# Create .env file
echo "VITE_API_BASE_URL=http://localhost:8082" > .env
echo "VITE_RAZORPAY_KEY_ID=rzp_test_xxx" >> .env

# Install and run
npm install
npm run dev
```

Frontend starts at `http://localhost:5173`

---

## 🌐 API Endpoints

| Method | Endpoint | Access | Description |
|---|---|---|---|
| POST | `/api/auth/signup` | Public | Register new user |
| POST | `/api/auth/login` | Public | Login and get JWT |
| GET | `/api/pgs/all` | Public | Get all verified PGs |
| GET | `/api/pgs/{id}` | Public | Get single PG |
| POST | `/api/owner/pg/add` | Owner | Add new PG listing |
| GET | `/api/owner/pg/my` | Owner | Get owner's listings |
| GET | `/api/owner/dashboard` | Owner | Dashboard stats |
| PUT | `/api/owner/booking/{id}/status` | Owner | Confirm/reject booking |
| POST | `/api/payments/create-order` | Student | Create Razorpay order |
| POST | `/api/payments/verify` | Student | Verify payment signature |
| GET | `/api/bookings/my` | Student | Get student's bookings |
| GET | `/api/reviews/pg/{pgId}` | Public | Get PG reviews |
| POST | `/api/reviews/add` | Student | Add review |
| GET | `/api/notifications/me` | Auth | Get notifications |
| GET | `/api/admin/dashboard` | Admin | Platform stats |
| GET | `/api/admin/bookings` | Admin | All bookings |
| GET | `/api/admin/pgs/pending` | Admin | Pending PGs |
| PUT | `/api/admin/pgs/{id}/verify` | Admin | Approve/reject PG |
| GET | `/api/admin/owner-earnings` | Admin | Owner earnings |
| GET | `/api/admin/users` | Admin | All users |

---

## 🔐 Security Implementation

- **JWT Authentication** — Stateless tokens signed with HMAC-SHA256, stored in localStorage
- **BCrypt Password Hashing** — Passwords never stored in plain text
- **Role-Based Access Control** — `@PreAuthorize` on all protected endpoints
- **Payment Verification** — Server-side HMAC-SHA256 signature check prevents fake payment confirmations
- **CORS Protection** — Whitelist of allowed frontend origins
- **SQL Injection Prevention** — JPA parameterized queries, no raw SQL concatenation

---

## 💰 Revenue Model

Every confirmed booking automatically calculates:

```
Total Rent Paid by Student  =  ₹10,000
Platform Fee (10%)          =  ₹1,000   → ComfortPG earns this
Owner Earnings (90%)        =  ₹9,000   → Owner receives this
```

All three values are stored per booking in the database and visible in both the Owner Dashboard and Admin Dashboard.

---

## 🖥️ Screenshots

| Page | Description |
|---|---|
| Home | Landing page with features, stats, and testimonials |
| Rooms | Browse verified PGs with search and category filter |
| Room Details | Full PG info with booking form and Razorpay integration |
| Student Dashboard | Booking stats, recent bookings, notifications |
| Owner Dashboard | Listings, booking requests, earnings tracker |
| Admin Dashboard | 5-tab panel — overview, bookings, verification, owners, users |
| Signup | 2-step role selection — Student or Owner |

---

## 🚢 Deployment

| Service | Platform | URL |
|---|---|---|
| Frontend | Render Static Site | https://comfortpg-frontend.onrender.com |
| Backend | Render Web Service (Docker) | https://comfortpg-backend.onrender.com |
| Database | Railway MySQL | Private connection |

---

## 🧪 Test Accounts

| Role | Email | Password |
|---|---|---|
| Admin | admin@comfortpg.com | admin123 |
| Owner | owner@example.com | owner123 |
| Student | student@example.com | student123 |

> **Test Payment Card:** 4111 1111 1111 1111 · Expiry: 12/26 · CVV: 123 · OTP: 1234

---

## 👨‍💻 Author

**Jignesh** — Full Stack Developer  
CDAC Project · 2026

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

<div align="center">

⭐ If you found this project helpful, please give it a star!

Made with ❤️ to solve a real problem for students across India

</div>