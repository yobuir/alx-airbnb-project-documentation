# Airbnb Clone Backend - Requirements Specification

This document describes the functional and technical requirements for the key backend features of the Airbnb Clone platform. It includes API routes, expected inputs/outputs, validation rules, and performance benchmarks.

---

## 🔐 1. User Authentication

### Functional Requirements
- Users can register, log in, and get a JWT token.
- Passwords are stored securely using hashing.
- JWT tokens are used for route protection.

### API Endpoints
| Method | Endpoint      | Description               |
|--------|---------------|---------------------------|
| POST   | /api/register | Register new user         |
| POST   | /api/login    | Authenticate user         |
| GET    | /api/profile  | Get current user profile  |

### Example Input/Output
```json
POST /api/register

Request:
{
  "first_name": "Alice",
  "last_name": "Doe",
  "email": "alice@example.com",
  "password": "SafePass123"
}

Response:
{
  "user_id": "uuid",
  "token": "jwt-token"
}
```

### Validation Rules

* Email must be valid and unique.
* Password must be at least 8 characters.

### Performance

* Login response time < 400ms
* Token validation < 200ms

---

## 👤 2. User Profile

### Functional Requirements

* Authenticated users can view and update their profile.
* Users can view their bookings or properties (based on role).

### API Endpoints

| Method | Endpoint           | Description             |
| ------ | ------------------ | ----------------------- |
| GET    | /api/profile       | Fetch user info         |
| PUT    | /api/profile       | Update user details     |
| GET    | /api/my-bookings   | Fetch user's bookings   |
| GET    | /api/my-properties | Fetch properties (host) |

### Validation Rules

* Only authenticated users can access their data.

---

## 🏠 3. Property Management

### Functional Requirements

* Hosts can add, update, or delete property listings.
* Listings include name, description, location, price, and images.

### API Endpoints

| Method | Endpoint             | Description                |
| ------ | -------------------- | -------------------------- |
| POST   | /api/properties      | Create a new listing       |
| PUT    | /api/properties/\:id | Update listing             |
| DELETE | /api/properties/\:id | Delete a listing           |
| GET    | /api/properties      | List/search all properties |
| GET    | /api/properties/\:id | Get single property        |

### Validation Rules

* Only users with role = 'host' can manage listings.
* Price must be a decimal greater than zero.

---

## 📅 4. Booking System

### Functional Requirements

* Guests can search for properties and make bookings.
* System must validate date availability and avoid double-bookings.
* Guests can cancel bookings.

### API Endpoints

| Method | Endpoint           | Description          |
| ------ | ------------------ | -------------------- |
| POST   | /api/bookings      | Create a new booking |
| GET    | /api/bookings/\:id | View booking details |
| DELETE | /api/bookings/\:id | Cancel a booking     |

### Example Input

```json
{
  "property_id": "uuid",
  "start_date": "2025-07-01",
  "end_date": "2025-07-05"
}
```

### Validation Rules

* Booking dates must not overlap with existing bookings.
* Dates must be in correct format and chronological.

### Performance

* Availability check < 300ms
* Booking confirmation < 500ms

---

## 💳 5. Payments

### Functional Requirements

* Guests can pay for bookings using supported methods.
* Payments are tied to bookings and stored securely.

### API Endpoints

| Method | Endpoint           | Description               |
| ------ | ------------------ | ------------------------- |
| POST   | /api/payments      | Process a booking payment |
| GET    | /api/payments/\:id | View payment details      |

### Example Input

```json
{
  "booking_id": "uuid",
  "amount": 250.00,
  "payment_method": "credit_card"
}
```

### Validation Rules

* Amount must match booking total.
* Payment method must be one of: credit\_card, paypal, stripe.

---

## ⭐ 6. Reviews and Ratings

### Functional Requirements

* Guests can leave reviews and rate properties after a stay.
* Hosts can view all reviews for their properties.

### API Endpoints

| Method | Endpoint          | Description               |
| ------ | ----------------- | ------------------------- |
| POST   | /api/reviews      | Leave a review            |
| GET    | /api/reviews/\:id | View a property’s reviews |

### Validation Rules

* Rating must be between 1 and 5.
* One review per guest per property.

---

## 💬 7. Messaging System

### Functional Requirements

* Users can send/receive messages.
* Messages are stored and time-stamped.

### API Endpoints

| Method | Endpoint      | Description           |
| ------ | ------------- | --------------------- |
| POST   | /api/messages | Send a message        |
| GET    | /api/messages | Get all conversations |

### Example Input

```json
{
  "recipient_id": "uuid",
  "message_body": "Hi, is your place available?"
}
```

### Validation Rules

* Message body cannot be empty.
* Sender and recipient must be valid users.

---

## 🛠️ 8. Admin Features

### Functional Requirements

* Admins can moderate listings, reviews, and users.
* Admins can view platform-wide metrics and analytics.

### API Endpoints

| Method | Endpoint               | Description             |
| ------ | ---------------------- | ----------------------- |
| GET    | /api/admin/stats       | View usage metrics      |
| DELETE | /api/admin/user/\:id   | Ban or delete a user    |
| DELETE | /api/admin/review/\:id | Remove a flagged review |
