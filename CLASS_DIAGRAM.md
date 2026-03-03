# 🏗️ SERVER 25 - CLASS DIAGRAM
## Architecture Orientée Microservices avec Relations Dynamiques

```mermaid
classDiagram
    %% ============================================
    %% AUTH & USER MANAGEMENT DOMAIN
    %% ============================================
    
    class User {
        <<Entity>>
        +UUID id
        +String email
        +String password_hash
        +String firstName
        +String lastName
        +String phone
        +DateTime createdAt
        +DateTime updatedAt
        +DateTime lastLogin
        +Boolean isVerified
        +Boolean isActive
        +String verificationToken
        +String resetPasswordToken
        +DateTime tokenExpiry
        ---
        +register() User
        +login() AuthToken
        +verifyEmail() Boolean
        +resetPassword() Boolean
        +updateProfile() User
        +deactivate() Boolean
    }

    class Role {
        <<Entity>>
        +UUID id
        +String name
        +String code
        +String description
        +Integer priority
        +Boolean isSystem
        +DateTime createdAt
        ---
        +assignPermissions(Permission[]) void
        +removePermissions(Permission[]) void
        +hasPermission(String) Boolean
        +getUsers() User[]
    }

    class Permission {
        <<Entity>>
        +UUID id
        +String name
        +String code
        +String resource
        +String action
        +String description
        +PermissionType type
        +JSON metadata
        ---
        +validate() Boolean
        +isGrantedTo(Role) Boolean
    }

    class UserRole {
        <<Association>>
        +UUID userId
        +UUID roleId
        +DateTime assignedAt
        +UUID assignedBy
        +DateTime expiresAt
        +Boolean isActive
        ---
        +activate() void
        +deactivate() void
        +extend(DateTime) void
    }

    class RolePermission {
        <<Association>>
        +UUID roleId
        +UUID permissionId
        +Boolean canDelegate
        +JSON constraints
        +DateTime grantedAt
        +UUID grantedBy
        ---
        +revoke() void
        +updateConstraints(JSON) void
    }

    class AuthToken {
        <<ValueObject>>
        +String accessToken
        +String refreshToken
        +DateTime expiresAt
        +String tokenType
        ---
        +isExpired() Boolean
        +refresh() AuthToken
        +revoke() void
    }

    %% ============================================
    %% PROFILE & DOCUMENTS DOMAIN
    %% ============================================

    class Profile {
        <<Entity>>
        +UUID id
        +UUID userId
        +String title
        +String summary
        +String location
        +String country
        +String city
        +Decimal desiredSalary
        +String currency
        +ContractType[] contractTypes
        +Boolean remoteOnly
        +String linkedinUrl
        +String githubUrl
        +JSON preferences
        +DateTime updatedAt
        ---
        +importFromLinkedIn() void
        +importFromGitHub() void
        +calculateCompleteness() Integer
        +generateCV() Document
    }

    class Experience {
        <<Entity>>
        +UUID id
        +UUID profileId
        +String company
        +String position
        +String description
        +Date startDate
        +Date endDate
        +Boolean isCurrent
        +String location
        +String[] technologies
        ---
        +getDuration() Integer
        +validate() Boolean
    }

    class Education {
        <<Entity>>
        +UUID id
        +UUID profileId
        +String institution
        +String degree
        +String field
        +Date startDate
        +Date endDate
        +String description
        ---
        +validate() Boolean
    }

    class Skill {
        <<Entity>>
        +UUID id
        +String name
        +String category
        +SkillLevel level
        +Integer yearsOfExperience
        ---
        +normalize() String
    }

    class ProfileSkill {
        <<Association>>
        +UUID profileId
        +UUID skillId
        +SkillLevel level
        +Integer yearsOfExperience
        +Boolean isVerified
    }

    class DocumentFolder {
        <<Entity>>
        +UUID id
        +UUID userId
        +String name
        +String speciality
        +String path
        +Integer documentCount
        +DateTime createdAt
        ---
        +addDocument(Document) void
        +removeDocument(UUID) void
        +getDocuments() Document[]
    }

    class Document {
        <<Entity>>
        +UUID id
        +UUID folderId
        +UUID userId
        +String name
        +String originalName
        +DocumentType type
        +String mimeType
        +String s3Key
        +String s3Url
        +Long fileSize
        +DateTime uploadedAt
        ---
        +download() Stream
        +delete() Boolean
        +getMetadata() JSON
    }

    %% ============================================
    %% JOB SCRAPING & SEARCH DOMAIN
    %% ============================================

    class JobOffer {
        <<Entity>>
        +UUID id
        +String title
        +String description
        +String company
        +String location
        +String country
        +String city
        +Decimal salaryMin
        +Decimal salaryMax
        +String currency
        +ContractType contractType
        +Boolean isRemote
        +String sourceUrl
        +String sourcePlatform
        +String contactEmail
        +String[] requiredSkills
        +DateTime publishedAt
        +DateTime scrapedAt
        +DateTime expiresAt
        +JobStatus status
        +String hash
        ---
        +isDuplicate() Boolean
        +calculateMatchScore(Profile) Integer
        +isExpired() Boolean
        +normalize() void
    }

    class Company {
        <<Entity>>
        +UUID id
        +String name
        +String industry
        +String description
        +String website
        +String location
        +String country
        +String city
        +Integer employeeCount
        +String logoUrl
        +JSON socialLinks
        ---
        +getActiveJobs() JobOffer[]
        +getEmployees() Employee[]
    }

    class ScrapingJob {
        <<Entity>>
        +UUID id
        +String platform
        +String searchQuery
        +JSON filters
        +ScrapingStatus status
        +Integer jobsFound
        +Integer jobsAdded
        +DateTime startedAt
        +DateTime completedAt
        +String errorMessage
        ---
        +execute() void
        +retry() void
        +cancel() void
    }

    class SearchFilter {
        <<ValueObject>>
        +String keyword
        +String[] countries
        +String[] cities
        +Decimal salaryMin
        +Decimal salaryMax
        +Boolean remoteOnly
        +ContractType[] contractTypes
        +String[] companies
        +Date publishedAfter
        ---
        +toQuery() String
        +validate() Boolean
    }

    class SavedSearch {
        <<Entity>>
        +UUID id
        +UUID userId
        +String name
        +SearchFilter filters
        +Boolean alertEnabled
        +String alertFrequency
        +DateTime createdAt
        ---
        +execute() JobOffer[]
        +enableAlert() void
    }

    class Favorite {
        <<Association>>
        +UUID userId
        +UUID jobOfferId
        +DateTime savedAt
        +String notes
    }

    %% ============================================
    %% APPLICATION & EMAIL DOMAIN
    %% ============================================

    class Application {
        <<Entity>>
        +UUID id
        +UUID userId
        +UUID jobOfferId
        +ApplicationStatus status
        +String coverLetter
        +UUID[] attachmentIds
        +DateTime appliedAt
        +DateTime lastUpdated
        +String notes
        ---
        +updateStatus(ApplicationStatus) void
        +addNote(String) void
        +getTimeline() ApplicationEvent[]
    }

    class ApplicationEvent {
        <<Entity>>
        +UUID id
        +UUID applicationId
        +EventType type
        +String description
        +DateTime occurredAt
        +JSON metadata
    }

    class EmailCampaign {
        <<Entity>>
        +UUID id
        +UUID userId
        +String name
        +String subject
        +String body
        +UUID[] attachmentIds
        +CampaignStatus status
        +Integer totalRecipients
        +Integer sentCount
        +Integer failedCount
        +DateTime scheduledAt
        +DateTime completedAt
        ---
        +send() void
        +pause() void
        +resume() void
        +cancel() void
    }

    class EmailLog {
        <<Entity>>
        +UUID id
        +UUID userId
        +UUID applicationId
        +String recipientEmail
        +String subject
        +String body
        +UUID[] attachmentIds
        +EmailStatus status
        +DateTime sentAt
        +String errorMessage
        +JSON metadata
        ---
        +retry() void
        +markAsRead() void
    }

    %% ============================================
    %% NETWORKING PREMIUM DOMAIN
    %% ============================================

    class NetworkingCampaign {
        <<Entity>>
        +UUID id
        +UUID userId
        +String name
        +String targetRole
        +String targetCity
        +String targetIndustry
        +String connectionMessage
        +String followUpMessage
        +Integer maxConnections
        +Integer connectionsSent
        +Integer connectionsAccepted
        +CampaignStatus status
        +DateTime startedAt
        ---
        +start() void
        +pause() void
        +getMetrics() JSON
    }

    class Connection {
        <<Entity>>
        +UUID id
        +UUID campaignId
        +String name
        +String title
        +String company
        +String profileUrl
        +ConnectionStatus status
        +DateTime requestedAt
        +DateTime acceptedAt
        +DateTime lastContactAt
        +Integer followUpCount
        ---
        +sendFollowUp() void
        +markAsResponded() void
    }

    %% ============================================
    %% AI & ANALYTICS DOMAIN
    %% ============================================

    class AIAnalysis {
        <<Entity>>
        +UUID id
        +UUID userId
        +UUID profileId
        +AnalysisType type
        +JSON input
        +JSON output
        +Integer tokensUsed
        +Decimal cost
        +DateTime analyzedAt
        ---
        +getRecommendations() String[]
        +getScore() Integer
    }

    class MatchingScore {
        <<ValueObject>>
        +UUID profileId
        +UUID jobOfferId
        +Integer overallScore
        +Integer skillsMatch
        +Integer experienceMatch
        +Integer locationMatch
        +Integer salaryMatch
        +String[] missingSkills
        +String[] recommendations
        ---
        +calculate() Integer
        +explain() String
    }

    class UserStatistics {
        <<Entity>>
        +UUID id
        +UUID userId
        +Integer applicationsCount
        +Integer responsesCount
        +Integer interviewsCount
        +Decimal responseRate
        +Decimal interviewRate
        +JSON monthlyTrend
        +JSON sectorPerformance
        +DateTime lastCalculated
        ---
        +refresh() void
        +compare(UserStatistics) JSON
    }

    %% ============================================
    %% SUBSCRIPTION & PAYMENT DOMAIN
    %% ============================================

    class Subscription {
        <<Entity>>
        +UUID id
        +UUID userId
        +SubscriptionPlan plan
        +SubscriptionStatus status
        +Decimal price
        +String currency
        +BillingCycle cycle
        +DateTime startDate
        +DateTime endDate
        +DateTime nextBillingDate
        +Boolean autoRenew
        +String stripeSubscriptionId
        ---
        +upgrade(SubscriptionPlan) void
        +downgrade(SubscriptionPlan) void
        +cancel() void
        +renew() void
        +isActive() Boolean
    }

    class Payment {
        <<Entity>>
        +UUID id
        +UUID userId
        +UUID subscriptionId
        +Decimal amount
        +String currency
        +PaymentMethod method
        +PaymentStatus status
        +String transactionId
        +String invoiceUrl
        +DateTime paidAt
        +JSON metadata
        ---
        +refund() Boolean
        +downloadInvoice() Stream
    }

    class SubscriptionPlan {
        <<Entity>>
        +UUID id
        +String name
        +String code
        +String description
        +Decimal monthlyPrice
        +Decimal yearlyPrice
        +JSON features
        +JSON limits
        +Boolean isActive
        ---
        +hasFeature(String) Boolean
        +getLimit(String) Integer
    }

    %% ============================================
    %% B2B API DOMAIN
    %% ============================================

    class APIClient {
        <<Entity>>
        +UUID id
        +String companyName
        +String email
        +String apiKey
        +String apiSecret
        +UUID planId
        +APIStatus status
        +Integer requestCount
        +Integer requestLimit
        +DateTime createdAt
        +DateTime expiresAt
        ---
        +generateApiKey() String
        +rotateApiKey() String
        +incrementUsage() void
        +hasReachedLimit() Boolean
    }

    class APIRequest {
        <<Entity>>
        +UUID id
        +UUID clientId
        +String endpoint
        +String method
        +JSON requestBody
        +JSON responseBody
        +Integer statusCode
        +Integer responseTime
        +DateTime requestedAt
        ---
        +log() void
    }

    class APIEndpoint {
        <<Entity>>
        +UUID id
        +String path
        +String method
        +String description
        +JSON requiredParams
        +Integer rateLimit
        +Boolean requiresAuth
        ---
        +validate(JSON) Boolean
    }

    %% ============================================
    %% ADMIN DOMAIN
    %% ============================================

    class AdminDashboard {
        <<Service>>
        +getTotalUsers() Integer
        +getTotalJobs() Integer
        +getTotalApplications() Integer
        +getRevenue() Decimal
        +getActiveSubscriptions() Integer
        +getSystemHealth() JSON
        ---
        +generateReport(DateRange) Report
    }

    class AuditLog {
        <<Entity>>
        +UUID id
        +UUID userId
        +String action
        +String resource
        +JSON oldValue
        +JSON newValue
        +String ipAddress
        +String userAgent
        +DateTime timestamp
        ---
        +search(JSON) AuditLog[]
    }

    %% ============================================
    %% ENUMERATIONS
    %% ============================================

    class PermissionType {
        <<enumeration>>
        READ
        WRITE
        DELETE
        EXECUTE
        ADMIN
    }

    class ContractType {
        <<enumeration>>
        CDI
        CDD
        FREELANCE
        INTERNSHIP
        ALTERNANCE
    }

    class SkillLevel {
        <<enumeration>>
        BEGINNER
        INTERMEDIATE
        ADVANCED
        EXPERT
    }

    class DocumentType {
        <<enumeration>>
        CV
        COVER_LETTER
        CERTIFICATE
        PORTFOLIO
        OTHER
    }

    class JobStatus {
        <<enumeration>>
        ACTIVE
        EXPIRED
        FILLED
        REMOVED
    }

    class ApplicationStatus {
        <<enumeration>>
        PENDING
        SENT
        VIEWED
        INTERVIEW
        REJECTED
        ACCEPTED
    }

    class EmailStatus {
        <<enumeration>>
        PENDING
        SENT
        DELIVERED
        FAILED
        BOUNCED
    }

    class CampaignStatus {
        <<enumeration>>
        DRAFT
        SCHEDULED
        RUNNING
        PAUSED
        COMPLETED
        CANCELLED
    }

    class ConnectionStatus {
        <<enumeration>>
        PENDING
        ACCEPTED
        REJECTED
        RESPONDED
    }

    class SubscriptionStatus {
        <<enumeration>>
        ACTIVE
        CANCELLED
        EXPIRED
        SUSPENDED
    }

    class PaymentStatus {
        <<enumeration>>
        PENDING
        COMPLETED
        FAILED
        REFUNDED
    }

    class APIStatus {
        <<enumeration>>
        ACTIVE
        SUSPENDED
        EXPIRED
    }

    %% ============================================
    %% RELATIONSHIPS - AUTH & USER
    %% ============================================

    User "1" --o "*" UserRole : has
    Role "1" --o "*" UserRole : assigned to
    Role "1" --o "*" RolePermission : has
    Permission "1" --o "*" RolePermission : granted to
    User "1" --> "1" AuthToken : authenticates with
    User "1" --> "0..1" Profile : owns

    %% ============================================
    %% RELATIONSHIPS - PROFILE & DOCUMENTS
    %% ============================================

    Profile "1" --o "*" Experience : has
    Profile "1" --o "*" Education : has
    Profile "1" --o "*" ProfileSkill : has
    Skill "1" --o "*" ProfileSkill : used in
    User "1" --o "*" DocumentFolder : organizes
    DocumentFolder "1" --o "*" Document : contains

    %% ============================================
    %% RELATIONSHIPS - JOBS & SEARCH
    %% ============================================

    Company "1" --o "*" JobOffer : posts
    User "1" --o "*" SavedSearch : creates
    User "1" --o "*" Favorite : saves
    JobOffer "1" --o "*" Favorite : favorited by
    ScrapingJob "1" --> "*" JobOffer : discovers

    %% ============================================
    %% RELATIONSHIPS - APPLICATIONS
    %% ============================================

    User "1" --o "*" Application : submits
    JobOffer "1" --o "*" Application : receives
    Application "1" --o "*" ApplicationEvent : tracks
    Application "1" --> "*" Document : attaches
    User "1" --o "*" EmailCampaign : creates
    EmailCampaign "1" --o "*" EmailLog : sends
    Application "1" --> "0..1" EmailLog : sent via

    %% ============================================
    %% RELATIONSHIPS - NETWORKING
    %% ============================================

    User "1" --o "*" NetworkingCampaign : runs
    NetworkingCampaign "1" --o "*" Connection : manages

    %% ============================================
    %% RELATIONSHIPS - AI & ANALYTICS
    %% ============================================

    User "1" --o "*" AIAnalysis : requests
    Profile "1" --> "*" AIAnalysis : analyzed by
    Profile "1" --> "*" MatchingScore : scored for
    JobOffer "1" --> "*" MatchingScore : evaluated with
    User "1" --> "1" UserStatistics : has

    %% ============================================
    %% RELATIONSHIPS - SUBSCRIPTION & PAYMENT
    %% ============================================

    User "1" --o "*" Subscription : subscribes to
    SubscriptionPlan "1" --o "*" Subscription : defines
    Subscription "1" --o "*" Payment : generates
    User "1" --o "*" Payment : makes

    %% ============================================
    %% RELATIONSHIPS - B2B API
    %% ============================================

    APIClient "1" --> "1" SubscriptionPlan : uses
    APIClient "1" --o "*" APIRequest : makes
    APIEndpoint "1" --o "*" APIRequest : handles

    %% ============================================
    %% RELATIONSHIPS - ADMIN
    %% ============================================

    User "1" --o "*" AuditLog : generates
    AdminDashboard ..> User : monitors
    AdminDashboard ..> JobOffer : monitors
    AdminDashboard ..> Application : monitors
    AdminDashboard ..> Subscription : monitors

    %% ============================================
    %% DYNAMIC ROLE-PERMISSION RELATIONSHIPS
    %% ============================================

    note for RolePermission "🔐 DYNAMIC PERMISSIONS\n- Constraints: time-based, resource-based\n- Delegation: can be delegated to sub-roles\n- Audit: all changes logged\n- Revocation: instant effect"
    
    note for UserRole "👤 DYNAMIC ROLE ASSIGNMENT\n- Temporal: can expire\n- Multiple: user can have multiple roles\n- Hierarchical: roles inherit permissions\n- Contextual: role active in specific contexts"

    note for Permission "⚡ PERMISSION TYPES\n- Resource-based: user.read, job.write\n- Action-based: execute, admin\n- Granular: field-level permissions\n- Dynamic: evaluated at runtime"
```

