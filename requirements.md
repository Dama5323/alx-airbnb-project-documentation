# 📋 Backend Feature Requirements – Airbnb Clone

This document outlines the technical and functional specifications for three core backend features of the Airbnb Clone project: User Authentication, Property Management, and Booking System.

---

## 🔐 1. User Authentication

### Functional Requirements:
- Users must be able to register, log in, log out, and update their profile.
- Passwords must be securely hashed before storage.
- Only authenticated users should access protected endpoints.

### API Endpoints:

| Method | Endpoint            | Description             | Authentication |
|--------|---------------------|-------------------------|----------------|
| POST   | `/api/register`     | Register a new user     | No             |
| POST   | `/api/login`        | Authenticate user       | No             |
| GET    | `/api/logout`       | Log out the user        | Yes            |
| PUT    | `/api/profile`      | Update user profile     | Yes            |

### Input/Output Specifications:
- **POST /api/register**
  - Input: `{ "email": "user@example.com", "password": "Pass123!", "name": "User Name" }`
  - Output: `{ "message": "Registration successful", "user_id": "123abc" }`

- **POST /api/login**
  - Input: `{ "email": "user@example.com", "password": "Pass123!" }`
  - Output: `{ "token": "jwt_token", "user_id": "123abc" }`

### Validation Rules:
- Email must be in valid format.
- Password must contain at least 8 characters, one number, and one special character.
- Name must not be empty.

### Performance Criteria:
- Registration/login must complete within 500ms under normal load.
- Tokens should expire after 24 hours.

---

## 🏠 2. Property Management

### Functional Requirements:
- Hosts can create, update, and delete property listings.
- Listings must include a title, description, price, availability, and images.
- Users can browse available listings.

### API Endpoints:

| Method | Endpoint              | Description                   | Authentication |
|--------|-----------------------|-------------------------------|----------------|
| POST   | `/api/properties`     | Create new property listing   | Yes (Host)     |
| GET    | `/api/properties`     | View all listings             | No             |
| GET    | `/api/properties/:id` | View specific property        | No             |
| PUT    | `/api/properties/:id` | Update a property listing     | Yes (Owner)    |
| DELETE | `/api/properties/:id` | Delete a property listing     | Yes (Owner)    |

### Input/Output Specifications:
- **POST /api/properties**
  - Input: `{ "title": "Cozy Loft", "price": 80, "location": "Nairobi", "images": ["img1.jpg"], "description": "A nice place" }`
  - Output: `{ "message": "Property created", "property_id": "prop123" }`

### Validation Rules:
- Title, price, and location must be provided.
- Price must be a positive number.
- Maximum 10 images allowed per property.

### Performance Criteria:
- Listing retrieval should not exceed 300ms with caching enabled.
- CRUD operations must complete in under 1s.

---

## 📅 3. Booking System

### Functional Requirements:
- Guests can book available properties.
- Booking must validate availability and avoid double-booking.
- Bookings should trigger confirmation emails.

### API Endpoints:

| Method | Endpoint              | Description                | Authentication |
|--------|-----------------------|----------------------------|----------------|
| POST   | `/api/bookings`       | Create a new booking       | Yes            |
| GET    | `/api/bookings`       | List user’s bookings       | Yes            |
| GET    | `/api/bookings/:id`   | View specific booking      | Yes            |
| DELETE | `/api/bookings/:id`   | Cancel booking             | Yes (Guest)    |

### Input/Output Specifications:
- **POST /api/bookings**
  - Input: `{ "property_id": "prop123", "check_in": "2025-06-01", "check_out": "2025-06-05" }`
  - Output: `{ "message": "Booking confirmed", "booking_id": "book456" }`

### Validation Rules:
- Property must be available for the selected dates.
- Dates must be in the future and check-out > check-in.
- A user cannot book their own property.

### Performance Criteria:
- Booking creation must complete within 800ms.
- Availability check should support concurrent access without conflicts (use row-level DB locking or transactions).

---

## ✅ Additional Notes

- All API responses should follow a consistent structure with `message`, `data`, and `error` fields.
- Use RESTful principles for endpoint naming and HTTP methods.
- Use JWT for stateless authentication.
- Apply input sanitization and rate-limiting to mitigate security risks.

