# Requirements Specification

**Project:** Development of a Proctoring System for Online Programming Contests
**Document:** `docs/06-requirements.md`
**Status:** Draft v0.1
**Related documents:** `docs/02-evidence-of-relevance.md`

> **Evidence status.** All interview references in this document point to `docs/02-evidence-of-relevance.md`. That document contains three interviews (INT-01, INT-02, INT-03). They are not real research. 


---

## 1. Introduction

### 1.1 Purpose
This document specifies the functional and non-functional requirements of the proctoring system, assigns priorities, links each requirement to its source, and defines how each Must Have requirement will be verified.

### 1.2 Project overview
The project is a web-based platform for online programming contests, built on top of **DOMjudge**. DOMjudge handles contest administration, problem statements, submission processing, automated judging, verdicts and standings. The project adds a dedicated interface for students and teachers and proctoring functionality that detects and records suspicious browser events. The approach is *prevention plus detection*: events are recorded for human review, and browser restrictions are not presented as foolproof.

The planned frontend stack is React, TypeScript, Vite, Tailwind CSS, Monaco Editor, TanStack Query, WebSocket and i18next.

### 1.3 Scope
In scope: student and teacher interfaces, event detection and recording in the browser, warnings, teacher monitoring and incident review, results display, and integration with DOMjudge. Out of scope: see Section 3.3 and Section 9.5.

### 1.4 Intended readers
The diploma supervisor and committee, the developers (frontend and backend), and anyone who will test or extend the system.

### 1.5 Relation to the provisional IDs in `02-evidence-of-relevance.md`
That document used provisional, coarse IDs (FR-01 to FR-11, NFR-01 to NFR-03). This specification replaces them with finer-grained IDs. The mapping is:

| Provisional ID (doc 02) | Refined IDs (this document) |
|---|---|
| FR-01 Authentication | FR-001, FR-002, FR-003 |
| FR-02 Contest List | FR-004, FR-007 |
| FR-03 Contest Details and Rules | FR-005, FR-006 |
| FR-04 Programming Workspace | FR-008, FR-009, FR-010, FR-015, FR-016, FR-018 |
| FR-05 Code Execution and Submission | FR-011, FR-012, FR-013, FR-017 |
| FR-06 Submission History and Results | FR-014 |
| FR-07 / FR-07a Proctoring Notifications | FR-027, FR-028, FR-029 |
| FR-08 Proctoring Event Recording | FR-019 to FR-026 |
| FR-09 Teacher Monitoring Dashboard | FR-034 to FR-038 |
| FR-10 Participant Event Review | FR-039 to FR-042 |
| FR-11 Contest Results | FR-043, FR-044, FR-045 |
| NFR-01 Usability and Clarity | NFR-001, NFR-002, NFR-003, NFR-016 |
| NFR-02 Fairness and Human Review | NFR-013 |
| NFR-03 Privacy and Transparency | NFR-010, NFR-011, NFR-012 |

Contest creation (FR-030 to FR-033) and DOMjudge integration (FR-046, FR-047) come from the project scope and were not listed in doc 02. When this document is accepted, the matrix in doc 02 should be updated to the new IDs.

---

## 2. Stakeholders and User Roles

| Stakeholder | Status | Needs |
|---|---|---|
| Student / contest participant | **Confirmed** (project scope) | Log in, find contests, understand the rules and monitoring, solve and submit problems, see time and results, understand warnings. |
| Teacher / contest organizer | **Confirmed** (project scope) | Create and configure contests, monitor participants, review recorded events, view submissions and results, make fair decisions. |
| System administrator | **Proposed**, not confirmed in the project brief | Deploy and maintain the system, manage accounts and DOMjudge connection. No separate administrator interface is specified. |
| Diploma supervisor and committee | Evaluators of the project | Clear, traceable, honestly labelled documentation. |
| DOMjudge (external system) | **Confirmed** (project scope) | Provides contest, problem, submission and judging services through its interfaces. |

The system has two user roles to implement: Student and Teacher. No administrator role is required by this specification (see Q-07 in Section 9.4).

---

## 3. Requirement Prioritization

### 3.1 Scheme (MoSCoW)

| Priority | Meaning |
|---|---|
| **Must Have** | Essential for the minimum acceptable system, including the core scenarios in the project brief. |
| **Should Have** | Important and implemented when feasible, but the system works without it. |
| **Could Have** | Optional improvement. |
| **Won't Have (this version)** | Explicitly outside the current release. |

