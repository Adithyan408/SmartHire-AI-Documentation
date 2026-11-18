# SmartHire AI – Open Questions and Follow-Up Actions

**Document Type:** Technical Questions and Action Tracker  
**Project:** SmartHire AI  
**Reference:** SmartHire AI HLD v1 and HLD v2  
**Status:** Active Documentation

---

## 1. Purpose

This document maintains unresolved technical questions, architectural clarifications, assumptions and follow-up actions identified during project planning and Knowledge Transfer sessions.

It provides a central place to track topics requiring further discussion, evaluation or approval.

## 2. Open Technical Decisions

| ID | Decision | Why It Matters | Owner | Status |
|---|---|---|---|---|
| OQ-01 | Primary Database | Persistence model and transaction handling | TBD | Open |
| OQ-02 | Message Broker / Job Orchestration | Retries, scheduling and durable workflows | TBD | Open |
| OQ-03 | AI Provider and Model Strategy | Evaluation quality, cost and privacy | TBD | Open |
| OQ-04 | Real-Time Communication Technology | Streaming, latency and reconnection | TBD | Open |
| OQ-05 | Code Sandbox Technology | Isolation, language support and scaling | TBD | Open |
| OQ-06 | File / Object Storage | Resume privacy, access and retention | TBD | Open |
| OQ-07 | Hosting and Deployment Model | Network boundaries and availability | TBD | Open |
| OQ-08 | Audio/Video Recording Policy | Consent, privacy and retention | TBD | Product Decision Required |
| OQ-09 | Subscription and Billing Scope | Company SaaS billing requirements | TBD | Confirm Scope |

## 3. Architecture Clarifications

| ID | Question | Reference | Status |
|---|---|---|---|
| AQ-01 | What are the final module boundaries within the Core Backend? | HLD v1, Pages 5–6 | Open |
| AQ-02 | What event publishing and job orchestration approach will be used? | HLD v1, Page 11 | Open |
| AQ-03 | What real-time transport will support live interviews? | HLD v1, Pages 5 and 10 | Open |
| AQ-04 | How will the Code Execution Service communicate with the interview service? | HLD v1, Page 10 | Open |
| AQ-05 | What will be the retention policy for interview artifacts? | HLD v1, Page 13 | Open |
| AQ-06 | What deployment infrastructure will be selected? | HLD v1, Page 14 | Open |

## 4. Policy Clarifications

### 4.1 Proctoring Policy

The detailed HLD v1 specifies that `PAGE_HIDDEN` during an active recruitment interview results in immediate termination and a `DISQUALIFIED` outcome.

The HLD also distinguishes:

- `PAGE_HIDDEN`
- `CONNECTION_LOST`
- `SERVER_ERROR`

**Clarification required:** Confirm that the detailed v1 policy remains the approved product behavior and document the agreed handling of disputed technical cases.

### 4.2 Audio and Video Retention

The HLD does not finalize:

- Whether raw audio/video will be recorded.
- Candidate consent requirements.
- Access permissions.
- Retention duration.
- Deletion procedures.
- Data export behavior.

These decisions must be finalized before implementing the relevant recording and retention features.

## 5. Follow-Up Action Tracker

| Action ID | Action Description | Assigned To | Due Date | Status |
|---|---|---|---|---|
| FA-01 | Review architecture baseline | TBD | TBD | Pending |
| FA-02 | Evaluate primary database options | TBD | TBD | Pending |
| FA-03 | Evaluate message broker options | TBD | TBD | Pending |
| FA-04 | Review AI provider options | TBD | TBD | Pending |
| FA-05 | Evaluate real-time communication options | TBD | TBD | Pending |
| FA-06 | Review sandbox isolation approaches | TBD | TBD | Pending |
| FA-07 | Finalize privacy and retention requirements | TBD | TBD | Pending |
| FA-08 | Review hosting and deployment options | TBD | TBD | Pending |

## 6. Question Recording Template

Use this template when a new question is raised during a KT session.

### Question ID: AQ-XX

**Question:** TBD

**Raised By:** TBD

**KT Session:** TBD

**Date:** TBD

**Related Component:** TBD

**Context:** TBD

**Discussion:** TBD

**Proposed Resolution:** TBD

**Final Decision:** TBD

**Assigned Owner:** TBD

**Status:** Open / Under Review / Resolved

## 7. Status Definitions

| Status | Description |
|---|---|
| Open | No resolution yet |
| Under Review | Discussion or evaluation is in progress |
| Proposed | A possible solution has been identified |
| Approved | Decision has been formally accepted |
| Rejected | Proposed solution was not accepted |
| Resolved | Question has been answered and documented |

## 8. Maintenance Guidelines

- Record questions as soon as they arise.
- Assign an owner to every important question.
- Record the relevant HLD reference.
- Document proposed and approved solutions separately.
- Update resolved questions with the final decision.
- Transfer approved architectural decisions to `06-TECHNICAL-DECISIONS.md`.

**End of Document**