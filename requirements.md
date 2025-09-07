# Backend Requirement Specifications for Airbnb Clone

## 1. User Authentication

**Description:** Handles user registration, login, and role-based access.

**API Endpoints:**
- `POST /api/auth/register` – Register a new user
- `POST /api/auth/login` – Login for registered users
- `POST /api/auth/logout` – Logout user
- `GET /api/auth/users` – Retrieve list of users (admin only)

**Input/Output:**

| Endpoint | Input | Output |
|----------|-------|--------|
| /register | username, email, password, role | success message, user ID |
| /login | username/email, password | JWT token, user details |
| /logout | JWT token | success message |
| /users | none | list of users (JSON) |

**Validation Rules:**
- Username: required, 3-30 chars
- Email: required, valid format
- Password: required, min 8 chars, must include letters and numbers
- Role: allowed values: `user`, `host`, `admin`

**Performance Criteria:**
- JWT token expiry: 1 hour
- Response time: < 200ms for login/register under normal load

---

## 2. Property Management

**Description:** CRUD operations for properties listed by hosts.

**API Endpoints:**
- `POST /api/properties` – Add a property
- `GET /api/properties` – List all properties
- `GET /api/properties/:id` – View property details
- `PUT /api/properties/:id` – Update property
- `DELETE /api/properties/:id` – Delete property

**Input/Output:**

| Endpoint | Input | Output |
|----------|-------|--------|
| /properties POST | title, description, price, location, host_id | property object with ID |
| /properties GET | none | list of properties |
| /properties/:id GET | property ID | property details |
| /properties/:id PUT | property fields | updated property object |
| /properties/:id DELETE | property ID | success message |

**Validation Rules:**
- Title: required
- Price: required, numeric
- Location: required
- host_id: must exist in users table

**Performance Criteria:**
- CRUD operations response time: < 250ms
- Maximum concurrent property retrieval: 1000 requests/sec

---

## 3. Booking System

**Description:** Allows users to book properties and track reservations.

**API Endpoints:**
- `POST /api/bookings` – Create a booking
- `GET /api/bookings` – List all bookings (user-specific)
- `GET /api/bookings/:id` – View booking details
- `PUT /api/bookings/:id/cancel` – Cancel booking
- `GET /api/bookings/user/:userId` – List bookings by user

**Input/Output:**

| Endpoint | Input | Output |
|----------|-------|--------|
| /bookings POST | user_id, property_id, check_in, check_out | booking object with ID |
| /bookings GET | none | list of bookings |
| /bookings/:id GET | booking ID | booking details |
| /bookings/:id/cancel PUT | booking ID | success message |
| /bookings/user/:userId GET | user ID | list of bookings for user |

**Validation Rules:**
- Dates must be valid and in the future
- Property must be available for selected dates
- user_id and property_id must exist in their respective tables

**Performance Criteria:**
- Booking creation response: < 300ms
- Data consistency must prevent double bookings

---

**Notes:**
- All endpoints require JWT authentication except registration and login.
- Errors should be returned in JSON format with proper HTTP status codes.