### 3.2 How priorities were assigned
1. A requirement is **Must Have** if a core scenario in the project brief (login, contest participation, submission, event recording, warnings, teacher monitoring, incident review, results) cannot work without it, or if it protects fairness and privacy in a way the project depends on.
2. A requirement is **Should Have** if it improves clarity, reliability or efficiency and is supported by Tech findings or the brief, but a minimal system can work without it.
3. A requirement is **Could Have** if it rests mainly on an unvalidated assumption or on convenience.
4. Priorities are provisional. Requirements based only on Tech findings should be re-prioritized after real-user research.

### 3.3 Won't Have (this version)

| ID | Excluded item | Reason |
|---|---|---|
| WH-01 | Face recognition, webcam recording, biometric analysis | Explicitly outside project scope. |
| WH-02 | Automatic disqualification, score deduction or blocking based on events | Conflicts with the human-review principle (NFR-013). |
| WH-03 | Numeric "suspicion" or "cheating probability" scores | Conflicts with NFR-013. |
| WH-04 | Operating-system-level lockdown, screen capture, reading clipboard contents | Not feasible in a browser and conflicts with NFR-010. |
| WH-05 | Automated plagiarism detection between submissions | Not in the project brief. |
| WH-06 | Native mobile applications | Desktop-first web application only. |

---

## 4. Functional Requirements

Columns: **Source** lists finding IDs from doc 02 (all Tech), or *Scope* for the project brief, or *Tech* for a technical constraint. Where only an interview question supports a requirement and no consolidated finding exists, the interview and question are named. **Role:** S = Student, T = Teacher, Sys = system.

### 4.1 Authentication and access control

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-001 | Login | The system shall authenticate a user by username or email and password and show an error message when the credentials are invalid. | Must | S, T | Scope | Given a registered user, when valid credentials are submitted, then the user reaches the start page for their role. Given invalid credentials, then no session is created and an error is shown. |
| FR-002 | Role-based access | The system shall show student functions only to students and teacher functions only to teachers. | Must | Sys | Scope | Given a logged-in student, when the student requests a teacher page or API endpoint, then the request is rejected and no teacher data is returned. |
| FR-003 | Logout and session end | The system shall allow a user to log out and shall end the session on logout. | Should | S, T | Scope | After logout, a request using the old session is rejected. |

### 4.2 Contest discovery and participation

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-004 | Contest list | The system shall display to an authenticated student the contests available to that student, each with title, status (upcoming, ongoing, completed), start time and duration. | Must | S | Scope | Given an authenticated student and contests in all three states, when the student opens the contest list, then each accessible contest is shown with its status, start time and duration. |
| FR-005 | Contest details and rules | The system shall display, for a selected contest, its description, start time, duration, number of problems, rules, and the list of monitored events. | Must | S | Scope; F-01, F-03 | Given a contest, when the student opens its details, then all listed fields are visible, including the monitored events. |
| FR-006 | Rules acknowledgment | The system shall require the student to confirm that they have read the rules and monitoring information before entering the contest workspace for the first time. | Must | S | Scope (explanation before start); F-01 | Given a student who has not confirmed, when the student attempts to enter the workspace, then entry is blocked until the confirmation is given. Given confirmation, then entry succeeds. |
| FR-007 | Search and filter contests | The system shall allow the student to search the contest list by title and filter it by status. | Could | S | Scope (design brief) | Given several contests, when a status filter is applied, then only contests with that status are displayed. |

### 4.3 Programming workspace and submissions

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-008 | Problem statement | The system shall display each problem's title, statement, input and output format, constraints and sample input and output. | Must | S | Scope | Given an ongoing contest, when the student opens a problem, then all listed parts are displayed. |
| FR-009 | Code editor | The system shall provide a code editor with syntax highlighting and line numbers and a selector for at least C++, Python and Java. | Must | S | Scope | Given the workspace, when the student selects each language, then the editor highlights that language's syntax. |
| FR-010 | Draft autosave | The system shall save the current code draft automatically and show when it was last saved. | Should | S | F-09 (INT-03 Q4) | Given typed code, when the page is reloaded, then the latest saved draft is restored. |
| FR-011 | Run on sample | The system shall let the student run the code on the sample input and see the output or error. | Should | S | Scope (design brief) | Given valid code, when the student runs it, then the sample output is shown. |
| FR-012 | Submit solution | The system shall submit the student's code, selected language and problem to DOMjudge for judging. | Must | S | Scope | Given an ongoing contest, when the student submits, then DOMjudge contains a new submission for that student, problem and language. |
| FR-013 | Submission status | The system shall show the state of each submission (queued, judging, final verdict) and update it when the verdict is available. | Must | S | F-02 (INT-01 Q4, Q10) | Given a new submission, then its status is displayed as pending; when DOMjudge returns a verdict, then the verdict is displayed without manual reload. |
| FR-014 | Submission history | The system shall list the student's own submissions with problem, language, submission time, verdict, execution time and memory usage where available. | Must | S | Scope; F-08 | Given earlier submissions, when the student opens the history, then each is listed with the stated fields, and no other student's submissions appear. |
| FR-015 | Problem solved indicator | The system shall show for each problem whether the student has an accepted submission. | Should | S | F-02 (INT-01 Q4) | Given an accepted submission on problem A, then problem A is marked solved in the problem navigation. |

