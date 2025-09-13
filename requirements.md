# Airbnb Clone Backend – Requirements Specification

## 🎯 Objective

This document outlines the **technical and functional requirements** for key backend features of the Airbnb Clone.  
It covers **API endpoints**, **input/output specifications**, **validation rules**, and **performance criteria**.

---

## 1. User Authentication

### Functional Requirements

- Users can register as **Guest** or **Host**.
- Users can log in with **email + password** or via **OAuth (Google, Facebook)**.
- Sessions are managed securely with **JWT tokens**.
- Passwords must be securely hashed (e.g., bcrypt/argon2).

### API Endpoints

- `POST /api/auth/register`
  - **Input**: `{ firstName, lastName, email, password, role }`
  - **Output**: `{ userId, token }`
- `POST /api/auth/login`
  - **Input**: `{ email, password }`
  - **Output**: `{ userId, token }`
- `GET /api/auth/profile`
  - **Auth required**
  - **Output**: `{ userId, name, email, role, createdAt }`

### Validation Rules

- Email must be unique and valid format.
- Password length ≥ 8, include letters & numbers.
- Role must be one of `["guest", "host", "admin"]`.

### Performance Criteria

- Authentication requests must complete < 300ms under normal load.
- JWT token verification < 50ms.

---

## 2. Property Management

### Functional Requirements

- Hosts can **create, edit, and delete** property listings.
- Properties store details: title, description, location, price, amenities, photos, availability.
- Guests can search and view property details.

### API Endpoints

- `POST /api/properties`
  - **Input**: `{ title, description, location, pricePerNight, amenities[], photos[], availability[] }`
  - **Output**: `{ propertyId, hostId, createdAt }`
- `PUT /api/properties/:id`
  - **Input**: Updated property fields
  - **Output**: `{ success: true, updatedProperty }`
- `DELETE /api/properties/:id`
  - **Auth: Host or Admin**
  - **Output**: `{ success: true }`
- `GET /api/properties`
  - **Query params**: `location`, `priceMin`, `priceMax`, `guests`, `amenities[]`, `page`, `limit`
  - **Output**: `[ { propertyId, title, location, pricePerNight, rating, availability } ]`
- `GET /api/properties/:id`
  - **Output**: `{ propertyId, title, description, location, pricePerNight, amenities[], photos[], host, reviews[] }`

### Validation Rules

- Price must be > 0.
- Location string length ≤ 100 chars.
- Availability dates must be future dates.
- Only **Hosts** or **Admins** may create/update/delete.

### Performance Criteria

- Property search should return results < 500ms for queries with up to 10,000 records.
- Pagination required for lists > 50 results.

---

## 3. Booking System

### Functional Requirements

- Guests can book properties for specified dates.
- Prevent overlapping/double bookings.
- Guests and Hosts can cancel bookings (based on policy).
- Booking statuses: `pending`, `confirmed`, `canceled`, `completed`.

### API Endpoints

- `POST /api/bookings`
  - **Input**: `{ userId, propertyId, startDate, endDate }`
  - **Output**: `{ bookingId, status, totalPrice }`
- `GET /api/bookings/:id`
  - **Auth: Guest (own booking), Host (their property), or Admin**
  - **Output**: `{ bookingId, property, user, startDate, endDate, status, totalPrice }`
- `PUT /api/bookings/:id/cancel`
  - **Output**: `{ bookingId, status: "canceled" }`
- `GET /api/bookings?userId=...`
  - **Output**: List of bookings for a user.

### Validation Rules

- Dates must not overlap existing confirmed bookings.
- End date must be after start date.
- Booking duration max 90 days.
- Only Guests can create bookings; only Guests/Hosts/Admins can cancel.

### Performance Criteria

- Double-booking validation must complete in < 200ms.
- Booking creation must respond < 400ms under standard load.

---

## ✅ Notes

- All endpoints should follow **RESTful conventions** with proper HTTP status codes:
  - `200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `404 Not Found`, `500 Internal Server Error`.
- Input validation errors must return clear error messages.
- Use consistent JSON response structure across APIs.
