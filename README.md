# airbnb-clone-project

## Overview: 
This project is a backend system designed to mimic the core functionality of Airbnb, enabling property rentals, user bookings, and review systems. It focuses on building scalable, secure and feature-rich system with Django and Django REST Framework.

## Project Goals:
- ***User Management***: Implement secure user registration and login
- ***Property Management***: Allow hosts to list, update, and delete properties
- ***Booking and Review System***: Enable users to book properties and leave reviews
- ***Payment Processing***: Process payments securely
- ***Data Optimization***: Optimize database for performance and scalability

## Tech Stack
**Django**, **Django REST Framework**, **PostgreSQL**, **GraphQL**, **Celery**, **Redis**, **Docker**, **CI/CD Pipelines**

## Team Roles

| Role | Responsibility |
|------|----------------|
|Backend Developer | Responsible for developing the server-side logic, implementing REST and GraphQL APIs, managing integrations, and ensuring the backend meets functional requirements. Works with Django and DRF to handle user authentication, property management, bookings, and reviews.|
|Database Administrator| Designs, implements, and maintains the PostgreSQL database structure. Ensures data integrity, creates indexes for query performance, manages backups, and oversees optimization strategies such as caching and normalization.|
|DevOps Engineer| Builds and maintains CI/CD pipelines, manages Docker containers, and oversees deployment, scaling, and monitoring of the backend services. Ensures high availability and reliability of the infrastructure.|
|QA Engineer | Tests all backend functionalities through manual and automated testing to ensure quality, functionality, and security. Writes unit tests and runs integration and regression testing to catch bugs early and ensure stable releases.|
|Test automation Engineer | Designs a test automation ecosystem. Writes and maintains test scripts for automated testing.|
|Project Manager | Makes sure a product or its part is delivered on time and within budget. Manages and motivates the software development team.|

## Technology Stack Overview
+ **Django** - A Python web framework for building RESTful API
+ **Django REST Framework** - Provides tools for API Development
+ **PostgreSQL** - A powerful Relational Database for data storage
+ **GraphQL** - Allows for flexible and efficient API querying
+ **Celery** - For handling asynchronous tasks such as sending notifications or processing paymnents
+ **Redis** - Used for caching and session management
+ **Docker** - Containerization tool for consistent development and deployment environments
+ **CI/CD Pipelines** - Automated pipelines for testing and deployment

---

## Database Design

### User
Represents all users of the platform whether hosts or guests.

**Key Fields:**
- `id`: Primary key 
- `name`: Users full name
- `email`: Unique email used for authentication
- `password_hash`: Securely stored hashed password
- `is_host`: Boolean variable indicatinf if the user can list properties

**Relationships:**
- A user can own multiple properties if they are a host.
- A user can make multiple bookings as a guest.
- A user can leave multiple reviews.
- A user is referenced by the host_id in the Properties table and guest_id in the Bookings table.

### Properties
Represents properties listed on the platform by hosts.

**Key Fields:**
- `id`: Primary Key
- `host_id`: Foreign Key referencing Users(id)
- `title`: Title of the property.
- `description`: Detailed Info about the property
- `price_per_night`: Cost per night (in chosen currency)

**Relationships:**
- A property belongs to one user (host).
- A property can have many bookings.
- A property can be reviewed through associated bookings.

### Bookings
Reservations made by guests for properties

**Key Fields**
- `id`: Primary Key
- `property_id`: Foreign Key referencing properties(id)
- `guest_id`: Foriegn Key referencing Users(id)
- `check_in_date`: Start date of user stay
- `check_out_date`: End date of user stay

**Relationships:**
- A booking is made by one user (guest).
- A booking is for one specific property.
- A booking can have one payment.
- A booking can receive one review.

### Payments
Represents Payment records for bookings

**Key Fields:**
- `id`: Primary key
- `booking_id`: Foreign Key referencing Bookings(id)
- `amount`: Total amount paid for the booking
- `payment_status`: Status of payment(pending, completed, failed)
- `timestamp`: Time of transaction