### 4.4 Contest timing and status

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-016 | Remaining time | The system shall display the remaining contest time in the workspace at all times while the student is in the contest. | Must | S | Scope; F-02 | Given an ongoing contest, then the remaining time is visible on every workspace view and is consistent with the contest end time held by the server. |
| FR-017 | Submission after contest end | The system shall reject submissions made after the contest end time and tell the student why. | Must | S | Scope (design brief states) | Given a contest that has ended, when the student submits, then no submission is accepted and a message states that the contest has ended. |
| FR-018 | Connection indicator | The system shall show the student whether the connection to the server is active. | Should | S | F-09 | Given a lost connection, then the indicator shows the disconnected state; when it is restored, then it shows the connected state. |

### 4.5 Proctoring event detection and recording

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-019 | Tab and window switch detection | The system shall detect when the contest page loses visibility or focus during an ongoing contest and record the event. | Must | Sys | Scope | Given an active contest session, when the student switches to another tab, then an event of type "tab switch" is stored for that student. |
| FR-020 | Fullscreen exit detection | The system shall detect when the student leaves fullscreen mode during the contest and record the event. | Must | Sys | Scope | Given fullscreen mode active, when the student exits it, then an event of type "fullscreen exit" is stored. |
| FR-021 | Clipboard action detection | The system shall detect copy, cut and paste actions in the workspace and record that the action occurred, without recording the clipboard content. | Must | Sys | Scope; F-10 | Given the workspace, when the student pastes text, then an event of type "paste" is stored and the stored record does not contain the pasted text. |
| FR-022 | Event persistence | The system shall store each recorded event with type, timestamp, student, contest and, where technically feasible, duration. | Must | Sys | Scope; F-05 | Given any recorded event, when the stored record is inspected, then it contains the listed fields. |
| FR-023 | Connection events | The system shall record when a student's connection is lost and restored. | Should | Sys | F-09 | Given a simulated connection loss and restore, then two events with timestamps are stored. |
| FR-024 | Duplicate event suppression | The system shall merge repeated identical events caused by one action within a short interval (value to be validated) into one recorded event. | Should | Sys | F-03 (INT-03 Q6); Assumption | Given two identical events within the configured interval, then one record is stored. |
| FR-025 | Per-contest monitoring settings | The system shall let the teacher choose which event types are monitored for a contest and the number of events after which the student sees a warning message. | Should | T | Scope (design brief) | Given tab-switch monitoring turned off, when the student switches tabs, then no event is stored. |
| FR-026 | Fullscreen prompt | When fullscreen is required by the contest settings, the system shall ask the student to enter fullscreen on entering the workspace. | Should | S | Scope (design brief) | Given a contest requiring fullscreen, when the student enters the workspace, then the fullscreen prompt is shown. |

### 4.6 Student warnings and feedback

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-027 | Proctoring warning | After a monitored event, the system shall show the student a message stating the event type, the time, that the event has been recorded for review, and the action to take, in neutral wording that does not state or imply misconduct. | Must | S | Scope; F-03, F-06 | Given a recorded event, then the student sees a message with the four listed elements. The text does not contain accusatory terms (see NFR-001). |
| FR-028 | Warning severity levels | The system shall present notifications at three levels (informational, warning, critical), proportionate to the event. | Should | S | Scope (design brief); F-03 | Given events of different types and counts, then each is displayed at the level defined in the severity table. |
| FR-029 | Student context note | The system shall allow the student to add a short note to a recorded event. | Could | S | F-06 (INT-03 Q8); Assumption A-6 | Given a recorded event, when the student adds a note, then the note is stored and shown with the event to the teacher. |

