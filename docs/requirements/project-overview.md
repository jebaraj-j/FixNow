1. Problem we're solving

FixNow connects customers who need services with service providers who can perform those services.

Example services:

Electrician
Plumber
AC Repair
Appliance Repair
Computer/Mobile Repair
Mechanic

The customer creates a service request, a suitable provider receives it, accepts the job, communicates with the customer, performs the service, and completes the request.

2. Users

We have exactly 3 roles for V1:

👤 Customer

Can:

Register/login
Manage profile
Browse services
Find service providers
Create service request
Upload problem images
Track request
Chat with provider
Receive notifications
Cancel request
Confirm completion
Rate/review provider
View service history
🔧 Service Provider

Can:

Register/login
Create provider profile
Select services
Set availability
Receive requests
Accept/reject requests
Chat with customer
Update job status
Complete jobs
View job history
View ratings
🛡️ Admin

Can:

Login
Manage users
Manage providers
Approve/reject providers
Manage service categories
View service requests
Manage reported content/users
View platform statistics
3. Main Customer Flow

This is our primary business flow.

Register
↓
Login
↓
Customer Dashboard
↓
Choose Service
↓
Browse/Search Providers
↓
Select Provider
↓
Create Service Request
↓
Provider Receives Request
↓
Provider Accepts
↓
Customer Gets Notification
↓
Chat
↓
Provider Starts Service
↓
Service Completed
↓
Customer Confirms
↓
Rating & Review

This flow will guide almost everything we build.

4. Service Request Lifecycle

We'll use:

PENDING
↓
ACCEPTED
↓
ON_THE_WAY
↓
IN_PROGRESS
↓
COMPLETED

Cancellation/rejection:

PENDING → CANCELLED
PENDING → REJECTED
Important business rule

A request cannot randomly jump between states.

For example:

COMPLETED → PENDING ❌
COMPLETED → ACCEPTED ❌

The backend will enforce valid transitions.

This is an important piece of real business logic.

5. Provider Flow
   Provider Registration
   ↓
   Profile Setup
   ↓
   Select Services
   ↓
   Set Availability
   ↓
   Wait for Requests
   ↓
   Receive Request
   ↓
   Accept / Reject
   ↓
   ON_THE_WAY
   ↓
   IN_PROGRESS
   ↓
   COMPLETED
6. Real-Time Flow

This is one of our flagship features.

Without WebSocket

Customer would have to:

Refresh
Refresh
Refresh

to know whether the provider accepted the request.

With WebSocket
Provider
↓
Accept Request
↓
Spring Boot WebSocket
↓
Customer
↓
Instant notification

We'll eventually use WebSocket for:

Request notifications
Status changes
Chat messages
New messages
Provider/customer activity
7. What belongs in V1?

Because we're targeting 20 days, we need scope discipline.

🔴 Must Have

These are essential:

Customer authentication
Provider authentication
Admin authentication
JWT
Role-based authorization
Service categories
Provider profiles
Provider availability
Service requests
Request status lifecycle
Customer dashboard
Provider dashboard
REST APIs
MySQL
React
WebSocket notifications
Real-time chat
Reviews
Search/filter
Validation
Exception handling
🟡 Important but later
File/image upload
Pagination
Advanced filtering
Activity logs
Admin statistics
Email notifications
🟢 Optional

We won't let these delay the project:

Payment gateway
GPS/live location
Maps
AI provider matching
Mobile application
Microservices

Do not add payment or maps right now. They sound impressive, but they can consume a huge amount of development time without addressing the main gaps we're trying to fix.

8. Non-Functional Requirements

These are important because you want this to look like a professional application.

FixNow should be:

Secure
Passwords hashed
JWT authentication
Role authorization
Protected APIs
No secrets in GitHub
Reliable
Validation
Exception handling
Proper HTTP status codes
Transaction management
Maintainable
Layered architecture
DTOs
Clear naming
Separation of responsibilities
Testable
JUnit
Mockito
Controller/service tests
Documented
Swagger
ER diagram
Architecture diagram
README
9. Our First Architecture

At a high level:

                    ┌───────────────┐
                    │    Customer   │
                    └───────┬───────┘
                            │
                            │
                    ┌───────▼───────┐
                    │  React.js     │
                    │  Frontend     │
                    └───────┬───────┘
                            │
                    REST / WebSocket
                            │
                    ┌───────▼───────┐
                    │ Spring Boot   │
                    │ Backend       │
                    └───────┬───────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
        Spring Security   Services     WebSocket
              │             │             │
              └─────────────┼─────────────┘
                            │
                    ┌───────▼───────┐
                    │ JPA/Hibernate │
                    └───────┬───────┘
                            │
                    ┌───────▼───────┐
                    │    MySQL      │
                    └───────────────┘