---

## 🎯 Architecture Patterns Utilisés

### 1. **Domain-Driven Design (DDD)**
- Séparation claire des domaines métier
- Entities, Value Objects, Aggregates
- Bounded Contexts isolés

### 2. **CQRS (Command Query Responsibility Segregation)**
- Séparation lecture/écriture
- Optimisation des performances
- Scalabilité horizontale

### 3. **Event Sourcing**
- ApplicationEvent pour traçabilité
- AuditLog pour conformité
- Historique complet des actions

### 4. **Repository Pattern**
- Abstraction de la persistance
- Testabilité accrue
- Changement de DB facilité

### 5. **Strategy Pattern**
- ScrapingJob pour différentes plateformes
- EmailCampaign pour différents providers
- AIAnalysis pour différents modèles

---

## 🔐 Système de Permissions Dynamique

### Architecture RBAC Avancée

```
User ←→ UserRole ←→ Role ←→ RolePermission ←→ Permission
  ↓         ↓         ↓           ↓              ↓
Context  Temporal  Priority  Constraints    Granular
```

### Exemples de Permissions

| Resource | Action | Code | Description |
|----------|--------|------|-------------|
| user | read | user.read | Lire profil utilisateur |
| user | write | user.write | Modifier profil |
| job | read | job.read | Consulter offres |
| job | apply | job.apply | Postuler |
| admin | execute | admin.dashboard | Accès dashboard admin |
| api | execute | api.b2b.access | Accès API B2B |
| premium | execute | premium.networking | Networking automatique |