### 4.7 Contest management (teacher)

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-030 | Create and configure contest | The system shall allow a teacher to create a contest with title, description, start date and time, duration and time zone. | Must | T | Scope | Given valid inputs, when the teacher saves, then the contest exists in DOMjudge and in the contest list. Given a missing title, then saving is refused with a validation message. |
| FR-031 | Select problems and order | The system shall allow the teacher to select problems for a contest and set their order. | Must | T | Scope | Given selected problems, then students see them in the set order. |
| FR-032 | Assign participants | The system shall allow the teacher to assign participants individually or by student group. | Must | T | Scope | Given an assigned group, then its students see the contest, and unassigned students do not. |
| FR-033 | Draft and publish | The system shall allow a contest to be saved as a draft and published later. | Should | T | Scope (design brief) | A draft is not visible to students. After publishing, it is visible to assigned students. |

### 4.8 Teacher monitoring dashboard

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-034 | Participants table | The system shall show the teacher, for an ongoing contest, a table of participants with progress, current problem, last submission, connection status, number of recorded events and last activity. | Must | T | Scope; F-07 | Given an ongoing contest with participants, then each participant appears with the listed columns. |
| FR-035 | Filter and sort | The system shall allow the teacher to filter the table (all, active, with events, needing review) and sort by event count and last activity. | Should | T | F-07 | Given the filter "with events", then only participants with at least one event are listed. |
| FR-036 | Live update | The system shall update the participants table and event feed during the contest without a manual page reload. | Must | T | Scope ("real time") | Given an open dashboard, when a student triggers an event, then the dashboard shows it without reload. Delay target: see NFR-004. |
| FR-037 | Event feed | The system shall show recent events with event type, student, time, severity and review status. | Must | T | Scope; F-05, F-07 | Given recorded events, then the feed lists them in time order with the listed fields. |
| FR-038 | Optional live alerts | The system shall allow the teacher to enable or disable on-screen alerts for new events. | Could | T | Assumption A-2 (INT-02 Q10, undecided) | With alerts enabled, a new event shows an alert. With alerts disabled, none is shown. |

### 4.9 Incident review and results

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-039 | Participant details and timeline | The system shall show for one participant a chronological timeline of recorded events, warnings shown and submissions, with timestamps. | Must | T | Scope; F-05 | Given a participant with events and submissions, then the timeline lists all of them in chronological order. |
| FR-040 | Review notes | The system shall allow the teacher to add, view and edit review notes attached to a participant or an event. | Must | T | Scope; F-05 (INT-02 Q7) | Given a note saved on an event, when the page is reopened, then the note is shown. |
| FR-041 | Review status | The system shall allow the teacher to set a review status for an event or incident (for example not reviewed, reviewed with no concern, needs follow-up). | Should | T | F-04 | Given a status change, then it is stored and displayed in the table and feed. |
| FR-042 | Attention indicator | The system shall show a neutral indicator of whether a participant has events that may need review, derived from event counts and teacher-set thresholds, without a numeric score and without wording that implies misconduct. | Should | T | F-06, F-07 | Given a participant above the threshold, then the indicator reads "needs review". No percentage or score is displayed. |
| FR-043 | Contest results | The system shall display the contest scoreboard with rank, problems solved and penalty time, based on DOMjudge data. | Must | T | Scope | Given a finished contest, then the scoreboard matches the DOMjudge standings for the same contest. |
| FR-044 | Review list after contest | The system shall let the teacher list participants with events or review status "needs follow-up" after a contest ends. | Should | T | INT-02 Q8 (no consolidated finding) | Given a finished contest, then the list shows the matching participants. |
| FR-045 | Export | The system shall allow export of results, and optionally of event records, to a file. | Could | T | INT-02 Q8 (undecided); Assumption | Given a finished contest, when export is requested, then a file with the results is produced. |

### 4.10 Data storage and DOMjudge integration

| ID | Name | Requirement | Priority | Role | Source | Acceptance criterion |
|---|---|---|---|---|---|---|
| FR-046 | DOMjudge data synchronization | The system shall obtain contests, problems, submissions, verdicts and standings from DOMjudge and use DOMjudge as the authority for judging results. | Must | Sys | Scope | Given a verdict in DOMjudge, then the same verdict is shown in the system for that submission. |
| FR-047 | Proctoring data storage | The system shall store proctoring events and review notes in the system's own storage, linked to contest and participant identifiers. | Must | Sys | Scope; Tech | Given a recorded event, then it can be retrieved by contest and participant, and it is not required to be stored inside DOMjudge. |

