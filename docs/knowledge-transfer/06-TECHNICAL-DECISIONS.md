# SmartHire AI – Technical Decisions and Architecture Rationale

**Document Type:** Technical Decision Record  
**Reference:** SmartHire AI HLD v1 and HLD v2  
**Status:** Draft

---

## 1. Purpose

This document records the architectural positions, technical decisions, design rationale and unresolved technology choices for SmartHire AI.

It helps the team understand why particular architectural approaches are being considered and prevents repeated discussions about previously reviewed decisions.

## 2. Architecture Baseline

| Area | Working Position | Reason |
|---|---|---|
| Overall Structure | Modular Monolith | Keeps related recruitment workflows cohesive and avoids premature distributed-system complexity |
| Internal Module Design | Pragmatic Clean Architecture | Separates business rules from framework and infrastructure details where valuable |
| Background Work | Selective Event-Driven Processing | Allows long-running tasks without holding the original API request open |
| Live Interviews | Separate Real-Time Interview Service | Separates live connections and session workloads from ordinary APIs |
| Candidate Code | Isolated Code Execution Service | Provides a security boundary for untrusted code |
| Data Strategy | One primary transactional database initially | Avoids unnecessary distributed data ownership and synchronization complexity |

## 3. Architecture Principles

### 3.1 Modular Monolith

Keep related business modules within one deployable core application while maintaining clear module boundaries.

### 3.2 Event-Driven Processing

Use events for asynchronous work such as screening, assessment, coordination and notifications.

### 3.3 Real-Time Communication

Use a dedicated real-time communication path for live interviews and interactive session updates.

Do not use the background message broker for live audio/video transport.

### 3.4 Isolated Code Execution

Execute candidate-submitted code in restricted, disposable environments.

Never provide direct access to application databases, secrets or internal services.

### 3.5 Data Ownership

Maintain logical ownership of business data even when modules share one database instance.

Avoid direct writes into another module's data without an agreed interface or ownership rule.

## 4. When Should a Component Become a Separate Service?

A module should be extracted into an independently deployed service only when there is a measurable reason.

Examples:

- Different scaling requirements.
- Security boundaries.
- Independent availability requirements.
- Separate ownership.
- Independent release cadence.

The Code Execution Service has a strong security reason to remain separate. The Real-Time Interview Service has a distinct workload and operational reason.

## 5. Technology Decisions Still Open

| Decision | Why It Matters | Status |
|---|---|---|
| Primary Database | Persistence model and transaction handling | Open |
| Message Broker / Job Orchestration | Retries, scheduling and durable workflows | Open |
| AI Provider and Model Strategy | Evaluation quality, cost and privacy | Open |
| Real-Time Communication | Streaming, latency and reconnection | Open |
| Code Sandbox Technology | Isolation, supported languages and scaling | Open |
| File / Object Storage | Resume privacy, signed access and retention | Open |
| Hosting and Deployment | Network boundaries, scaling and availability | Open |
| Audio/Video Recording Policy | Consent, privacy, storage and deletion | Product decision required |
| Subscription and Billing Scope | Company SaaS billing requirements | Confirm scope |

## 6. Decision Record Template

Use this template whenever a new technical decision is discussed.

### Decision ID: ADR-001

**Title:** TBD

**Date:** TBD

**Status:** Proposed / Accepted / Rejected / Superseded

**Context:**

Describe the technical problem or requirement.

**Decision:**

Describe the selected approach.

**Alternatives Considered:**

1. Alternative A.
2. Alternative B.
3. Alternative C.

**Reasoning:**

Explain why the selected approach is preferred.

**Consequences:**

Describe the benefits, limitations and trade-offs.

**Approved By:** TBD

**Related Documents:** TBD

---

## 7. Decision Maintenance Rules

- Record important decisions with their reasoning.
- Do not mark proposals as approved.
- Assign an owner for every unresolved decision.
- Update decisions when requirements change.
- Preserve previous decisions for historical reference.
- Link decisions to relevant architecture documents.

**End of Document**