### Contraintes Dynamiques

```json
{
  "timeConstraint": {
    "validFrom": "2024-01-01T00:00:00Z",
    "validUntil": "2024-12-31T23:59:59Z"
  },
  "resourceConstraint": {
    "ownedResourcesOnly": true,
    "maxResourcesPerDay": 100
  },
  "contextConstraint": {
    "allowedIPs": ["192.168.1.0/24"],
    "allowedCountries": ["MA", "FR"]
  }
}
```

---

## 📊 Cardinalités et Relations Clés

| Relation | Type | Cardinalité | Description |
|----------|------|-------------|-------------|
| User ↔ Role | Many-to-Many | * ↔ * | Un user peut avoir plusieurs rôles |
| Role ↔ Permission | Many-to-Many | * ↔ * | Un rôle peut avoir plusieurs permissions |
| User ↔ Profile | One-to-One | 1 ↔ 0..1 | Un user a un profil optionnel |
| Profile ↔ Experience | One-to-Many | 1 ↔ * | Un profil a plusieurs expériences |
| User ↔ Application | One-to-Many | 1 ↔ * | Un user fait plusieurs candidatures |
| JobOffer ↔ Application | One-to-Many | 1 ↔ * | Une offre reçoit plusieurs candidatures |
| User ↔ Subscription | One-to-Many | 1 ↔ * | Un user peut avoir plusieurs abonnements |

---

## 🚀 Points Forts de cette Architecture

✅ **Scalabilité**: Architecture microservices-ready  
✅ **Sécurité**: RBAC dynamique avec contraintes  
✅ **Traçabilité**: AuditLog + ApplicationEvent  
✅ **Flexibilité**: Permissions granulaires et temporelles  
✅ **Performance**: Séparation lecture/écriture (CQRS)  
✅ **Maintenabilité**: DDD avec bounded contexts  
✅ **Extensibilité**: Strategy pattern pour nouveaux providers  
✅ **Testabilité**: Repository pattern + injection de dépendances  

---

## 🎨 Légende des Stéréotypes

- `<<Entity>>`: Entité avec identité unique et cycle de vie
- `<<ValueObject>>`: Objet valeur immutable sans identité
- `<<Association>>`: Table de liaison many-to-many
- `<<Service>>`: Service métier sans état
- `<<enumeration>>`: Énumération de valeurs fixes

---

**Créé avec ❤️ pour Server 25 - Architecture de classe professionnelle niveau Enterprise**