**Count:** 47 functional requirements.

| Priority | FR count |
|---|---|
| Must Have | 28 |
| Should Have | 15 |
| Could Have | 4 |
| **Total** | **47** |

### 4.11 Severity levels (supports FR-028)

| Level | Used for | Presentation |
|---|---|---|
| Informational | A single, brief event (for example one short focus loss) | Small notice, no interruption of typing |
| Warning | A clear event such as leaving fullscreen, or a repeat | Dialog with return action |
| Critical | Several events above the teacher-set threshold | Dialog explaining that events are recorded and will be reviewed, with no statement of misconduct |

The thresholds are a teacher setting (FR-025). Default values are not defined here and need validation.

---

## 5. Non-Functional Requirements

Numeric targets below are **proposed targets requiring validation**; no measurement or evidence supports them yet.

| ID | Category | Name | Requirement | Priority | Rationale / Source | Verification / Acceptance criterion |
|---|---|---|---|---|---|---|
| NFR-001 | Usability | Neutral, plain wording | All student-facing warnings and rules shall use plain language and shall not state or imply misconduct (for example "cheating detected" is not permitted). | Must | F-03, F-06 | Inspection: every warning text is reviewed against a list of prohibited phrases; none is present. |
| NFR-002 | Usability | Workspace layout | The workspace shall show the timer, problem statement and code editor at the same time at 1440 × 900 without scrolling the page. | Should | F-02; design brief | Manual check at 1440 × 900. |
| NFR-003 | Usability | Localization readiness | All interface text shall be stored in translation files, not in code. | Could | Tech (i18next in stack) | Inspection of the code base. Languages to support: see Q-08. |
| NFR-004 | Performance | Event delivery delay | An event recorded in a student's browser should appear on the teacher dashboard within 5 seconds under normal conditions (**proposed target, requires validation**). | Should | Scope ("real time"); Assumption | Test with a simulated event and timestamps on both sides. |
| NFR-005 | Reliability | Event buffering | If the connection is lost, the client shall keep recorded events and send them after reconnection, preserving the original timestamps. | Should | F-09 | Simulate disconnection, trigger events, reconnect: events are stored with their original times. |
| NFR-006 | Reliability | Draft preservation | A page reload shall not lose the last autosaved code draft. | Should | F-09 | See FR-010 test. |
| NFR-007 | Security | Server-side authorization | The server shall enforce role and ownership checks on all endpoints. A student shall not read proctoring records, other students' submissions, or teacher functions. | Must | Scope; F-04 | Access-control test: student credentials used against teacher and other-student endpoints return access-denied. |
| NFR-008 | Security | Credential protection | Credentials shall be transmitted over HTTPS and stored only as salted hashes. | Must | Tech (standard practice) | Inspection of deployment configuration and storage. Requests over plain HTTP are not served in the deployed configuration. |
| NFR-009 | Security | Client events are not trusted | The server shall assign or verify event timestamps and shall treat client-reported events as signals, not as authoritative proof. | Should | Tech | Review of server code; test with a modified client timestamp. |
| NFR-010 | Privacy | No clipboard content | The system shall not read or store clipboard contents. | Must | F-10 (INT-03 Q7) | Inspection of stored records after a paste test: no pasted text is present. |
| NFR-011 | Privacy | Disclosure of monitoring | Before the contest, the system shall tell students which events are recorded, which are not recorded, and who can see the records. | Must | F-01, F-10 | Inspection: the contest details and acknowledgment screen contain the three statements. |
| NFR-012 | Privacy | Retention | The retention period for proctoring records shall be defined and configurable. The value is not decided (see Q-03). | Should | Assumption | Inspection of configuration; value is an open question. |
| NFR-013 | Fairness | Human review | The system shall not apply automatic penalties, disqualification, blocking or numeric suspicion scores based on proctoring events. Events shall be presented as indicators for human review. | Must | Scope; F-04, F-06 | Inspection and test: after several events, the student's submissions and score are unchanged, and no score is displayed. |
| NFR-014 | Compatibility | Browsers | The web application shall work on current desktop versions of Chrome, Firefox and Edge (**proposed**; Safari support to be decided). | Should | Tech | Compatibility testing of main scenarios on each browser. |
| NFR-015 | Maintainability | Documentation | The repository shall contain setup instructions and a description of the API between frontend and backend. | Should | Tech | Inspection; a new developer can run the system following the instructions. |
| NFR-016 | Accessibility | Contrast and status | Status shall not be conveyed by color alone, and text shall meet a sufficient contrast ratio (WCAG AA proposed). | Should | Design brief | Inspection with a contrast checker. |



