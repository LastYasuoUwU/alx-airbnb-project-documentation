# Airbnb Clone Backend – Use Case Diagram

## 🎯 Objective

This document visualizes the **system interactions** for the Airbnb Clone backend.  
It highlights the main actors (users) and their interactions (use cases) with the system.

---

## 🧑‍🤝‍🧑 Actors

- **Guest**: Regular user booking properties
- **Host**: User listing properties and managing availability
- **Admin**: Manages users, listings, bookings, and payments
- **Payment Gateway**: External service (e.g., Stripe, PayPal) for processing payments

---

## ⚙️ Use Cases

### Guest

- Register / Login
- Search & Filter Properties
- Book Property
- Cancel Booking
- Make Payment
- Leave Review

### Host

- Register / Login
- Create / Edit / Delete Property
- Manage Availability & Pricing
- Respond to Reviews

### Admin

- Manage Users
- Manage Listings
- Manage Bookings
- Manage Payments

### Payment Gateway

- Process Payment
- Confirm Transaction
