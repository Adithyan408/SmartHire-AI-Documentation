# SmartHire AI – Business Workflow Knowledge Transfer

**Document Type:** Business Workflow Documentation  
**Reference:** SmartHire AI HLD v1 and HLD v2  
**Status:** Draft

---

## 1. Introduction

This document describes the primary business workflows of SmartHire AI.

It explains how different application components interact to complete important recruitment operations.

The four major workflows are:

1. Application Submission and Resume Screening.
2. Interview Coordination.
3. Live AI Interview and Code Execution.
4. Post-Interview Assessment.

## 2. Application Submission and Resume Screening

### 2.1 Workflow

1. Candidate submits a job application.
2. Core Backend validates the application and eligibility.
3. Application is saved in the primary database.
4. Core Backend publishes the `ApplicationSubmitted` event.
5. Resume Screening Worker receives the event.
6. Worker loads job requirements and permitted resume content.
7. AI compares the resume with job requirements.
8. Screening report is generated and saved.
9. Recruiter is notified that the report is ready.

### 2.2 Important Concepts

- Application validation.
- Transactional persistence.
- Event publishing.
- Resume processing.
- AI-assisted screening.
- Report generation.
- Recruiter notification.

### 2.3 Expected Result

The candidate receives application confirmation independently of the asynchronous screening process.

The screening report should identify the job requirements or job version used during evaluation.

## 3. Interview Coordination

### 3.1 Workflow

1. Recruiter selects a candidate.
2. Core Backend persists the coordination request.
3. Interview Coordinator manages available slots.
4. Candidate receives an interview invitation.
5. Candidate confirms, declines or requests rescheduling.
6. Interview record is updated.
7. Reminders are scheduled.
8. Confirmed interview details remain stored in the Core Backend.

### 3.2 Important Concepts

- Interview invitations.
- Slot management.
- Confirmation and rejection.
- Rescheduling.
- Reminder scheduling.
- Persistent workflow state.
- Timeout handling.
- Retry mechanisms.

### 3.3 Reliability Requirement

Coordination state, deadlines and confirmation status must be persistent.

The workflow must survive process restarts and delayed candidate responses.

## 4. Live AI Interview and Code Execution

### 4.1 Workflow

1. Candidate opens a confirmed interview invitation.
2. Eligibility and pre-interview checks are performed.
3. Real-Time Interview Service creates an interview session.
4. AI Interviewer conducts the interview.
5. Candidate interacts with the coding workspace when required.
6. Candidate submits code and test input.
7. Execution gateway validates the request.
8. Disposable sandbox executes the code.
9. Execution results are returned to the live session.
10. Proctoring events are recorded.
11. Interview completes or terminates according to the applicable policy.

### 4.2 Important Concepts

- Live session management.
- Real-time communication.
- AI interview interaction.
- Coding workspace.
- Execution gateway.
- Disposable sandbox.
- Execution limits.
- Proctoring events.
- Interview lifecycle.

### 4.3 Important Boundary

The live interview service must not execute arbitrary candidate code directly.

Code must be sent to the isolated execution service through a controlled execution request.

## 5. Post-Interview Assessment

### 5.1 Workflow

1. Interview completion triggers assessment processing.
2. Assessment Worker retrieves permitted interview evidence.
3. Interview evidence and coding results are evaluated.
4. Structured assessment report is generated.
5. Report is saved.
6. Recruiter is notified.
7. Recruiter reviews the report and makes the hiring decision.

### 5.2 Assessment Report Contents

- Overall summary.
- Topic-wise findings.
- Coding results where relevant.
- Strengths.
- Gaps.
- Limitations.
- Traceable supporting evidence.

### 5.3 Important Principle

The report must distinguish observed evidence from AI interpretation.

AI assessment is advisory. The recruiter retains hiring authority.

## 6. Event Catalogue

| Event | Trigger | Consumer / Action |
|---|---|---|
| `ApplicationSubmitted` | Valid application saved | Resume Screening Worker |
| `ResumeScreeningCompleted` | Screening report saved | Notification Worker |
| `CandidateSelectedForInterview` | Recruiter selects candidate | Interview Coordinator |
| `InterviewScheduled` | Interview time confirmed | Notification Worker |
| `InterviewStarted` | Live session begins | Core interview records and monitoring |
| `CodeExecutionRequested` | Candidate submits code | Execution Gateway |
| `CodeExecutionCompleted` | Sandbox execution finishes | Real-Time Interview Service |
| `ProctoringViolationDetected` | Configured policy condition occurs | Interview Management |
| `InterviewCompleted` | Interview reaches normal completion | Assessment Worker |
| `InterviewTerminated` | Interview ends under a termination rule | Core Backend and required notifications |
| `AssessmentCompleted` | Assessment report stored | Recruiter notification |

## 7. Event Reliability Requirements

The architecture requires:

- Reliable event publishing.
- Transactional Outbox Pattern where appropriate.
- Idempotent consumers.
- Bounded retries with backoff.
- Dead-letter or failed-event handling.
- Event identifiers.
- Correlation identifiers.
- Minimal event payloads.
- Monitoring of failed messages.

## 8. Workflow Documentation Template

Use the following structure when documenting additional workflows:

### Workflow Name

**Objective:** TBD

**Trigger:** TBD

**Participating Components:** TBD

**Preconditions:** TBD

**Steps:**

1. TBD
2. TBD
3. TBD

**Events Involved:** TBD

**Data Created or Updated:** TBD

**Expected Result:** TBD

**Failure Scenarios:** TBD

**Related Documents:** TBD

## 9. Reference Materials

- HLD v1 – Pages 8–11: Main business workflows.
- HLD v2 – Page 2: Condensed workflow descriptions.

**End of Document**