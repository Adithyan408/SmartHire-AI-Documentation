# SmartHire AI – Architecture Knowledge Transfer

**Document Type:** Technical Architecture Documentation  
**Reference:** SmartHire AI HLD v1 and HLD v2  
**Status:** Draft

---

## 1. Architecture Overview

The agreed working architecture consists of:

1. Modular Monolith for the Core Backend.
2. Selective Event-Driven Processing.
3. Isolated Code Execution Service.
4. Separate Real-Time Interview Service.
5. Pragmatic Clean Architecture for internal module organization.

The architecture separates ordinary recruitment operations, asynchronous processing, live interview communication and untrusted code execution.

## 2. Main Architectural Components

| Component | Responsibility |
|---|---|
| Client Applications | Candidate, Recruiter and Admin interfaces |
| Core Backend | Recruitment business logic and persistent business state |
| Event Broker | Asynchronous event communication |
| Background Workers | Screening, coordination, assessment and notifications |
| Real-Time Interview Service | Live interview sessions and session coordination |
| AI Interviewer | Conversational interview and follow-up questions |
| Proctoring Policy Engine | Deterministic handling of interview policy events |
| Code Execution Service | Restricted execution of candidate-submitted code |
| Primary Database | Transactional business data |
| Private Object Storage | Resumes and approved protected artifacts |
| Session / Cache Storage | Ephemeral session or coordination data where required |

## 3. Modular Monolith

The Core Backend follows a Modular Monolith architecture.

It is initially one deployable business application, internally divided into modules with clearly defined responsibilities.

### Architecture Principles

- Each module owns its business rules.
- Modules communicate through explicit interfaces.
- Modules should not access another module's private implementation.
- Cross-module direct database writes should be avoided.
- The architecture should avoid premature microservice decomposition.

### Why Modular Monolith?

- Keeps related recruitment workflows cohesive.
- Reduces unnecessary distributed-system complexity.
- Simplifies initial development and deployment.
- Maintains clear internal module boundaries.

## 4. Pragmatic Clean Architecture

Pragmatic Clean Architecture guides the internal organization of complex modules.

The objective is to separate business logic from infrastructure and framework-specific implementation details where that separation provides value.

It should not introduce unnecessary layers into simple CRUD features.

## 5. Event-Driven Processing

Event-driven processing is used for asynchronous tasks that do not need to block the original API request.

Examples:

- Resume screening.
- Interview coordination.
- Assessment generation.
- Reminder scheduling.
- Notification delivery.

### Event Processing Components

- Event Publisher.
- Message Broker.
- Event Consumer.
- Background Worker.
- Retry Mechanism.
- Failed-Event Handling.

## 6. Real-Time Interview Service

The Real-Time Interview Service is deployed separately to handle:

- Live interview sessions.
- Live voice interaction.
- AI interviewer communication.
- Question progression.
- Session timing.
- Collaborative coding coordination.
- Proctoring signals.
- Interview lifecycle events.

Real-time communication must remain separate from ordinary background event transport.

## 7. Isolated Code Execution Service

The Code Execution Service provides a restricted environment for executing candidate-submitted code.

Security requirements:

- Disposable execution environments.
- CPU, memory and time limits.
- Restricted filesystem access.
- Restricted network access.
- No access to platform secrets.
- No direct access to the core application database.
- Bounded execution output.

Folder separation alone does not provide security isolation. The execution service requires an actual infrastructure security boundary.

## 8. Communication Patterns

| Communication | Use Case |
|---|---|
| HTTPS / REST | Profiles, jobs, applications and recruiter actions |
| Message Broker | Screening, reminders, coordination and assessment |
| Real-Time Connection | Live interview interaction and session updates |
| Isolated Execution Request | Candidate code and test input submission |

### Important Communication Rules

- REST is used when the caller needs an immediate response.
- Events are used when processing can happen later.
- Real-time transport is used for live interaction.
- The message broker must not carry live audio/video streams.
- Candidate code must not execute inside the core backend process.

## 9. Event Reliability

The architecture requires:

- Reliable event publishing, such as the Transactional Outbox Pattern.
- Idempotent event consumers.
- Bounded retries with backoff.
- Dead-letter or failed-event handling.
- Event and correlation identifiers.
- Monitoring for failed messages.
- Entity state checks where event ordering matters.

## 10. Architecture Principles

1. Keep business logic independent from infrastructure where appropriate.
2. Maintain clear module ownership.
3. Use synchronous communication for immediate operations.
4. Use asynchronous processing for long-running tasks.
5. Use real-time communication for live sessions.
6. Maintain isolated execution boundaries.
7. Extract modules into independent services only when measurable requirements justify it.

## 11. Initial Deployment Direction

- Core backend as one modular application.
- Background workers as separate processes where needed.
- Real-Time Interview Service deployed separately.
- Code Execution Service deployed on isolated infrastructure.
- One primary transactional database initially.
- Private object storage for protected files.

## 12. Architecture Reference

| Topic | HLD v1 Pages |
|---|---|
| Architecture decisions | 3–4 |
| System architecture diagram | 5 |
| Component responsibilities | 5–7 |
| Event processing | 6 and 11 |
| Communication patterns | 5 and 12 |
| Deployment | 14 |

**End of Document**