---

## 6. Proctoring-Specific Requirements and Ethical Constraints

### 6.1 Principle
A recorded event is a **signal for human review, not proof of misconduct**. The system does not decide whether a student cheated. Tab switches, fullscreen exits and clipboard actions can have innocent causes.

### 6.2 What the system does
- Detects and records tab or window switching, fullscreen exit and clipboard actions (FR-019 to FR-021), plus connection loss (FR-023), with type and timestamp (FR-022).
- Tells students in advance what is monitored (FR-005, FR-006, NFR-011) and informs them when an event is recorded (FR-027).
- Gives teachers a timeline, notes and review status (FR-039 to FR-041).
- Restricts proctoring records to the teacher role (FR-002, NFR-007).

### 6.3 What the system does not do
- It does not guarantee that cheating cannot happen, and it does not claim to detect every form of it (a student can use another device, for instance).
- It does not apply automatic penalties or scores (NFR-013, WH-02, WH-03).
- It does not read the clipboard (NFR-010) and does not use cameras or biometrics (WH-01).
- It does not treat a single event as grounds for action (FR-042 uses teacher-set thresholds and a neutral label).

### 6.4 Technical limitations (browser)
- Detection depends on browser events (page visibility and focus, fullscreen change, clipboard events). Their availability and behavior differ between browsers.
- Focus can be lost without the student's action, for example when an operating-system notification appears. These cases can produce false events (INT-03, Tech).
- A browser cannot see activity on another device or outside the browser.
- Client-side code runs on the student's machine, so reports from it can be altered or blocked. They are therefore not authoritative (NFR-009).
- Fullscreen can normally be entered only after a user action, so the system asks the student (FR-026).
- A page cannot always distinguish copying from the problem statement from copying elsewhere in the page. How to treat copying of sample input is open (Q-02).

### 6.5 Privacy and retention
Proctoring records contain personal data. Access is limited to authorized teachers; the contents of the clipboard are not stored; the retention period and the right of the student to view their own record are undecided (Q-03, Q-04) and must be settled with the supervisor and the institution before real use.

---

## 7. Requirements Traceability to Interview Evidence

**Reading this table.** "Related Finding ID" uses the finding IDs (F-01 to F-10) from `docs/02-evidence-of-relevance.md`. The existing document uses the prefix F-, so these IDs are reused unchanged and no IF- identifiers were created. **Basis** shows what kind of evidence supports each requirement: *Scope* (project brief), *Tech* (technical constraint), or *Assumption*.

