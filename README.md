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
`id`: Primary Key
`property_id`: Foreign Key referencing properties(id)
`guest_id`: Foriegn Key referencing Users(id)
`check_in_date`: Start date of user stay
`check_out_date`: End date of user stay

**Relationships:**
- A booking is made by one user (guest).
- A booking is for one specific property.
- A booking can have one payment.
- A booking can receive one review.

### Payments
Represents Payment records for bookings

**Key Fields:**
`id`: Primary key
`booking_id`: Foreign Key referencing Bookings(id)
`amount`: Total amount paid for the booking
`payment_status`: Status of payment(pending, completed, failed)
`timestamp`: Time of transaction

**Relationships:**
- A payment is linked to one booking.
- Each booking has at most one payment record.

### Reviews
Ratings and feedback submitted by guests after their stay

**Key Fields:** 
`id`: Primary Key 
`booking_id`: Foreign key referencing Bookings(id)
`user_id`: Foreign key referencing Users(id)
`rating`: Star rating (1 to 5)
`comment`: Guest's written feedback

**Relationships:**
- A review is written by one user.
- A review is linked to one booking.
- Indirectly, a review relates to a property through the booking.

---

Stay tuned for updates and new features