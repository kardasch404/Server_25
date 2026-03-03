# 🚀 SERVER 25 - Complete Microservices Architecture Documentation

> **The Ultimate Job Search Automation Platform**  
> A production-ready microservices architecture built with NestJS, Clean Architecture, DDD, and AI-powered automation.

---

## 📋 Table of Contents

1. [System Overview](#system-overview)
2. [Architecture Diagrams](#architecture-diagrams)
3. [Microservices Details](#microservices-details)
4. [Class Diagrams](#class-diagrams)
5. [Sequence Diagrams](#sequence-diagrams)
6. [Database Schema](#database-schema)
7. [API Documentation](#api-documentation)
8. [Deployment Guide](#deployment-guide)

---

## 🌐 System Overview

Server 25 is an intelligent job search automation platform that helps candidates find jobs faster through AI-powered matching, automated applications, and professional networking. Built with microservices architecture for scalability and resilience.

### Core Services

| Service | Port | gRPC Port | Description |
|---------|------|-----------|-------------|
| **Auth Service** | 4001 | 5001 | Authentication, Authorization, Dynamic RBAC |
| **Profile Service** | 4002 | 5002 | User Profiles, CV, Documents Management |
| **Job Service** | 4003 | 5003 | Job Scraping, Search, Matching |
| **Application Service** | 4004 | 5004 | Application Management, Email Campaigns |
| **Networking Service** | 4005 | 5005 | Premium Networking Automation |
| **Payment Service** | 4006 | 5006 | Stripe Integration, Subscriptions |
| **AI Service** | 4007 | 5007 | CV Analysis, Matching, Recommendations |
| **B2B API Service** | 4008 | 5008 | Enterprise API Access |
| **Notification Service** | 4009 | 5009 | Email, SMS, WebSocket Notifications |
| **Analytics Service** | 4010 | 5010 | Statistics, Reports, Insights |

### Technology Stack

```mermaid
graph TB
    subgraph "Frontend Layer"
        A[React Native Mobile App]
        B[Admin Dashboard - React]
    end
    
    subgraph "API Gateway"
        C[Kong API Gateway<br/>Port: 3000]
    end
    
    subgraph "Microservices - NestJS + TypeScript"
        D[🔐 Auth Service<br/>4001]
        E[👤 Profile Service<br/>4002]
        F[💼 Job Service<br/>4003]
        G[📧 Application Service<br/>4004]
        H[🤝 Networking Service<br/>4005]
        I[💳 Payment Service<br/>4006]
        J[🤖 AI Service<br/>4007]
        K[🏢 B2B API Service<br/>4008]
        L[🔔 Notification Service<br/>4009]
        M[📊 Analytics Service<br/>4010]
    end
    
    subgraph "Data Layer"
        N[(PostgreSQL<br/>User, Profile, Jobs)]
        O[(Redis<br/>Cache + Sessions)]
        P[(Elasticsearch<br/>Job Search)]
        Q[(AWS S3<br/>Documents)]
    end
    
    subgraph "Message Queue"
        R[Apache Kafka<br/>Event Streaming]
    end
    
    subgraph "External Services"
        S[OpenAI API<br/>GPT-4]
        T[Stripe API<br/>Payments]
        U[AWS SES<br/>Email]
        V[LinkedIn API<br/>Scraping]
    end
    
    A --> C
    B --> C
    C --> D & E & F & G & H & I & J & K
    
    D & E & F & G & H & I & J & K --> N
    D & E & F & G --> O
    F --> P
    E --> Q
    
    D & E & F & G & H & I & J --> R
    R --> L & M
    
    J --> S
    I --> T
    L --> U
    F --> V
    
    style A fill:#61dafb
    style B fill:#61dafb
    style C fill:#009639
    style D fill:#e0234e
    style E fill:#e0234e
    style F fill:#e0234e
    style G fill:#e0234e
    style H fill:#e0234e
    style I fill:#e0234e
    style J fill:#e0234e
    style K fill:#e0234e
    style L fill:#4caf50
    style M fill:#4caf50
    style R fill:#231f20
```

---

## 🏗️ Architecture Diagrams

### High-Level System Architecture

```mermaid
C4Context
    title System Context Diagram - Server 25 Platform
    
    Person(candidate, "Job Seeker", "Searches and applies for jobs")
    Person(admin, "Administrator", "Manages platform, users, jobs")
    Person(b2b, "B2B Client", "Accesses data via API")
    
    System(server25, "Server 25 Platform", "Job search automation and networking platform")
    
    System_Ext(linkedin, "LinkedIn", "Job scraping source")
    System_Ext(indeed, "Indeed", "Job scraping source")
    System_Ext(openai, "OpenAI", "AI analysis and matching")
    System_Ext(stripe, "Stripe", "Payment processing")
    System_Ext(aws, "AWS Services", "Email, Storage, Infrastructure")
    
    Rel(candidate, server25, "Uses", "HTTPS/Mobile App")
    Rel(admin, server25, "Manages", "HTTPS/Dashboard")
    Rel(b2b, server25, "Consumes API", "REST/GraphQL")
    
    Rel(server25, linkedin, "Scrapes jobs", "Playwright")
    Rel(server25, indeed, "Scrapes jobs", "Playwright")
    Rel(server25, openai, "Analyzes profiles", "API")
    Rel(server25, stripe, "Processes payments", "API")
    Rel(server25, aws, "Sends emails, stores files", "API")
```

### Microservices Communication Flow

```mermaid
graph TB
    subgraph "Client Layer"
        MOBILE[📱 Mobile App]
        WEB[🖥️ Admin Dashboard]
    end
    
    subgraph "Gateway Layer"
        GATEWAY[🚪 API Gateway<br/>Kong - Port 3000<br/>Rate Limiting, Auth, Routing]
    end
    
    subgraph "Service Layer"
        AUTH[🔐 Auth Service<br/>REST: 4001 | gRPC: 5001<br/>JWT, RBAC, Permissions]
        PROFILE[👤 Profile Service<br/>REST: 4002 | gRPC: 5002<br/>CV, Experience, Skills]
        JOB[💼 Job Service<br/>REST: 4003 | gRPC: 5003<br/>Scraping, Search, Matching]
        APP[📧 Application Service<br/>REST: 4004 | gRPC: 5004<br/>Apply, Email Campaigns]
        NET[🤝 Networking Service<br/>REST: 4005 | gRPC: 5005<br/>Auto Connect, Follow-up]
        PAY[💳 Payment Service<br/>REST: 4006 | gRPC: 5006<br/>Subscriptions, Invoices]
        AI[🤖 AI Service<br/>REST: 4007 | gRPC: 5007<br/>CV Analysis, Recommendations]
        B2B[🏢 B2B API Service<br/>REST: 4008 | gRPC: 5008<br/>Enterprise Data Access]
        NOTIFY[🔔 Notification Service<br/>REST: 4009 | gRPC: 5009<br/>Email, SMS, Push]
        ANALYTICS[📊 Analytics Service<br/>REST: 4010 | gRPC: 5010<br/>Statistics, Reports]
    end
    
    subgraph "Data Layer"
        POSTGRES[(🐘 PostgreSQL<br/>Primary Database)]
        REDIS[(⚡ Redis<br/>Cache + Sessions)]
        ELASTIC[(🔍 Elasticsearch<br/>Job Search Engine)]
        S3[(☁️ AWS S3<br/>Document Storage)]
    end
    
    subgraph "Message Layer"
        KAFKA[📨 Apache Kafka<br/>Event Bus<br/>Topics: user, job, application, payment]
    end
    
    MOBILE --> GATEWAY
    WEB --> GATEWAY
    
    GATEWAY --> AUTH
    GATEWAY --> PROFILE
    GATEWAY --> JOB
    GATEWAY --> APP
    GATEWAY --> NET
    GATEWAY --> PAY
    GATEWAY --> B2B
    
    APP -.->|gRPC| AUTH
    APP -.->|gRPC| PROFILE
    APP -.->|gRPC| JOB
    NET -.->|gRPC| AUTH
    PAY -.->|gRPC| AUTH
    AI -.->|gRPC| PROFILE
    
    AUTH --> POSTGRES
    PROFILE --> POSTGRES
    JOB --> POSTGRES
    APP --> POSTGRES
    NET --> POSTGRES
    PAY --> POSTGRES
    B2B --> POSTGRES
    
    AUTH --> REDIS
    PROFILE --> REDIS
    JOB --> REDIS
    APP --> REDIS
    
    JOB --> ELASTIC
    PROFILE --> S3
    
    AUTH --> KAFKA
    PROFILE --> KAFKA
    JOB --> KAFKA
    APP --> KAFKA
    NET --> KAFKA
    PAY --> KAFKA
    
    KAFKA --> NOTIFY
    KAFKA --> ANALYTICS
    
    style MOBILE fill:#61dafb
    style WEB fill:#61dafb
    style GATEWAY fill:#009639
    style AUTH fill:#e0234e
    style PROFILE fill:#e0234e
    style JOB fill:#e0234e
    style APP fill:#e0234e
    style NET fill:#e0234e
    style PAY fill:#e0234e
    style AI fill:#e0234e
    style B2B fill:#e0234e
    style NOTIFY fill:#4caf50
    style ANALYTICS fill:#4caf50
    style KAFKA fill:#231f20
```

### Event-Driven Architecture

```mermaid
graph LR
    subgraph "Event Publishers"
        AUTH[Auth Service]
        PROFILE[Profile Service]
        JOB[Job Service]
        APP[Application Service]
        NET[Networking Service]
        PAY[Payment Service]
    end
    
    subgraph "Kafka Topics"
        T1[user.events<br/>UserRegistered<br/>UserLoggedIn<br/>UserUpdated]
        T2[profile.events<br/>ProfileCreated<br/>ProfileUpdated<br/>CVUploaded]
        T3[job.events<br/>JobScraped<br/>JobExpired<br/>JobMatched]
        T4[application.events<br/>ApplicationSent<br/>ApplicationViewed<br/>InterviewScheduled]
        T5[networking.events<br/>ConnectionSent<br/>ConnectionAccepted<br/>MessageReceived]
        T6[payment.events<br/>SubscriptionCreated<br/>PaymentSucceeded<br/>PaymentFailed]
    end
    
    subgraph "Event Consumers"
        NOTIFY[Notification Service]
        ANALYTICS[Analytics Service]
        AI[AI Service]
        AUDIT[Audit Service]
    end
    
    AUTH -->|Publish| T1
    PROFILE -->|Publish| T2
    JOB -->|Publish| T3
    APP -->|Publish| T4
    NET -->|Publish| T5
    PAY -->|Publish| T6
    
    T1 --> NOTIFY & ANALYTICS & AUDIT
    T2 --> AI & ANALYTICS
    T3 --> NOTIFY & ANALYTICS & AI
    T4 --> NOTIFY & ANALYTICS & AUDIT
    T5 --> NOTIFY & ANALYTICS
    T6 --> NOTIFY & ANALYTICS & AUDIT
    
    style AUTH fill:#e0234e
    style PROFILE fill:#e0234e
    style JOB fill:#e0234e
    style APP fill:#e0234e
    style NET fill:#e0234e
    style PAY fill:#e0234e
    style NOTIFY fill:#4caf50
    style ANALYTICS fill:#4caf50
    style AI fill:#4caf50
    style AUDIT fill:#4caf50
```

---

## 🎯 Microservices Details

### 1. Auth Service (Port 4001)

**Responsibilities:**
- User registration and authentication
- JWT token generation and validation
- Dynamic RBAC (Role-Based Access Control)
- Permission evaluation with constraints
- Session management
- Password reset and email verification

**Key Features:**
- ✅ Dynamic role assignment with expiration
- ✅ Granular permissions (resource.action)
- ✅ Temporal and contextual constraints
- ✅ Multi-role support per user
- ✅ Refresh token rotation
- ✅ Audit logging

**Tech Stack:**
- NestJS + TypeScript
- PostgreSQL (users, roles, permissions)
- Redis (sessions, blacklist)
- JWT + bcrypt
- Passport.js

---

### 2. Profile Service (Port 4002)

**Responsibilities:**
- User profile management
- CV generation and parsing
- Experience and education tracking
- Skills management
- Document storage (CV, cover letters)
- LinkedIn/GitHub import

**Key Features:**
- ✅ Multi-folder document organization
- ✅ CV generation from profile
- ✅ Profile completeness calculation
- ✅ LinkedIn profile import
- ✅ GitHub integration
- ✅ S3 document storage

**Tech Stack:**
- NestJS + TypeScript
- PostgreSQL (profiles, experiences)
- AWS S3 (document storage)
- Redis (cache)

---

### 3. Job Service (Port 4003)

**Responsibilities:**
- Job scraping from multiple platforms
- Job search and filtering
- Duplicate detection
- Job matching algorithm
- Saved searches and alerts
- Favorites management

**Key Features:**
- ✅ Multi-platform scraping (LinkedIn, Indeed)
- ✅ Elasticsearch for fast search
- ✅ AI-powered matching score
- ✅ Real-time job alerts
- ✅ Advanced filtering
- ✅ Duplicate detection via hashing

**Tech Stack:**
- NestJS + TypeScript
- Python + Playwright (scraping)
- PostgreSQL (jobs, companies)
- Elasticsearch (search)
- Redis (cache)

---

### 4. Application Service (Port 4004)

**Responsibilities:**
- Application submission
- Email campaign management
- Bulk application sending
- Application tracking
- Status updates
- Email personalization

**Key Features:**
- ✅ One-click multiple applications
- ✅ Email personalization with AI
- ✅ Attachment selection
- ✅ Application timeline tracking
- ✅ Email delivery status
- ✅ Retry mechanism

**Tech Stack:**
- NestJS + TypeScript
- PostgreSQL (applications, emails)
- AWS SES (email sending)
- Redis (queue)

---

### 5. Networking Service (Port 4005) - PREMIUM

**Responsibilities:**
- Automated LinkedIn connection requests
- Follow-up message automation
- HR targeting by location/industry
- Connection tracking
- Response management
- Campaign analytics

**Key Features:**
- ✅ Smart HR targeting
- ✅ Auto-connect with personalized messages
- ✅ Scheduled follow-ups
- ✅ Connection acceptance tracking
- ✅ Response detection
- ✅ Campaign pause/resume

**Tech Stack:**
- NestJS + TypeScript
- PostgreSQL (campaigns, connections)
- Playwright (LinkedIn automation)
- Redis (queue)

---

### 6. Payment Service (Port 4006)

**Responsibilities:**
- Subscription management
- Payment processing
- Invoice generation
- Plan upgrades/downgrades
- Refund handling
- Billing cycle management

**Key Features:**
- ✅ Stripe integration
- ✅ Multiple subscription plans
- ✅ Auto-renewal
- ✅ Proration on upgrades
- ✅ Invoice PDF generation
- ✅ Payment webhooks

**Tech Stack:**
- NestJS + TypeScript
- PostgreSQL (subscriptions, payments)
- Stripe API
- Redis (cache)

---

### 7. AI Service (Port 4007)

**Responsibilities:**
- CV analysis and parsing
- Job matching score calculation
- Email personalization
- Cover letter generation
- Skill extraction
- Profile optimization suggestions

**Key Features:**
- ✅ OpenAI GPT-4 integration
- ✅ CV parsing and extraction
- ✅ Matching algorithm (0-100%)
- ✅ Personalized email generation
- ✅ Missing skills detection
- ✅ Profile improvement recommendations

**Tech Stack:**
- NestJS + TypeScript
- OpenAI API
- PostgreSQL (analysis history)
- Redis (cache)

---

### 8. B2B API Service (Port 4008)

**Responsibilities:**
- Enterprise API access
- API key management
- Rate limiting per plan
- Data analytics endpoints
- Usage tracking
- API documentation

**Key Features:**
- ✅ RESTful + GraphQL APIs
- ✅ Company data by sector
- ✅ Job market statistics
- ✅ Employee data access
- ✅ Custom rate limits
- ✅ Usage analytics

**Tech Stack:**
- NestJS + TypeScript
- PostgreSQL (API clients, requests)
- Redis (rate limiting)
- GraphQL

---

### 9. Notification Service (Port 4009)

**Responsibilities:**
- Email notifications
- SMS notifications
- Push notifications
- WebSocket real-time updates
- Notification preferences
- Template management

**Key Features:**
- ✅ Multi-channel notifications
- ✅ Template engine
- ✅ User preferences
- ✅ Delivery tracking
- ✅ Retry logic
- ✅ Real-time WebSocket

**Tech Stack:**
- NestJS + TypeScript
- AWS SES (email)
- Twilio (SMS)
- Socket.io (WebSocket)
- Redis (queue)

---

### 10. Analytics Service (Port 4010)

**Responsibilities:**
- User statistics calculation
- Application success rates
- Platform-wide metrics
- Performance tracking
- Report generation
- Data visualization

**Key Features:**
- ✅ Real-time dashboards
- ✅ Success rate calculation
- ✅ Sector performance analysis
- ✅ Monthly trends
- ✅ Comparative analytics
- ✅ Export to PDF/Excel

**Tech Stack:**
- NestJS + TypeScript
- PostgreSQL (aggregated data)
- Redis (cache)
- Chart.js

---

## 🔄 Sequence Diagrams

### 1. User Registration & Login Flow

```mermaid
sequenceDiagram
    participant Client
    participant Gateway
    participant AuthService
    participant PostgreSQL
    participant Redis
    participant Kafka
    participant NotificationService
    
    Note over Client,NotificationService: User Registration Flow
    
    Client->>Gateway: POST /api/auth/register
    Gateway->>AuthService: register(dto)
    
    AuthService->>AuthService: Validate email format
    AuthService->>PostgreSQL: Check if email exists
    PostgreSQL-->>AuthService: No user found
    
    AuthService->>AuthService: Hash password (bcrypt)
    AuthService->>PostgreSQL: BEGIN TRANSACTION
    AuthService->>PostgreSQL: INSERT INTO users
    AuthService->>PostgreSQL: INSERT INTO user_roles (CANDIDATE)
    AuthService->>PostgreSQL: COMMIT
    PostgreSQL-->>AuthService: User created
    
    AuthService->>Kafka: Publish UserRegisteredEvent
    AuthService-->>Gateway: UserDTO
    Gateway-->>Client: 201 Created {user}
    
    Kafka-->>NotificationService: Consume UserRegisteredEvent
    NotificationService->>NotificationService: Send welcome email
    
    Note over Client,NotificationService: User Login Flow
    
    Client->>Gateway: POST /api/auth/login
    Gateway->>AuthService: login(credentials)
    
    AuthService->>PostgreSQL: findByEmail(email)
    PostgreSQL-->>AuthService: User with hashed password
    
    AuthService->>AuthService: Compare password (bcrypt)
    
    alt Password valid
        AuthService->>PostgreSQL: Get user roles & permissions
        PostgreSQL-->>AuthService: Roles & Permissions
        
        AuthService->>AuthService: Generate JWT access token (15min)
        AuthService->>AuthService: Generate refresh token (7days)
        
        AuthService->>PostgreSQL: Save refresh token
        AuthService->>PostgreSQL: Update lastLoginAt
        AuthService->>Redis: Cache user permissions
        
        AuthService->>Kafka: Publish UserLoggedInEvent
        AuthService-->>Gateway: {accessToken, refreshToken, user}
        Gateway-->>Client: 200 OK
    else Password invalid
        AuthService-->>Gateway: UnauthorizedException
        Gateway-->>Client: 401 Unauthorized
    end
```

### 2. Complete Job Application Flow

```mermaid
sequenceDiagram
    participant Client
    participant Gateway
    participant AuthGuard
    participant ApplicationService
    participant ProfileService
    participant JobService
    participant AIService
    participant PostgreSQL
    participant Kafka
    participant NotificationService
    
    Client->>Gateway: POST /api/applications (JWT + jobIds[])
    Gateway->>AuthGuard: canActivate()
    AuthGuard->>AuthGuard: Verify JWT
    AuthGuard->>Redis: Get cached permissions
    AuthGuard-->>Gateway: ✓ Authorized (userId)
    
    Gateway->>ApplicationService: createApplications(userId, dto)
    
    ApplicationService->>ProfileService: getProfile(userId) [gRPC]
    ProfileService-->>ApplicationService: ProfileDTO
    
    ApplicationService->>ProfileService: getDocuments(userId, folderId) [gRPC]
    ProfileService-->>ApplicationService: Document[]
    
    loop For each jobId
        ApplicationService->>JobService: getJob(jobId) [gRPC]
        JobService-->>ApplicationService: JobOfferDTO
        
        ApplicationService->>AIService: generateCoverLetter(profile, job) [gRPC]
        AIService->>AIService: Call OpenAI GPT-4
        AIService-->>ApplicationService: Personalized cover letter
        
        ApplicationService->>AIService: generateEmail(profile, job) [gRPC]
        AIService-->>ApplicationService: Personalized email
        
        ApplicationService->>PostgreSQL: BEGIN TRANSACTION
        ApplicationService->>PostgreSQL: INSERT INTO applications
        ApplicationService->>PostgreSQL: INSERT INTO email_logs
        ApplicationService->>PostgreSQL: COMMIT
        
        ApplicationService->>ApplicationService: Send email via AWS SES
        
        ApplicationService->>Kafka: Publish ApplicationSentEvent
    end
    
    ApplicationService-->>Gateway: ApplicationDTO[]
    Gateway-->>Client: 201 Created {applications}
    
    Kafka-->>NotificationService: Consume ApplicationSentEvent
    NotificationService->>NotificationService: Send confirmation notification
```

### 3. Job Scraping & Matching Flow

```mermaid
sequenceDiagram
    participant Scheduler
    participant JobService
    participant ScrapingWorker
    participant LinkedIn
    participant Indeed
    participant PostgreSQL
    participant Elasticsearch
    participant AIService
    participant Kafka
    
    Note over Scheduler,Kafka: Automated Job Scraping (Cron: Every 2 hours)
    
    Scheduler->>JobService: Trigger scraping job
    JobService->>PostgreSQL: INSERT INTO scraping_jobs (status: RUNNING)
    
    JobService->>ScrapingWorker: Start scraping (Python + Playwright)
    
    par Scrape LinkedIn
        ScrapingWorker->>LinkedIn: Navigate to jobs page
        LinkedIn-->>ScrapingWorker: HTML content
        ScrapingWorker->>ScrapingWorker: Parse job listings
    and Scrape Indeed
        ScrapingWorker->>Indeed: Navigate to jobs page
        Indeed-->>ScrapingWorker: HTML content
        ScrapingWorker->>ScrapingWorker: Parse job listings
    end
    
    loop For each scraped job
        ScrapingWorker->>ScrapingWorker: Generate hash (title + company + location)
        ScrapingWorker->>PostgreSQL: Check if hash exists
        
        alt Job is new
            ScrapingWorker->>PostgreSQL: INSERT INTO job_offers
            ScrapingWorker->>Elasticsearch: Index job for search
            ScrapingWorker->>Kafka: Publish JobScrapedEvent
        else Job exists
            ScrapingWorker->>ScrapingWorker: Skip duplicate
        end
    end
    
    ScrapingWorker-->>JobService: Scraping completed (count)
    JobService->>PostgreSQL: UPDATE scraping_jobs (status: COMPLETED)
    
    Note over Scheduler,Kafka: AI-Powered Job Matching
    
    Kafka-->>AIService: Consume JobScrapedEvent
    AIService->>PostgreSQL: Get all active user profiles
    
    loop For each profile
        AIService->>AIService: Calculate matching score (0-100%)
        
        alt Score >= 70%
            AIService->>Kafka: Publish JobMatchedEvent
        end
    end
    
    Kafka-->>NotificationService: Consume JobMatchedEvent
    NotificationService->>NotificationService: Send job alert email
```

### 4. Premium Networking Automation Flow

```mermaid
sequenceDiagram
    participant Client
    participant Gateway
    participant NetworkingService
    participant AuthService
    participant PaymentService
    participant PostgreSQL
    participant LinkedInBot
    participant Kafka
    participant NotificationService
    
    Note over Client,NotificationService: Create Networking Campaign (Premium Feature)
    
    Client->>Gateway: POST /api/networking/campaigns (JWT)
    Gateway->>AuthService: validatePermission(userId, "premium.networking")
    
    AuthService->>PaymentService: checkSubscription(userId) [gRPC]
    PaymentService-->>AuthService: {plan: "PREMIUM", active: true}
    AuthService-->>Gateway: ✓ Authorized
    
    Gateway->>NetworkingService: createCampaign(userId, dto)
    
    NetworkingService->>PostgreSQL: INSERT INTO networking_campaigns
    NetworkingService->>PostgreSQL: Set status = SCHEDULED
    
    NetworkingService->>Kafka: Publish CampaignCreatedEvent
    NetworkingService-->>Gateway: CampaignDTO
    Gateway-->>Client: 201 Created
    
    Note over Client,NotificationService: Automated Campaign Execution
    
    NetworkingService->>NetworkingService: Start campaign worker
    NetworkingService->>PostgreSQL: Get campaign targets (RH in Casablanca IT)
    
    loop For each target (max 50/day)
        NetworkingService->>LinkedInBot: Navigate to profile (Playwright)
        LinkedInBot->>LinkedInBot: Click "Connect" button
        LinkedInBot->>LinkedInBot: Type personalized message
        LinkedInBot->>LinkedInBot: Send connection request
        
        NetworkingService->>PostgreSQL: INSERT INTO connections (status: PENDING)
        NetworkingService->>NetworkingService: Wait random delay (30-60s)
    end
    
    NetworkingService->>PostgreSQL: UPDATE campaign (connectionsSent++)
    NetworkingService->>Kafka: Publish ConnectionsSentEvent
    
    Note over Client,NotificationService: Connection Accepted - Auto Follow-up
    
    LinkedInBot->>NetworkingService: Webhook: Connection accepted
    NetworkingService->>PostgreSQL: UPDATE connection (status: ACCEPTED)
    
    NetworkingService->>NetworkingService: Wait 24 hours
    
    NetworkingService->>LinkedInBot: Send follow-up message
    LinkedInBot->>LinkedInBot: Type follow-up message
    LinkedInBot->>LinkedInBot: Send message
    
    NetworkingService->>PostgreSQL: UPDATE connection (followUpCount++)
    NetworkingService->>Kafka: Publish FollowUpSentEvent
    
    Kafka-->>NotificationService: Consume events
    NotificationService->>NotificationService: Send campaign updates
```

### 5. Payment & Subscription Flow

```mermaid
sequenceDiagram
    participant Client
    participant Gateway
    participant PaymentService
    participant Stripe
    participant PostgreSQL
    participant Kafka
    participant NotificationService
    participant NetworkingService
    
    Note over Client,NetworkingService: Upgrade to Premium Plan
    
    Client->>Gateway: POST /api/payments/subscribe (JWT + planId)
    Gateway->>PaymentService: createSubscription(userId, planId)
    
    PaymentService->>PostgreSQL: Get subscription plan details
    PostgreSQL-->>PaymentService: PlanDTO (PREMIUM, 29.99€/month)
    
    PaymentService->>Stripe: Create customer
    Stripe-->>PaymentService: Customer ID
    
    PaymentService->>Stripe: Create subscription
    Stripe-->>PaymentService: Subscription ID + Payment Intent
    
    PaymentService-->>Gateway: {clientSecret, subscriptionId}
    Gateway-->>Client: 200 OK {clientSecret}
    
    Client->>Client: Show Stripe payment form
    Client->>Stripe: Confirm payment (card details)
    
    Stripe->>PaymentService: Webhook: payment_intent.succeeded
    
    PaymentService->>PostgreSQL: BEGIN TRANSACTION
    PaymentService->>PostgreSQL: INSERT INTO subscriptions (status: ACTIVE)
    PaymentService->>PostgreSQL: INSERT INTO payments (status: COMPLETED)
    PaymentService->>PostgreSQL: COMMIT
    
    PaymentService->>Kafka: Publish SubscriptionActivatedEvent
    
    Kafka-->>NotificationService: Consume event
    NotificationService->>NotificationService: Send welcome to premium email
    
    Kafka-->>NetworkingService: Consume event
    NetworkingService->>NetworkingService: Unlock premium features
    
    Note over Client,NetworkingService: Auto-Renewal (Monthly)
    
    Stripe->>PaymentService: Webhook: invoice.payment_succeeded
    
    PaymentService->>PostgreSQL: INSERT INTO payments
    PaymentService->>PostgreSQL: UPDATE subscription (nextBillingDate)
    
    PaymentService->>Kafka: Publish PaymentSucceededEvent
    
    Kafka-->>NotificationService: Consume event
    NotificationService->>NotificationService: Send invoice email
```

### 6. B2B API Access Flow

```mermaid
sequenceDiagram
    participant B2BClient
    participant Gateway
    participant B2BService
    participant RateLimiter
    participant PostgreSQL
    participant JobService
    participant Redis
    
    Note over B2BClient,Redis: B2B Client API Request
    
    B2BClient->>Gateway: GET /api/b2b/companies?sector=IT (API Key in header)
    Gateway->>B2BService: getCompaniesBySector(apiKey, sector)
    
    B2BService->>PostgreSQL: Validate API key
    PostgreSQL-->>B2BService: APIClientDTO {plan: ENTERPRISE, limit: 10000/day}
    
    B2BService->>Redis: Check rate limit (apiKey)
    Redis-->>B2BService: Current usage: 523/10000
    
    alt Under limit
        B2BService->>Redis: Increment usage counter
        
        B2BService->>PostgreSQL: Get companies by sector
        PostgreSQL-->>B2BService: Company[]
        
        B2BService->>PostgreSQL: Log API request
        
        B2BService-->>Gateway: CompanyDTO[]
        Gateway-->>B2BClient: 200 OK {data, remaining: 9477}
    else Over limit
        B2BService-->>Gateway: RateLimitExceededException
        Gateway-->>B2BClient: 429 Too Many Requests
    end
```

---

## 🗄️ Database Schema

### Entity Relationship Diagram

```mermaid
erDiagram
    USER ||--o{ USER_ROLE : has
    USER ||--o{ PROFILE : owns
    USER ||--o{ DOCUMENT_FOLDER : organizes
    USER ||--o{ APPLICATION : submits
    USER ||--o{ SUBSCRIPTION : subscribes
    USER ||--o{ SAVED_SEARCH : creates
    USER ||--o{ NETWORKING_CAMPAIGN : runs
    
    ROLE ||--o{ USER_ROLE : assigned
    ROLE ||--o{ ROLE_PERMISSION : has
    PERMISSION ||--o{ ROLE_PERMISSION : granted
    
    PROFILE ||--o{ EXPERIENCE : has
    PROFILE ||--o{ EDUCATION : has
    PROFILE ||--o{ PROFILE_SKILL : has
    SKILL ||--o{ PROFILE_SKILL : used
    
    DOCUMENT_FOLDER ||--o{ DOCUMENT : contains
    
    COMPANY ||--o{ JOB_OFFER : posts
    JOB_OFFER ||--o{ APPLICATION : receives
    JOB_OFFER ||--o{ FAVORITE : favorited
    
    APPLICATION ||--o{ APPLICATION_EVENT : tracks
    APPLICATION ||--o{ EMAIL_LOG : sends
    
    SUBSCRIPTION_PLAN ||--o{ SUBSCRIPTION : defines
    SUBSCRIPTION ||--o{ PAYMENT : generates
    
    NETWORKING_CAMPAIGN ||--o{ CONNECTION : manages
    
    API_CLIENT ||--o{ API_REQUEST : makes
    
    USER {
        uuid id PK
        string email UK
        string password_hash
        string firstName
        string lastName
        string phone
        timestamp createdAt
        timestamp updatedAt
        timestamp lastLogin
        boolean isVerified
        boolean isActive
    }
    
    ROLE {
        uuid id PK
        string name UK
        string code UK
        string description
        int priority
        boolean isSystem
        timestamp createdAt
    }
    
    PERMISSION {
        uuid id PK
        string resource
        string action
        string code UK
        json metadata
        timestamp createdAt
    }
    
    USER_ROLE {
        uuid userId FK
        uuid roleId FK
        timestamp assignedAt
        timestamp expiresAt
        boolean isActive
    }
    
    ROLE_PERMISSION {
        uuid roleId FK
        uuid permissionId FK
        boolean canDelegate
        json constraints
        timestamp grantedAt
    }
    
    PROFILE {
        uuid id PK
        uuid userId FK
        string title
        text summary
        string country
        string city
        decimal desiredSalary
        string currency
        boolean remoteOnly
        string linkedinUrl
        string githubUrl
        timestamp updatedAt
    }
    
    EXPERIENCE {
        uuid id PK
        uuid profileId FK
        string company
        string position
        text description
        date startDate
        date endDate
        boolean isCurrent
        string location
    }
    
    EDUCATION {
        uuid id PK
        uuid profileId FK
        string institution
        string degree
        string field
        date startDate
        date endDate
    }
    
    SKILL {
        uuid id PK
        string name UK
        string category
    }
    
    DOCUMENT_FOLDER {
        uuid id PK
        uuid userId FK
        string name
        string speciality
        string path
        timestamp createdAt
    }
    
    DOCUMENT {
        uuid id PK
        uuid folderId FK
        uuid userId FK
        string name
        string s3Key
        string s3Url
        string mimeType
        bigint fileSize
        timestamp uploadedAt
    }
    
    COMPANY {
        uuid id PK
        string name
        string industry
        string website
        string country
        string city
        int employeeCount
    }
    
    JOB_OFFER {
        uuid id PK
        uuid companyId FK
        string title
        text description
        string country
        string city
        decimal salaryMin
        decimal salaryMax
        string currency
        boolean isRemote
        string sourceUrl
        string sourcePlatform
        string contactEmail
        string hash UK
        timestamp publishedAt
        timestamp scrapedAt
        timestamp expiresAt
    }
    
    APPLICATION {
        uuid id PK
        uuid userId FK
        uuid jobOfferId FK
        string status
        text coverLetter
        timestamp appliedAt
        timestamp lastUpdated
    }
    
    APPLICATION_EVENT {
        uuid id PK
        uuid applicationId FK
        string type
        text description
        timestamp occurredAt
    }
    
    EMAIL_LOG {
        uuid id PK
        uuid userId FK
        uuid applicationId FK
        string recipientEmail
        string subject
        text body
        string status
        timestamp sentAt
    }
    
    SUBSCRIPTION {
        uuid id PK
        uuid userId FK
        uuid planId FK
        string status
        decimal price
        string currency
        date startDate
        date endDate
        date nextBillingDate
        boolean autoRenew
        string stripeSubscriptionId
    }
    
    SUBSCRIPTION_PLAN {
        uuid id PK
        string name UK
        string code UK
        decimal monthlyPrice
        decimal yearlyPrice
        json features
        json limits
        boolean isActive
    }
    
    PAYMENT {
        uuid id PK
        uuid userId FK
        uuid subscriptionId FK
        decimal amount
        string currency
        string method
        string status
        string transactionId
        timestamp paidAt
    }
    
    NETWORKING_CAMPAIGN {
        uuid id PK
        uuid userId FK
        string name
        string targetRole
        string targetCity
        string targetIndustry
        text connectionMessage
        text followUpMessage
        int maxConnections
        int connectionsSent
        int connectionsAccepted
        string status
        timestamp startedAt
    }
    
    CONNECTION {
        uuid id PK
        uuid campaignId FK
        string name
        string title
        string company
        string profileUrl
        string status
        timestamp requestedAt
        timestamp acceptedAt
        int followUpCount
    }
    
    API_CLIENT {
        uuid id PK
        string companyName
        string email
        string apiKey UK
        string apiSecret
        uuid planId FK
        string status
        int requestCount
        int requestLimit
        timestamp createdAt
        timestamp expiresAt
    }
    
    API_REQUEST {
        uuid id PK
        uuid clientId FK
        string endpoint
        string method
        int statusCode
        int responseTime
        timestamp requestedAt
    }
```

---

## 📡 API Documentation

### Authentication Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/auth/register` | Register new user | ❌ |
| POST | `/api/auth/login` | Login user | ❌ |
| POST | `/api/auth/refresh` | Refresh access token | ❌ |
| POST | `/api/auth/logout` | Logout user | ✅ |
| POST | `/api/auth/forgot-password` | Request password reset | ❌ |
| POST | `/api/auth/reset-password` | Reset password | ❌ |
| GET | `/api/auth/verify-email/:token` | Verify email | ❌ |
| GET | `/api/auth/me` | Get current user | ✅ |

### Profile Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/profiles/me` | Get my profile | ✅ |
| PUT | `/api/profiles/me` | Update my profile | ✅ |
| POST | `/api/profiles/import/linkedin` | Import from LinkedIn | ✅ |
| POST | `/api/profiles/import/github` | Import from GitHub | ✅ |
| POST | `/api/profiles/experiences` | Add experience | ✅ |
| PUT | `/api/profiles/experiences/:id` | Update experience | ✅ |
| DELETE | `/api/profiles/experiences/:id` | Delete experience | ✅ |
| POST | `/api/profiles/education` | Add education | ✅ |
| POST | `/api/profiles/skills` | Add skills | ✅ |
| GET | `/api/profiles/completeness` | Get profile completeness | ✅ |

### Document Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/documents/folders` | List folders | ✅ |
| POST | `/api/documents/folders` | Create folder | ✅ |
| POST | `/api/documents/upload` | Upload document | ✅ |
| GET | `/api/documents/:id` | Get document | ✅ |
| DELETE | `/api/documents/:id` | Delete document | ✅ |
| GET | `/api/documents/:id/download` | Download document | ✅ |

### Job Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/jobs` | Search jobs | ✅ |
| GET | `/api/jobs/:id` | Get job details | ✅ |
| POST | `/api/jobs/favorites` | Add to favorites | ✅ |
| DELETE | `/api/jobs/favorites/:id` | Remove from favorites | ✅ |
| GET | `/api/jobs/favorites` | List favorites | ✅ |
| POST | `/api/jobs/saved-searches` | Save search | ✅ |
| GET | `/api/jobs/saved-searches` | List saved searches | ✅ |
| POST | `/api/jobs/match` | Get matching score | ✅ |

### Application Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/applications` | Create application(s) | ✅ |
| GET | `/api/applications` | List my applications | ✅ |
| GET | `/api/applications/:id` | Get application details | ✅ |
| PUT | `/api/applications/:id/status` | Update status | ✅ |
| POST | `/api/applications/campaigns` | Create email campaign | ✅ |
| GET | `/api/applications/campaigns` | List campaigns | ✅ |
| POST | `/api/applications/campaigns/:id/send` | Send campaign | ✅ |

### Networking Endpoints (Premium)

| Method | Endpoint | Description | Auth Required | Permission |
|--------|----------|-------------|---------------|------------|
| POST | `/api/networking/campaigns` | Create campaign | ✅ | premium.networking |
| GET | `/api/networking/campaigns` | List campaigns | ✅ | premium.networking |
| GET | `/api/networking/campaigns/:id` | Get campaign | ✅ | premium.networking |
| POST | `/api/networking/campaigns/:id/start` | Start campaign | ✅ | premium.networking |
| POST | `/api/networking/campaigns/:id/pause` | Pause campaign | ✅ | premium.networking |
| GET | `/api/networking/connections` | List connections | ✅ | premium.networking |

### Payment Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/payments/plans` | List subscription plans | ❌ |
| POST | `/api/payments/subscribe` | Create subscription | ✅ |
| GET | `/api/payments/subscription` | Get my subscription | ✅ |
| POST | `/api/payments/subscription/cancel` | Cancel subscription | ✅ |
| POST | `/api/payments/subscription/upgrade` | Upgrade plan | ✅ |
| GET | `/api/payments/invoices` | List invoices | ✅ |
| GET | `/api/payments/invoices/:id/download` | Download invoice | ✅ |

### B2B API Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/b2b/companies` | List companies | API Key |
| GET | `/api/b2b/companies/:id` | Get company details | API Key |
| GET | `/api/b2b/companies/sector/:sector` | Companies by sector | API Key |
| GET | `/api/b2b/jobs` | List jobs | API Key |
| GET | `/api/b2b/jobs/stats` | Job statistics | API Key |
| GET | `/api/b2b/market/analytics` | Market analytics | API Key |

### Analytics Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/analytics/me` | My statistics | ✅ |
| GET | `/api/analytics/applications` | Application stats | ✅ |
| GET | `/api/analytics/success-rate` | Success rate | ✅ |
| GET | `/api/analytics/trends` | Monthly trends | ✅ |

---

## 🚀 Deployment Guide

### Prerequisites

- Docker & Docker Compose
- Node.js 18+
- PostgreSQL 15+
- Redis 7+
- Kafka 3+
- AWS Account (S3, SES)
- Stripe Account
- OpenAI API Key

### Environment Variables

Create `.env` file in each service:

```bash
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/server25
REDIS_URL=redis://localhost:6379

# JWT
JWT_SECRET=your-super-secret-jwt-key
JWT_EXPIRES_IN=15m
REFRESH_TOKEN_EXPIRES_IN=7d

# Kafka
KAFKA_BROKERS=localhost:9092
KAFKA_CLIENT_ID=server25

# AWS
AWS_REGION=eu-west-1
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_S3_BUCKET=server25-documents
AWS_SES_FROM_EMAIL=noreply@server25.com

# Stripe
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# OpenAI
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-4

# Services URLs
AUTH_SERVICE_URL=http://localhost:4001
PROFILE_SERVICE_URL=http://localhost:4002
JOB_SERVICE_URL=http://localhost:4003
```

### Docker Compose Setup

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:15-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: server25
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  elasticsearch:
    image: elasticsearch:8.11.0
    ports:
      - "9200:9200"
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
    volumes:
      - elastic_data:/usr/share/elasticsearch/data

  kafka:
    image: confluentinc/cp-kafka:7.5.0
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
    depends_on:
      - zookeeper

  zookeeper:
    image: confluentinc/cp-zookeeper:7.5.0
    ports:
      - "2181:2181"
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181

  auth-service:
    build: ./services/auth-service
    ports:
      - "4001:4001"
      - "5001:5001"
    depends_on:
      - postgres
      - redis
      - kafka
    environment:
      - NODE_ENV=production

  profile-service:
    build: ./services/profile-service
    ports:
      - "4002:4002"
      - "5002:5002"
    depends_on:
      - postgres
      - redis
      - kafka

  job-service:
    build: ./services/job-service
    ports:
      - "4003:4003"
      - "5003:5003"
    depends_on:
      - postgres
      - redis
      - elasticsearch
      - kafka

volumes:
  postgres_data:
  redis_data:
  elastic_data:
```

### Running Services

```bash
# Install dependencies
npm install

# Run database migrations
npm run migrate

# Seed initial data
npm run seed

# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down
```

### Production Deployment (AWS)

```bash
# Build Docker images
docker build -t server25/auth-service:latest ./services/auth-service
docker build -t server25/profile-service:latest ./services/profile-service

# Push to ECR
aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.eu-west-1.amazonaws.com
docker tag server25/auth-service:latest <account-id>.dkr.ecr.eu-west-1.amazonaws.com/server25/auth-service:latest
docker push <account-id>.dkr.ecr.eu-west-1.amazonaws.com/server25/auth-service:latest

# Deploy to ECS/EKS
kubectl apply -f k8s/
```

---

## 📊 Monitoring & Observability

### Health Checks

Each service exposes health check endpoints:

```bash
GET /health
GET /health/ready
GET /health/live
```

### Metrics (Prometheus)

```bash
GET /metrics
```

### Logging (ELK Stack)

- Centralized logging with Elasticsearch
- Log aggregation with Logstash
- Visualization with Kibana

### Tracing (Jaeger)

- Distributed tracing across microservices
- Performance monitoring
- Request flow visualization

---

## 🔒 Security Best Practices

✅ **Authentication**: JWT with short expiration + refresh tokens  
✅ **Authorization**: Dynamic RBAC with granular permissions  
✅ **Encryption**: HTTPS only, bcrypt for passwords  
✅ **Rate Limiting**: Redis-based rate limiting per endpoint  
✅ **Input Validation**: DTO validation with class-validator  
✅ **SQL Injection**: Parameterized queries with Prisma ORM  
✅ **XSS Protection**: Content Security Policy headers  
✅ **CORS**: Configured allowed origins  
✅ **Secrets Management**: AWS Secrets Manager  
✅ **Audit Logging**: All sensitive actions logged  

---

## 📈 Performance Optimization

✅ **Caching**: Redis for frequently accessed data  
✅ **Database Indexing**: Optimized indexes on foreign keys  
✅ **Connection Pooling**: PostgreSQL connection pools  
✅ **Lazy Loading**: Pagination for large datasets  
✅ **CDN**: CloudFront for static assets  
✅ **Compression**: Gzip compression enabled  
✅ **Load Balancing**: Application Load Balancer  
✅ **Horizontal Scaling**: Auto-scaling groups  

---

## 🎯 Roadmap

### Phase 1 - MVP (3 months)
- ✅ Auth Service with RBAC
- ✅ Profile Service with CV management
- ✅ Job Service with basic scraping
- ✅ Application Service with email sending
- ✅ Payment Service with Stripe

### Phase 2 - Premium Features (3 months)
- ✅ AI Service with GPT-4 integration
- ✅ Networking Service automation
- ✅ Advanced matching algorithm
- ✅ Analytics dashboard

### Phase 3 - B2B & Scale (6 months)
- ✅ B2B API Service
- ✅ GraphQL API
- ✅ Mobile app optimization
- ✅ Multi-language support
- ✅ Advanced analytics

### Phase 4 - Enterprise (12 months)
- 🔄 ATS integration
- 🔄 Video interview scheduling
- 🔄 AI-powered interview prep
- 🔄 Freelance marketplace
- 🔄 Company reviews

---

## 📞 Support & Contact

- **Documentation**: https://docs.server25.com
- **API Reference**: https://api.server25.com/docs
- **Status Page**: https://status.server25.com
- **Support Email**: support@server25.com

---

**Built with ❤️ by Server 25 Team**  
**© 2024 Server 25 - All Rights Reserved**
