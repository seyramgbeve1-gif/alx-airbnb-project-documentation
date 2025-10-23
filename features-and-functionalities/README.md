# Features & Functionalities — Airbnb Clone (Backend)

This folder documents the core backend features and functionalities required for the Airbnb Clone project and includes a Draw.io diagram visualizing them.

## Overview
The backend should support user management, property management, booking and availability checks, payments, reviews, messaging, and administrative features. Each feature below lists responsibilities, important API endpoints and brief non-functional requirements.

---

## 1. User Authentication & Management
**Responsibilities**
- User registration, login, logout
- Email verification and password reset
- Role management (guest, host, admin)
- Secure password hashing and session/JWT handling

**Key endpoints**
- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/logout`
- `POST /api/auth/forgot-password`
- `POST /api/auth/reset-password`
- `GET /api/users/:id`

**Non-functional**
- Passwords hashed with bcrypt/argon2
- Email verification link expiry (e.g., 24 hrs)
- Account lockout after repeated failed logins

---

## 2. Property Management
**Responsibilities**
- Host creates/edits/deletes property listings
- Upload and manage property photos
- Property metadata: title, description, location, amenities, pricing, availability rules

**Key endpoints**
- `POST /api/properties`
- `PUT /api/properties/:id`
- `GET /api/properties` (filter by location, price, dates)
- `GET /api/properties/:id`
- `DELETE /api/properties/:id`

**Non-functional**
- Image upload handling (e.g., S3) with size & format validation
- Pagination, search indexing for listings
- Price and availability validation

---

## 3. Booking System & Availability
**Responsibilities**
- Search availability, create bookings, confirm/cancel bookings
- Prevent double bookings (date overlap checks)
- Booking lifecycle: pending → confirmed → canceled → completed

**Key endpoints**
- `POST /api/bookings`
- `GET /api/bookings/:userId`
- `PUT /api/bookings/:id` (host confirm/cancel)
- `GET /api/properties/:id/availability`

**Non-functional**
- Transactional operations around booking+payment (ACID or compensating transaction)
- Notifications to hosts/guests on status changes

---

## 4. Payments & Transactions
**Responsibilities**
- Integrate payment gateway (Stripe/PayPal)
- Create and record payments, refunds, receipts
- Link payments to bookings for confirmation

**Key endpoints**
- `POST /api/payments/checkout`
- `POST /api/payments/webhook` (gateway webhook)
- `GET /api/payments/:bookingId`

**Non-functional**
- PCI-compliant handling (use gateway hosted pages/tokens)
- Idempotency for webhook processing

---

## 5. Reviews & Ratings
**Responsibilities**
- Guests leave reviews for properties after stay
- Hosts may reply to reviews
- Aggregate rating per property

**Key endpoints**
- `POST /api/properties/:id/reviews`
- `GET /api/properties/:id/reviews`

**Non-functional**
- Enforce review timing (post-stay only)
- Rating validation (1–5)

---

## 6. Messaging & Notifications
**Responsibilities**
- In-app messaging between guest and host
- System notifications: booking status, payment, reminders

**Key endpoints**
- `POST /api/messages`
- `GET /api/messages/:conversationId`
- Push/Email notifications via background workers

**Non-functional**
- Message retention policy, rate limiting

---

## 7. Admin & Monitoring
**Responsibilities**
- Admin dashboard for user/property moderation and reports
- Audit logs and monitoring (uptime, error tracking)

**Key endpoints**
- `GET /api/admin/users`
- `GET /api/admin/reports`

**Non-functional**
- Role-based access control
- Logging and metrics (e.g., Prometheus, Sentry)

---

## Diagram
The Draw.io diagram `features-and-functionalities.png` in this folder visualizes actors (Guest, Host, Admin), the backend modules listed above, and primary interactions between them (e.g., Register → Property Listing → Booking → Payment → Review).  

---

## How to use
1. Use this doc as the canonical reference for backend endpoints and responsibilities.  
2. Implement features incrementally: start with Authentication → Property CRUD → Booking → Payment → Messaging → Reviews.  
3. Keep the diagram updated as endpoints and flows change.

