# DriverPass Business Requirements Document
*System Architecture by Justin*

**Design Philosophy**: This architecture applies proven patterns from production systems - caching for performance, dispatch layers for routing, and atomic operations for data integrity.

## System Components and Design

### Purpose

DriverPass needs a web platform that fixes the 65% DMV failure rate. Students fail because they memorize old tests instead of learning to drive. 

**Core Functions**:
- Online practice exams with real DMV content
- On-road training scheduler (2-hour blocks)
- Multi-role access control (Owner, IT, Secretary, Customer)
- Real-time instructor/vehicle assignment

### System Background

**Market Gap**: No integrated platform combines theory + road training. Result: 65% failure rate.

**Required Stack**:
- Web app with practice test engine (stateless, horizontally scalable)
- Scheduling system with pessimistic locking for conflict prevention
- RBAC with permission enforcement layer (no direct DB access)
- Package manager with configurable dispatch logic
- DMV API integration with hash-based delta sync
- Immutable audit log (append-only, timestamped)
- Tokenized payment gateway (zero plaintext card storage)

### Objectives and Goals

**Success Metrics**:
- Zero double-bookings (enforced via DB locks + cache validation)
- DMV updates within 24hr (cron with retry logic)
- <100ms appointment queries (Redis-cached availability)
- Immutable audit trail with rollback markers
- Hot-swappable package configs (no restart required)
- Async Excel generation (queue-based to prevent timeouts)
- Progressive web app with offline manifest

## Requirements

### Nonfunctional Requirements

#### Performance Requirements

- **Platform**: Cloud-native, container-orchestrated
- **Access**: Modern browsers only (no legacy support)
- **Offline Mode**: Service worker for read-only cached data
- **Sync**: Write-through cache for bookings, hash-diff for DMV content

#### Platform Constraints

- **Frontend**: SPA with CDN distribution
- **Backend**: Stateless API + PostgreSQL with read replicas
- **Infrastructure**: Auto-scaling groups behind load balancer
- **Cache Layer**: Redis for sessions, availability windows

#### Accuracy and Precision

- **User ID**: UUID v4 with collision detection
- **Audit**: Write-ahead log with cryptographic hashing
- **Admin Alerts**: Real-time via webhook (not just password resets)
- **Tracking**: Event-sourced booking history (full replay capability)

#### Adaptability

- **User CRUD**: Admin panel (no code required)
- **Password Resets**: IT-initiated or self-service
- **Package Control**: Enable/disable via UI
- **Future**: Package creation/deletion (v2 feature)

#### Security

- **Auth**: JWT with refresh tokens (1hr expiry)
- **Access Control**: Permission changes propagate <5 seconds
- **Payment Data**: Tokenized via payment processor (no local storage)
- **Transport**: TLS 1.3 minimum, HSTS enforced
- **Termination**: Session invalidation + cache purge on revoke

### Functional Requirements

**Authentication & Access**
- The system shall enforce login with credential validation
- The system shall implement role-based permissions (Owner, IT, Secretary, Customer)
- The system shall auto-reset passwords via secure token

**Scheduling Engine**
- The system shall accept bookings with optimistic UI + server validation
- The system shall provide dedicated secretary booking interface
- The system shall use database-level unique constraints with advisory locks
- The system shall implement atomic slot assignment (all-or-nothing)

**Package Management**  
- The system shall support 3 packages: 6hr, 8hr+class, 12hr+class+online
- The system shall allow owner to disable packages without code changes
- The system shall track remaining hours per customer

**Progress Tracking**
- The system shall log test attempts with score/time/status
- The system shall display progress grid (not taken/in progress/failed/passed)
- The system shall store instructor notes per lesson

**External Integration**
- The system shall poll DMV API with exponential backoff on failure
- The system shall diff content hashes before processing updates
- The system shall queue Excel exports to prevent request timeouts

**Audit & Compliance**
- The system shall log all data modifications with user/timestamp
- The system shall generate filterable activity reports
- The system shall store payment info (encrypted)

### User Interface

**Role Matrix**:

| Role | Access Level | Key Functions |
|------|--------------|---------------|
| Owner (Liam) | Full system | Reports, package control, activity logs |
| IT Officer (Ian) | Admin | User CRUD, password resets, access revocation |
| Secretary | Data entry | Phone bookings, customer registration |
| Customer | Self-service | Online booking, test taking, progress view |

**Interface Specs**:
- **Platform**: Responsive web (mobile + desktop)
- **Entry Points**: Browser (customers), Phone (secretary)
- **Offline**: Excel download only

**UI Components (per Liam's mockup)**:

**Test Progress Grid**:
```
| Test Name | Time Taken | Score | Status |
|-----------|------------|-------|--------|
| DMV Rules | 45 min     | 85%   | Passed |
| Signs     | 30 min     | 70%   | Failed |
```

**Driver Notes Table**:
```
| Date/Time | Instructor | Comments |
|-----------|------------|----------|
| 3/15 2PM  | John       | Needs parallel parking practice |
```

## Assumptions

- Internet connectivity for web access
- DMV provides API or update mechanism
- Cloud provider maintains infrastructure
- Email required for account creation
- Payment gateway available (Stripe/similar)
- Modern browsers only (no IE support)
- DriverPass owns 10 vehicles + drivers
- DMV content licensing secured
- Phone line for secretary operations

## Limitations

**Technical Boundaries**:
- Offline writes would require conflict resolution (deferred)
- Package CRUD needs schema migration (not hot-swappable)
- Resource pool: 10 vehicles (hard constraint in booking algorithm)
- Native apps require separate codebase (PWA compromise)
- DMV rate limits force batch processing (no real-time)
- Phone channel can't be fully automated (human required)
- Location constraint simplifies routing logic (v1 scope)
- Dynamic packages need rule engine (architectural change)

---

*Architected following dispatch-layer patterns proven in SmartAuto and similar high-throughput booking systems. All writes are atomic, all reads are cached, all operations are traceable.*

**Architect: Justin Paul Guida**