| Requirement ID | Requirement Summary | Related Finding ID | Scope | Stakeholder Need | Priority |
|---|---|---|---|---|---|
| FR-001 | Login | — | Scope | Access with the correct role | Must |
| FR-002 | Role-based access | — | Scope | Separate student and teacher functions | Must |
| FR-003 | Logout | — | Scope | End a session | Should |
| FR-004 | Contest list | — | Scope | Find contests by state | Must |
| FR-005 | Contest details and rules | F-01, F-03 | Scope | Know the rules and monitoring | Must |
| FR-006 | Rules acknowledgment | F-01 | Scope | Short rules before the start | Must |
| FR-007 | Search and filter contests | — | Scope | Find a contest quickly | Could |
| FR-008 | Problem statement | — | Scope | Read the problem | Must |
| FR-009 | Code editor | — | Scope | Write code in several languages | Must |
| FR-010 | Draft autosave | F-09 | Tech | Code not lost | Should |
| FR-011 | Run on sample | — | Scope | Test before submitting | Should |
| FR-012 | Submit solution | — | Scope | Get a solution judged | Must |
| FR-013 | Submission status | F-02 | Tech | Know whether a submission is received | Must |
| FR-014 | Submission history | F-08 | Scope, Tech | Track and prove submissions | Must |
| FR-015 | Problem solved indicator | F-02 | Scope | See progress | Should |
| FR-016 | Remaining time | F-02 | Scope | Stay oriented in time | Must |
| FR-017 | Submission after end | — | Scope | Clear contest boundary | Must |
| FR-018 | Connection indicator | F-09 | Tech | Know when connection drops | Should |
| FR-019 | Tab switch detection | — | Scope | Teachers receive events | Must |
| FR-020 | Fullscreen exit detection | — | Scope | Teachers receive events | Must |
| FR-021 | Clipboard action detection | F-10 | Scope | Action recorded, not content | Must |
| FR-022 | Event persistence | F-05 | Scope | Reviewable evidence | Must |
| FR-023 | Connection events | F-09 | Tech | Separate technical issues from other events | Should |
| FR-024 | Duplicate suppression | F-03 | Assumption (INT-03 Q6) | Avoid repeated counts | Should |
| FR-025 | Per-contest settings | — | Scope | Teacher control of monitoring | Should |
| FR-026 | Fullscreen prompt | — | Scope, Tech | Enter fullscreen when required | Should |
| FR-027 | Proctoring warning | F-03, F-06 | Scope | Understand a warning without being accused | Must |
| FR-028 | Severity levels | F-03 | Scope | Proportionate notifications | Should |
| FR-029 | Student context note | F-06 | Assumption (A-6) | Explain accidental events | Could |
| FR-030 | Create and configure contest | — | Scope | Organize contests | Must |
| FR-031 | Select problems and order | — | Scope | Compose a contest | Must |
| FR-032 | Assign participants | — | Scope | Control access | Must |
| FR-033 | Draft and publish | — | Scope | Prepare before release | Should |
| FR-034 | Participants table | F-07 | Scope, Tech | See who needs attention quickly | Must |
| FR-035 | Filter and sort | F-07 | Tech | Sort and filter participants | Should |
| FR-036 | Live update | — | Scope | Real-time monitoring | Must |
| FR-037 | Event feed | F-05, F-07 | Scope | Plain chronological events | Must |
| FR-038 | Optional live alerts | F-07 (open item) | Assumption (A-2) | Alerts or on-demand checking | Could |
| FR-039 | Participant details and timeline | F-05 | Scope | Context for review | Must |
| FR-040 | Review notes | F-05, F-04 | Scope, Tech | Keep the outcome of a review | Must |
| FR-041 | Review status | F-04 | Tech | Teacher decides | Should |
| FR-042 | Attention indicator | F-06, F-07 | Tech | See who to check without a score | Should |
| FR-043 | Contest results | — | Scope | Grade the contest | Must |
| FR-044 | Review list after contest | INT-02 Q8 | Tech  | Review flagged participants | Should |
| FR-045 | Export | INT-02 Q8 | Assumption | Use results elsewhere | Could |
| FR-046 | DOMjudge synchronization | — | Scope, Tech | One source of judging truth | Must |
| FR-047 | Proctoring data storage | F-05 | Scope, Tech | Keep review data | Must |
| NFR-001 | Neutral, plain wording | F-03, F-06 | Tech | No accusation | Must |
| NFR-002 | Workspace layout | F-02 | Tech, design brief | Focus on coding | Should |
| NFR-003 | Localization readiness | — | Tech | Interface in several languages | Could |
| NFR-004 | Event delivery delay (proposed 5 s) | — | Assumption | Timely monitoring | Should |
| NFR-005 | Event buffering | F-09 | Tech | Do not lose events on disconnect | Should |
| NFR-006 | Draft preservation | F-09 | Tech | Do not lose code | Should |
| NFR-007 | Server-side authorization | F-04 | Scope, Tech | Records visible to authorized roles only | Must |
| NFR-008 | Credential protection | — | Tech | Basic account security | Must |
| NFR-009 | Client events not trusted | — | Tech | Honest limits of detection | Should |
| NFR-010 | No clipboard content | F-10 | Tech | Privacy | Must |
| NFR-011 | Disclosure of monitoring | F-01, F-10 | Tech | Know what is stored and who sees it | Must |
| NFR-012 | Retention | — | Assumption | Data minimization | Should |
| NFR-013 | Human review | F-04, F-06 | Scope, Tech | Fair handling | Must |
| NFR-014 | Browsers | — | Tech | Run where students work | Should |
| NFR-015 | Documentation | — | Tech | Maintainability | Should |
| NFR-016 | Contrast and status | — | Design brief | Accessible status | Should |

**Findings with no requirement of their own.** All of F-01 to F-10 appear at least once above.
**Requirements with no finding.** Where the Related Finding column shows "—", the requirement rests on project scope or technical constraints and is not presented as supported by interviews.

---

## 8. Acceptance Criteria and Verification

### 8.1 Verification methods

