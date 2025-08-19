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

- **Backend Developer:** Responsible for implementing API endpoints, database schemas, and business logic.
- **Database Administrator:** Manages database design, indexing, and optimizations.
- **DevOps Engineer:** Handles deployment, monitoring, and scaling of the backend services.
- **QA Engineer:** Ensures the backend functionalities are thoroughly tested and meet quality standards.
- **Business analyst:** Understands customer’s business processes & Translates customer business needs into requirements
- **Product owner:** Holds responsibility for a product vision and evolution and Makes sure the final product meets customer requirements
- **Project manager:** Makes sure a product or its part is delivered on time and within budget.
- **UI/UX designer:** Creates user journeys for the best user experience and highest conversion rates
- **Software architect:** An architect is an expert-level software engineer who makes executive software design decisions on behalf of an app development team.

## Database Design

### Key entities required

```sql
-- Users Table

CREATE TABLE users (
 id VARCHAR(36) PRIMARY KEY,
 first_name VARCHAR(100) NOT NULL,
 last_name VARCHAR(100) NOT NULL,
 email VARCHAR(100) NOT NULL UNIQUE,
 password VARCHAR(255) NOT NULL,
 phone_number VARCHAR(15),
 address TEXT,
 profile_picture VARCHAR(255),
 bio TEXT,
 is_verified BOOLEAN DEFAULT FALSE,
 last_login TIMESTAMP,
 created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
 updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
 FOREIGN KEY (id) REFERENCES properties(id) ON DELETE CASCADE,
 FOREIGN KEY (id) REFERENCES bookings(id) ON DELETE CASCADE,
 FOREIGN KEY (id) REFERENCES payments(id) ON DELETE CASCADE,
 FOREIGN KEY (id) REFERENCES reviews(id) ON DELETE CASCADE
)

CREATE INDEX idx_email ON users(email);

-- Relationships
User can own multiple properties, make multiple bookings, and write multiple reviews. Each user can also have multiple payments associated with their bookings.
Users <-> Properties
Users <-> Bookings
Users <-> Payments
Users <-> Reviews
```

```sql
-- Properties Table
CREATE TABLE properties (
 id VARCHAR(255) PRIMARY KEY,
 user_id VARCHAR(255) NOT NULL,
 title VARCHAR(255) NOT NULL,
 description TEXT NOT NULL,
 price DECIMAL(10, 2) NOT NULL,
 location VARCHAR(255) NOT NULL,
 created_at TIMESTAMP NOT NULL,
 updated_at TIMESTAMP NOT NULL,
 FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
)

CREATE INDEX idx_user_id ON properties(user_id);
CREATE INDEX idx_location ON properties(location);
CREATE INDEX idx_price ON properties(price);

-- Relationships
A Property belongs to One User. A User can own multiple Properties.
Properties <-> Users
```

```sql
-- Bookings Table
CREATE TABLE bookings (
 id VARCHAR(255) PRIMARY KEY,
 user_id VARCHAR(255) NOT NULL,
 property_id VARCHAR(255) NOT NULL,
 start_date DATE NOT NULL,
 end_date DATE NOT NULL,
 total_price DECIMAL(10, 2) NOT NULL,
 status VARCHAR(50) NOT NULL DEFAULT 'pending',
 created_at TIMESTAMP NOT NULL,
 updated_at TIMESTAMP NOT NULL,
 FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
 FOREIGN KEY (property_id) REFERENCES properties(id) ON DELETE CASCADE
)
CREATE INDEX idx_user_id ON bookings(user_id);
CREATE INDEX idx_property_id ON bookings(property_id);

-- Relationships
A Booking belongs to One User and One Property. A User can make multiple Bookings for different Properties.
Bookings <-> Users
Bookings <-> Properties
```

```sql
-- Payments Table
CREATE TABLE payments (
 id VARCHAR(255) PRIMARY KEY,
 user_id VARCHAR(255) NOT NULL,
 booking_id VARCHAR(255) NOT NULL,
 amount DECIMAL(10, 2) NOT NULL,
 payment_method VARCHAR(50) NOT NULL,
 status VARCHAR(50) NOT NULL DEFAULT 'pending',
 created_at TIMESTAMP NOT NULL,
 updated_at TIMESTAMP NOT NULL,
 FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
 FOREIGN KEY (booking_id) REFERENCES bookings(id) ON DELETE CASCADE
)
CREATE INDEX idx_user_id ON payments(user_id);
CREATE INDEX idx_booking_id ON payments(booking_id);
CREATE INDEX idx_status ON payments(status);

-- Relationships
A Payment belongs to One User and One Booking. A User can make multiple Payments for different Bookings.
Payments <-> Users
Payments <-> Bookings
```

