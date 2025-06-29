
# 🧾 Backend Requirement Specifications – Airbnb Clone

This document outlines the technical and functional requirements for core backend features in the Airbnb Clone system, including user authentication, property management, and the booking system.

---

## 🔐 1. User Authentication

### 🔧 Functional Requirements:
- Allow users to register as guests or hosts
- Secure login using email and password
- Generate JWT tokens upon successful login
- Role-based access control (Guest, Host, Admin)

### 🔗 API Endpoints:

| Method | Endpoint       | Description          |
|--------|----------------|----------------------|
| POST   | `/api/auth/register` | Register a new user |
| POST   | `/api/auth/login`    | Login and receive JWT |
| GET    | `/api/auth/profile`  | Get current user profile |

### 📥 Input:
```json
POST /api/auth/register
{
  "first_name": "Ian",
  "last_name": "Aloise",
  "email": "ian@example.com",
  "password": "SecurePass123!",
  "role": "guest"
}
```

### 📤 Output (Success):
```json
{
  "token": "jwt.token.string",
  "user": {
    "id": "uuid",
    "first_name": "Ian",
    "role": "guest"
  }
}
```

### ✅ Validation Rules:
- Email must be valid and unique
- Password: Minimum 8 characters, alphanumeric
- Role: One of [guest, host, admin]

### ⚙️ Performance Criteria:
- Login response within 500ms
- Token expiration: 24 hours

---

## 🏠 2. Property Management

### 🔧 Functional Requirements:
- Hosts can create, update, and delete property listings
- Properties include title, description, location, price, availability
- Properties linked to host accounts

### 🔗 API Endpoints:

| Method | Endpoint             | Description            |
|--------|----------------------|------------------------|
| POST   | `/api/properties`    | Create new property    |
| PUT    | `/api/properties/:id`| Update property        |
| DELETE | `/api/properties/:id`| Delete property        |
| GET    | `/api/properties`    | List all properties    |
| GET    | `/api/properties/:id`| View a single property |

### 📥 Input:
```json
POST /api/properties
{
  "name": "Beachfront Villa",
  "description": "Beautiful ocean view",
  "location": "Diani, Kenya",
  "price_per_night": 120.50,
  "amenities": ["WiFi", "Pool"]
}
```

### 📤 Output (Success):
```json
{
  "id": "uuid",
  "host_id": "uuid",
  "name": "Beachfront Villa"
}
```

### ✅ Validation Rules:
- Name, location, and description: Required
- Price must be a positive decimal
- Host must be authenticated

### ⚙️ Performance Criteria:
- Create/update within 700ms
- Fetch all listings paginated (limit: 10–20 per page)

---

## 📅 3. Booking System

### 🔧 Functional Requirements:
- Guests can book properties for specific dates
- System checks for availability and prevents double bookings
- Calculate total price based on dates and price/night

### 🔗 API Endpoints:

| Method | Endpoint            | Description               |
|--------|---------------------|---------------------------|
| POST   | `/api/bookings`     | Create a new booking      |
| GET    | `/api/bookings/:id` | View a specific booking   |
| DELETE | `/api/bookings/:id` | Cancel a booking          |

### 📥 Input:
```json
POST /api/bookings
{
  "property_id": "uuid",
  "start_date": "2025-07-01",
  "end_date": "2025-07-05"
}
```

### 📤 Output (Success):
```json
{
  "booking_id": "uuid",
  "total_price": 482.00,
  "status": "confirmed"
}
```

### ✅ Validation Rules:
- Dates must be in the future
- End date must be after start date
- No overlap with existing confirmed bookings

### ⚙️ Performance Criteria:
- Check and create booking within 800ms
- Ensure no race conditions in simultaneous bookings

---

## 📌 Notes
- All endpoints must return proper HTTP status codes (e.g., 200 OK, 400 Bad Request, 401 Unauthorized)
- Use environment variables for secret keys and database connections
- All date-time values use UTC format