| Method | Used for |
|---|---|
| Functional testing | Student and teacher scenarios (FR-004 to FR-017, FR-030 to FR-043) |
| Integration testing | DOMjudge exchange (FR-012, FR-013, FR-043, FR-046) |
| Security / access-control testing | FR-002, NFR-007, NFR-008 |
| Usability testing | NFR-001, NFR-002, warning comprehension (FR-027) |
| Inspection of stored records | FR-021, FR-022, FR-047, NFR-010 |
| Manual review of proctoring events | FR-019 to FR-021, FR-037, FR-039 |
| Compatibility testing | NFR-014 and fullscreen and focus behavior |

### 8.2 Must Have coverage
Every Must Have requirement has an acceptance criterion in its table row in Section 4 or Section 5:

- Functional Must Have: FR-001, 002, 004, 005, 006, 008, 009, 012, 013, 014, 016, 017, 019, 020, 021, 022, 027, 030, 031, 032, 034, 036, 037, 039, 040, 043, 046, 047 (28 requirements).
- Non-functional Must Have: NFR-001, 007, 008, 010, 011, 013.

### 8.3 Status of testing
No test has been run. The criteria describe how each requirement will be checked once implemented. Acceptance criteria written in "Given / When / Then" form are to be turned into test cases.

### 8.4 Count check
Functional requirements: Must Have 28 (the list in 8.2), Should Have 15, Could Have 4, total 47. Non-functional requirements: Must Have 6, Should Have 9, Could Have 1, total 16. If a priority changes, update this note and the counts in Section 4 and Section 5.

---

## 9. Assumptions, Constraints, and Open Questions

### 9.1 Assumptions requiring confirmation
Identifiers A-1 to A-8 are those of `docs/02-evidence-of-relevance.md` Section 7C.

| ID | Assumption | Affects |
|---|---|---|
| A-1 | Students prefer short, specific warnings | FR-027 |
| A-2 | Teachers prefer on-demand checking to live alerts | FR-038 |
| A-3 | Tab, fullscreen and clipboard events are the most useful for review | FR-019 to FR-021 |
| A-4 | A timeline gives enough detail for review | FR-039 |
| A-5 | Students accept clipboard action recording if stated in advance | FR-021, NFR-011 |
| A-6 | Students should be able to add context to an event | FR-029 |
| A-7 | Review notes are visible to teachers only | FR-040 |
| A-8 | A prominent timer does not cause harmful stress | FR-016 |

### 9.2 Technical constraints
- DOMjudge is the authority for judging, contests and standings; the system uses its interfaces and cannot change its internal behavior.
- Proctoring depends on browser capabilities (Section 6.4).
- Frontend stack: React, TypeScript, Vite, Tailwind CSS, Monaco Editor, TanStack Query, WebSocket, i18next.
- Deployment environment and hosting are not defined in the sources used.

### 9.3 Requirements that need validation with real users
All requirements whose Basis is *Tech* or *Assumption* in Section 7, in particular FR-006 (acknowledgment step), FR-024, FR-027 wording, FR-029, FR-038, FR-042 and NFR-004.

### 9.4 Open questions

| ID | Question |
|---|---|
| Q-01 | What are the default thresholds for warning levels? |
| Q-02 | How should copying sample input from the problem statement be treated? |
| Q-03 | How long are proctoring records retained, and who deletes them? |
| Q-04 | May a student see their own recorded events? |
| Q-05 | Are review notes visible to anyone other than the teacher? |
| Q-06 | Is authentication handled by the system or delegated to DOMjudge or a university account? |
| Q-07 | Is a system administrator role needed? |
| Q-08 | Which interface languages are required? |
| Q-09 | Which browsers must be supported, including Safari? |
| Q-10 | Does the institution require approval for monitoring student activity? |

### 9.5 Excluded features
See Section 3.3 (WH-01 to WH-06).

---

## 10. Summary

**Main capabilities.** Students log in, view contests and rules, work in a code editor, submit to DOMjudge, see verdicts and history, and receive neutral warnings. Teachers create contests, monitor participants in a table with a live event feed, review a participant timeline, add notes, and view results.

**Key non-functional constraints.** Server-side access control (NFR-007), no clipboard content stored (NFR-010), disclosure of monitoring (NFR-011), neutral wording (NFR-001), and no automatic penalties or scores (NFR-013).

**Highest-priority requirements.** The 28 functional and 6 non-functional Must Have requirements, in particular FR-012 (submission), FR-019 to FR-022 (event detection and recording), FR-027 (warnings), FR-034 to FR-040 (monitoring and review) and NFR-013.