**Relationships:**
- A payment is linked to one booking.
- Each booking has at most one payment record.

### Reviews
Ratings and feedback submitted by guests after their stay

**Key Fields:** 
- `id`: Primary Key 
- `booking_id`: Foreign key referencing Bookings(id)
- `user_id`: Foreign key referencing Users(id)
- `rating`: Star rating (1 to 5)
- `comment`: Guest's written feedback

**Relationships:**
- A review is written by one user.
- A review is linked to one booking.
- Indirectly, a review relates to a property through the booking.

---

## Feature Breakdown

The Airbnb Clone project is structured around core features that replicate the essential functionalities of Airbnb. These features ensure users can interact with the platform as guests or hosts, manage properties, make bookings, process payments, and leave reviews.

---

### User Management
This feature handles user registration, login, profile management, and authentication. It supports role differentiation between hosts and guests and ensures secure access through password hashing and token-based authentication.

---

### Property Management
Hosts can create, update, retrieve, and delete property listings. This feature allows users to showcase accommodations with details like descriptions, prices, and images, making properties discoverable to potential guests.

---

### Booking System
Guests can book properties by selecting check-in and check-out dates. The system validates availability, calculates the total cost, and records the booking details, forming the core of the rental process.

---

### Payment Processing
Handles financial transactions securely when a booking is confirmed. This feature ensures that payment data is captured, stored, and processed correctly, enabling transaction tracking and status monitoring.

---

### Review System
Guests can leave ratings and comments about their stay after completing a booking. This feature builds trust among users by providing social proof and feedback for future guests and property owners.

---

### API Documentation
The backend includes clear and standardized API documentation using the OpenAPI Specification and GraphQL schema. This supports developers during integration and testing by providing example requests, responses, and query structures.

---

### Data Optimization
Indexes and caching strategies are implemented to enhance performance and scalability. This ensures fast data retrieval, reduced load on the database, and smoother user experience during high traffic periods.

## API Security

Security is a critical part of the Airbnb Clone backend, as the platform handles sensitive user information, payment transactions, and private data. The following measures are implemented to ensure the system is protected from unauthorized access, data breaches, and malicious activities.

---

### Authentication

We implement **token-based authentication** (e.g., JWT) to ensure that only verified users can access protected resources. Each user must log in to receive a valid token, which is required to access their account or perform actions like booking or listing properties.

>  *Why?* This prevents unauthorized users from accessing accounts and ensures user identity verification.

---

### Authorization

Role-based access control is enforced to differentiate between hosts and guests. For example, only hosts can manage properties, while only guests can create bookings.

>  *Why?* This prevents privilege escalation and restricts actions to only those users who are authorized to perform them.

---

### Rate Limiting & Throttling

We apply rate limits to API endpoints to protect against brute-force attacks, abuse, and denial-of-service (DoS) attempts.

> *Why?* This ensures API stability, fairness, and system resilience under heavy or malicious usage.

---

### Data Validation & Sanitization

All user inputs are validated and sanitized to prevent SQL injection, XSS, and other injection-based attacks. Django’s built-in validation and serializers help enforce strict rules.

> *Why?* This protects the application from code injection vulnerabilities and ensures data consistency.

---

### Secure Payments

All payment data is processed through secure channels (e.g., HTTPS) and integrates with trusted third-party payment gateways. Payment records are encrypted and never store sensitive card details directly.

> *Why?* This protects users’ financial information and ensures compliance with payment industry standards.

---

### HTTPS & SSL/TLS

All data transmitted between the client and server is encrypted using SSL/TLS protocols.

> *Why?* This ensures data privacy and integrity, preventing eavesdropping and man-in-the-middle attacks.

---

### Environment Variables & Secrets Management

Sensitive information like API keys, database credentials, and secret tokens are stored securely in environment variables and not exposed in the codebase.

> *Why?* This keeps critical secrets safe and prevents accidental leaks through version control.


Stay tuned for updates and new features