# SmartHire AI – Module-Wise Knowledge Transfer

**Document Type:** Component and Module Documentation  
**Reference:** SmartHire AI HLD v1 and HLD v2  
**Status:** Draft

---

## 1. Introduction

This document describes the major modules and specialized components of SmartHire AI.

The objective is to understand the responsibilities, boundaries, dependencies and interactions of individual components.

## 2. Core Backend Modules

The Core Backend follows a Modular Monolith architecture.

### 2.1 Authentication and Access

Responsibilities:

- User registration.
- Login and logout.
- Password reset.
- Session and token validation.
- Role and permission checks.

### 2.2 Users

Responsibilities:

- Candidate identity and profile data.
- Recruiter identity and profile data.
- Administrator identity and profile data.

### 2.3 Companies and Teams

Responsibilities:

- Company workspaces.
- Membership management.
- Invitations.
- Workspace roles.
- Access boundaries.

### 2.4 Jobs

Responsibilities:

- Job creation.
- Job editing.
- Job publishing.
- Job closing.
- Job visibility.
- Job requirements.

### 2.5 Applications

Responsibilities:

- Application submission.
- Application status history.
- Candidate-job relationships.
- Recruiter review records.

### 2.6 Interview Management

Responsibilities:

- Interview records.
- Invitations.
- Scheduling metadata.
- Interview lifecycle state.
- Links to live sessions.

### 2.7 Notifications

Responsibilities:

- Notification preferences.
- Notification records.
- Delivery status.
- Notification-related workflows.

### 2.8 Admin and Moderation

Responsibilities:

- User and company administration.
- Job moderation.
- Complaints.
- Fraud cases.
- Platform settings.

### 2.9 Reporting

Responsibilities:

- Recruiter-facing summaries.
- Operational reporting.
- Controlled access to screening reports.
- Controlled access to assessment reports.

## 3. Background Workers

Background workers handle asynchronous processing.

| Worker | Responsibilities |
|---|---|
| Resume Screening Worker | Loads job requirements and permitted resume content, invokes AI and stores structured screening results |
| Interview Coordinator Worker | Manages invitations, confirmations, reminder deadlines and rescheduling |
| Interview Assessment Worker | Retrieves permitted interview evidence and creates assessment reports |
| Notification Worker | Delivers notifications and records delivery outcomes |

Workers may initially run as separate processes using the same repository as the Core Backend.

## 4. Real-Time Interview Service

Responsibilities:

- Create and manage live interview sessions.
- Support live voice interaction with the AI interviewer.
- Coordinate question progression and session timing.
- Connect the coding workspace to the execution gateway.
- Receive proctoring signals.
- Record session outcomes with the Core Backend.
- Emit interview lifecycle events.

## 5. AI Interviewer

Responsibilities:

- Conduct conversational interviews.
- Ask planned questions.
- Interpret candidate answers.
- Generate relevant follow-up questions.
- Follow the configured interview structure.

The AI Interviewer handles conversational behavior.

## 6. Proctoring Policy Engine

Responsibilities:

- Handle page visibility signals.
- Handle camera and microphone state signals.
- Record session interruptions.
- Apply configured policy rules.
- Record technical events and timestamps.

The proctoring policy engine handles deterministic platform policy events. It must remain separate from AI conversational behavior.

## 7. Isolated Code Execution Service

Responsibilities:

- Accept validated code and test input.
- Execute code in disposable restricted environments.
- Enforce CPU, memory and time limits.
- Restrict filesystem and network access.
- Return bounded output, exit status, timing and test results.
- Clean up temporary execution artifacts.

It must not have broad credentials or direct access to the Core Backend database.

## 8. Shared Infrastructure Components

| Component | Purpose |
|---|---|
| Primary Database | Stores transactional business data |
| Private Object Storage | Stores resumes and protected artifacts |
| Message Broker | Transports asynchronous events |
| Session / Cache Storage | Holds ephemeral session data where required |
| Secrets Management | Protects provider credentials and internal secrets |
| Monitoring | Tracks system health and operational failures |

## 9. Module Boundary Guidelines

- Each business module owns its rules.
- Modules should communicate through explicit public interfaces.
- Avoid cross-module direct database writes.
- Avoid shared model imports that bypass module ownership.
- Maintain separate security boundaries for specialized services.

## 10. KT Discussion Checklist

For every module, discuss:

- [ ] Purpose and responsibility.
- [ ] Main operations.
- [ ] Inputs and outputs.
- [ ] Data ownership.
- [ ] Dependencies.
- [ ] Communication patterns.
- [ ] Error handling.
- [ ] Security requirements.
- [ ] Integration with other components.

**End of Document**