```sql
-- Reviews Table
CREATE TABLE reviews (
 id VARCHAR(255) PRIMARY KEY,
 user_id VARCHAR(255) NOT NULL,
 property_id VARCHAR(255) NOT NULL,
 rating INT CHECK (rating >= 1 AND rating <= 5),
 comment TEXT,
 created_at TIMESTAMP NOT NULL,
 updated_at TIMESTAMP NOT NULL,
 FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
 FOREIGN KEY (property_id) REFERENCES properties(id) ON DELETE CASCADE
)
CREATE INDEX idx_user_id ON reviews(user_id);
CREATE INDEX idx_property_id ON reviews(property_id);
CREATE INDEX idx_rating ON reviews(rating);

-- Relationships
A Review belongs to One User and One Property. A User can write multiple Reviews for different Properties.
Reviews <-> Users
Reviews <-> Properties
```

## Feature Breakdown

### Main Features

- **API Documentation**
  - **Django REST Framework:** Provides a comprehensive RESTful API for handling CRUD operations on user and property data.
  - **GraphQL:** Offers a flexible and efficient query mechanism for interacting with the backend.
- **User Management:** Register new users, authenticate, and manage user profiles.
- **Property Management:** Create, update, retrieve, and delete property listings.
- **Booking System:** Make, update, and manage bookings, including check-in and check-out details.
- **Payment processing:** Handle payment transactions related to bookings.
- **Review System:** Post and manage reviews for properties.
- **Database Optimizations**
  - **Indexing:** Implement indexes for fast retrieval of frequently accessed data.
  - **Caching:** Use caching strategies to reduce database load and improve performance.

## API Security

- **Authentication and Authorization:**
  Implementing robust mechanisms like OAuth 2.0 with OpenID Connect, API keys, and multi-factor authentication (MFA) to verify user and application identities. Granular authorization ensures users and services only access permitted resources based on the principle of least privilege through role-based access control (RBAC).

- **Encryption:**
  Encrypting all data transmitted via APIs using HTTPS and TLS to prevent interception and eavesdropping. This also extends to encrypting sensitive data at rest in databases or storage.

- **Input Validation and Sanitization:**
  Rigorously validating and sanitizing all input data on the server-side to prevent injection attacks (e.g., SQL injection, XSS) and ensure data integrity.

- **Rate Limiting and Throttling:**
  Implementing controls to limit the number of requests an individual client or IP address can make within a given timeframe, protecting against brute-force attacks and Denial-of-Service (DoS) attacks.

- **API Gateways:**
  Utilizing API gateways as a centralized point of enforcement for security policies, including authentication, authorization, rate limiting, and traffic management, before requests reach backend services.

- **Logging and Monitoring:**
  Maintaining comprehensive logs of API access and activity, and implementing real-time monitoring and alerting for suspicious behavior or potential security incidents.

- **Regular Security Audits and Testing:**
  Conducting frequent security audits, penetration testing, and vulnerability scanning to identify and address weaknesses in API implementations and configurations.

- **Error Handling:**
  Implementing secure error handling that avoids exposing sensitive system information or stack traces in API responses.

- **API Key Management:**
  Securely managing and rotating API keys, storing them in environment variables or secure vaults rather than hardcoding them within application code.

- **Principle of Least Privilege and Zero Trust:**
  Granting only the minimum necessary permissions to users and applications, and adopting a Zero Trust security model where no entity, internal or external, is implicitly trusted.

## CI/CD Pipeline

A CI/CD pipeline is an automated process utilized by software development teams to streamline the creation, testing and deployment of applications. "CI" represents continuous integration, where developers frequently merge code changes into a central repository, allowing early detection of issues. "CD" refers to continuous deployment or continuous delivery, which automates the application's release to its intended environment, ensuring that it is readily available to users. This pipeline is vital for teams aiming to improve software quality and speed up delivery through regular, reliable updates.

### Tools used for CI/CD

- GitHub Actions
- Jenkins
- Docker
- Kubernetes
- CircleCI
- Travis CI
- AWS CodePipeline
- Bamboo
- GitLab CI/CD
- Azure DevOps
