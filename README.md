# AirBnB Clone Project
The Airbnb Clone Project is a comprehensive, real-world application designed to simulate the development of a robust booking platform like Airbnb. It involves a deep dive into full-stack development, focusing on backend systems, database design, API development, and application security. This project provides understanding to complex architectures, workflows, and collaborative team dynamics while building a scalable web application.

## ⚙️Technology Stack
> - **Python:** A modern high-level programming language
> - **Django:** A high-level Python web framework used for building the RESTful API.
> - **Django REST Framework:** Provides tools for creating and managing RESTful APIs
> - **PostgreSQL:** A powerful relational database used for data storage
> - **GraphQL:** Allows for flexible and efficient querying of data
> - **Celery:** For handling asynchronous tasks such as sending notifications or processing payments
> - **Redis:** Used for caching and session management
> - **Docker:** Containerization tool for consistent development and deployment environments
> - **CI/CD Pipelines:** Automated pipelines for testing and deploying code changes.

## Team Roles
* **Backend Developer:** Responsible for implementing API endpoints, database schemas, and business logic.
* **Database Administrator:** Manages database design, indexing, and optimizations.
* **DevOps Engineer:** Handles deployment, monitoring, and scaling of the backend services.
* **QA Engineer:** Ensures the backend functionalities are thoroughly tested and meet quality standards.
* **Business analyst:** Understands customer’s business processes & Translates customer business needs into requirements
* **Product owner:** Holds responsibility for a product vision and evolution and Makes sure the final product meets customer requirements
* **Project manager:** Makes sure a product or its part is delivered on time and within budget.
* **UI/UX designer:** Creates user journeys for the best user experience and highest conversion rates
* **Software architect:** An architect is an expert-level software engineer who makes executive software design decisions on behalf of an app development team.

## Database Design
### Key entities required
```sql
Users Table
A user can have multiple properties, make multiple bookings, process payment and review multiple properties
CREATE TABLE users (
 id VARCHAR(255) PRIMARY KEY,
 first_name VARCHAR(255) NOT NULL,
 last_name VARCHAR(255) NOT NULL,
 email VARCHAR(255) NOT NULL UNIQUE,
 password VARCHAR(255) NOT NULL,
 created_at TIMESTAMP NOT NULL,
 updated_at TIMESTAMP NOT ULL
)
```

```sql
Properties Table
One user can own multiple properties, Properties belong to one user. Booking belong to a Property.
```

```sql
Bookings Table
One Booking belong to One Property
```

```sql
Payments Table
A Payment belongs to One Property
```

```sql
Reviews Table
Reviews belongs to multiple Properties. A user can make multiple Reviews.
```

## Feature Breakdown
### Main Features
* **API Documentation**
  - **Django REST Framework:** Provides a comprehensive RESTful API for handling CRUD operations on user and property data.
  - **GraphQL:** Offers a flexible and efficient query mechanism for interacting with the backend.
* User Management: Register new users, authenticate, and manage user profiles.
* Property Management: Create, update, retrieve, and delete property listings.
* Booking System: Make, update, and manage bookings, including check-in and check-out details.
* Payment processing: Handle payment transactions related to bookings.
* Review System: Post and manage reviews for properties.
* Database Optimizations
  - Indexing: Implement indexes for fast retrieval of frequently accessed data.
  - Caching: Use caching strategies to reduce database load and improve performance.

## API Security
* **User Authentication:** This allows registered users to gain access to the platform. It ensures that the user is who they say they are by providing their login details.
* **User Authorization:** This permits the user access to certain endpoints based on their role. It protects the user data and provides a secure way of payment.
* **Rate Limiting:** This prevents abuse of an endpoint due to multiple requests. It protects the database from being overworked.
