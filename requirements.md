# Technical and Functional Requirements

## 1. User Authentication

### Description
Allows users to register, log in, and manage sessions securely.

### API Endpoints
- **POST /api/v1/auth/register**
  - **Input:** `{ "name": "string", "email": "string", "password": "string" }`
  - **Validation:**
    - Email must be unique and valid format
    - Password: min 8 characters
  - **Output:** `{ "userId": "uuid", "token": "JWT" }`
- **POST /api/v1/auth/login**
  - **Input:** `{ "email": "string", "password": "string" }`
  - **Output:** `{ "token": "JWT" }`
- **GET /api/v1/auth/logout**
  - Invalidate the token/session

### Performance Criteria
- Login response time < 500ms
- Hash passwords using bcrypt (min 10 rounds)

---

## 2. Property Management

### Description
Allows hosts to create, update, and manage property listings.

### API Endpoints
- **POST /api/v1/properties/**
  - **Input:** `{ "title": "string", "description": "string", "price": "float", "location": "string", "availability": "boolean" }`
  - **Validation:**
    - Title is required
    - Price must be positive number
  - **Output:** `{ "propertyId": "uuid", "status": "created" }`
- **PUT /api/v1/properties/:id**
  - **Input:** JSON body with updatable fields
  - **Output:** `{ "status": "updated" }`
- **DELETE /api/v1/properties/:id**
  - **Output:** `{ "status": "deleted" }`
- **GET /api/v1/properties/:id**
  - **Output:** Full property details

### Performance Criteria
- Listing update within 1 second
- Cache frequently accessed listings using Redis

---

## 3. Booking System

### Description
Enables guests to book available properties and process payments.

### API Endpoints
- **POST /api/v1/bookings/**
  - **Input:** `{ "userId": "uuid", "propertyId": "uuid", "checkIn": "date", "checkOut": "date" }`
  - **Validation:**
    - Dates must be valid and not overlap with existing bookings
  - **Output:** `{ "bookingId": "uuid", "status": "pending/payment" }`
- **GET /api/v1/bookings/:id**
  - **Output:** Booking details
- **POST /api/v1/bookings/:id/confirm**
  - **Process:** Verifies payment, then updates booking status to "confirmed"

### Performance Criteria
- Payment integration with Stripe API
- Double-booking prevention with atomic DB transaction

---

## General Notes
- All endpoints return JSON and use RESTful standards
- Use JWT for authentication and role-based access control
- Input validation via middleware (e.g., Joi or Yup)

