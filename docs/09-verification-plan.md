# Verification and Evaluation Plan

**Project:** Development of a Proctoring System for Online Programming Contests
**Document:** `docs/09-verification-plan.md`
**Status:** Planning document, draft v0.1
**Related documents:** `docs/02-evidence-of-relevance.md`, `docs/06-requirements.md`, `prototype/README.md`


---

## 1. Introduction

### 1.1 Purpose
This document defines how the proctoring system will be tested and evaluated: which tests will be run, which metrics will be measured, under which conditions, and against which baseline. It lets another person repeat the measurements and judge the results.

### 1.2 System under evaluation
A web application for online programming contests built on DOMjudge. It adds a student interface (login, contest list, rules, workspace with code editor and timer, warnings), proctoring event detection and recording (tab or window switch, fullscreen exit, clipboard actions, connection events), and a teacher interface (contest creation, live monitoring, incident review, results). The planned frontend stack is React, TypeScript, Vite, Tailwind CSS, Monaco Editor, TanStack Query, WebSocket and i18next. The backend technology and deployment are .

### 1.3 Scope
In scope: functional behavior, DOMjudge integration, event recording, timer consistency, concurrency, bypass attempts, and usability. Out of scope: face recognition, webcam, biometric analysis, automated plagiarism detection and any automatic penalties (see `06-requirements.md`, Section 3.3). Also out of scope: certifying that the system prevents cheating.

### 1.4 Requirements, tests and metrics
Each test case in Section 7 names the requirement IDs from `docs/06-requirements.md` that it checks. The six mandatory metrics in Section 4 are measured by specific test cases. A requirement is *verified* only when its test cases have status `Passed` with evidence stored under Section 9.

**Gap in the requirements document.** `06-requirements.md` has no non-functional requirements for four of the six metrics (timer discrepancy, concurrent participants, coverage, event recording rate) or for SUS. Its only timing requirement, NFR-004, proposes that an event reach the teacher dashboard within 5 seconds. This plan uses stricter targets with different measurement boundaries (see Section 4.1). Recommended action: add new NFRs for these metrics to `06-requirements.md` and give them stable IDs. Until then the table in Section 7 marks them as *no existing ID*, and no ID is invented here.

---

## 2. Verification Objectives

| # | Objective | Type |
|---|---|---|
| VO-1 | Functional behavior matches the requirements (login, contests, workspace, submission, results). | Technical verification |
| VO-2 | The proctoring interface and DOMjudge exchange data correctly (contests, problems, submissions, verdicts, standings). | Technical verification |
| VO-3 | Proctoring events are detected, delivered and stored accurately and in time. | Technical verification |
| VO-4 | The contest timer is consistent with the server. | Technical verification |
| VO-5 | The system remains stable with at least 50 concurrent participants. | Technical verification |
| VO-6 | Attempts to bypass or suppress browser-side monitoring are understood, and their outcomes and limits are documented. | Security verification |
| VO-7 | Students and teachers can use the interface effectively and with acceptable satisfaction. | User experience evaluation |

**Technical verification vs UX evaluation.** VO-1 to VO-6 are answered by repeatable tests with defined pass or fail results. VO-7 is answered by a questionnaire (SUS) and observation, whose results depend on the participants and have a margin of uncertainty.

---

## 3. Testing Strategy

### 3.1 Unit testing

**Tools.** The test framework in the repository is . For a Vite and TypeScript frontend a suitable choice is Vitest with React Testing Library, run with `npm test` (*proposal*, not confirmed to be installed). Backend unit tests use the framework of the chosen backend language .

**What is tested**

| Area | Examples |
|---|---|
| Timer logic | Remaining time computed from server end time and a server-time offset; behavior at zero; no negative values; behavior when the local clock changes. |
| Event detection | Visibility, focus, fullscreen and clipboard handlers produce the expected event object. Clipboard handler records the action but not the content (NFR-010). |
| Event formatting | Event type, timestamp, contest and student identifiers, duration are present and correctly typed. |
| Timestamps and recording | Timestamp source, ordering, duplicate suppression window (FR-024), buffering when offline (NFR-005). |
| Validation and access control | Input validation, role checks and ownership checks (FR-002, NFR-007). |
| Contest and submission logic | Contest status (upcoming, ongoing, completed), rejection after end time (FR-017), submission payload creation. |
| Error handling | Backend errors, DOMjudge unavailable, malformed payloads, expired session. |



### 3.2 Integration testing

| Interaction | What is checked | Failure detection |
|---|---|---|
| Frontend and backend | Requests and responses match the documented API; error responses are handled in the UI. | Failed assertions; HTTP status and body mismatch. |
| Authentication and RBAC | Valid and invalid login; student cannot reach teacher endpoints; teacher sees only own contests. | Any 2xx response where 401 or 403 is expected. |
| Contest data retrieval | Contest list, details and problems match DOMjudge data. | Differences between DOMjudge and system output. |
| Timer across components | Frontend timer, backend end time and DOMjudge contest clock agree. | Discrepancy above the Section 4 threshold. |
| Event delivery and persistence | An event generated in the browser appears in storage with correct fields. | Missing, duplicate or late record. |
| Teacher dashboard | Table and feed show events of the right participants, updated without reload. | Event not displayed within the measured delay. |
| Submission and DOMjudge | Submission reaches DOMjudge and the verdict returns to the student view. | Submission absent in DOMjudge, or verdict mismatch. |

Features not yet implemented are tested only when they exist. Each case in Section 7 is `Planned` until the feature is implemented and the test is run.

### 3.3 Load testing

**Goal.** Establish whether the system supports at least **50 concurrent participants** (target in Section 4) and measure the cost added by proctoring.

**Tool.** No load-testing tool is confirmed in the repository . *Proposal:* k6 (JavaScript scenarios, supports HTTP and WebSocket) or Locust (Python). The choice does not affect the plan.

**Simulated participant (virtual user, VU).** Each VU: logs in; opens the contest list and details; enters the workspace; opens a WebSocket; sends heartbeat or timer synchronization as the application does; generates proctoring events at scripted times from a fixed event list; submits solutions at scripted times; reads submission status. A separate VU group acts as teachers and holds the monitoring dashboard open.

**Procedure**
1. Configure the load generator on a machine separate from the system under test, with its own CPU and network usage recorded.
2. Reset the test data (Section 5.9) and record the build identifier.
3. **Warm-up:** run 5 VUs for 2 minutes (*proposed*), discard these measurements.
4. **Ramp:** increase to 50 VUs, starting a new VU every 2 seconds (*proposed*).
5. **Hold:** keep 50 VUs for the main run, long enough to cover several timer synchronizations and a full scripted event list (*proposed: 15 minutes*).
6. Optionally run a **stress step** at 75 and 100 VUs to find where behavior degrades. This is not required for the target.
7. Collect results (Section 5.8), then repeat the run (Section 5.7).

**Measured.** Login and page response time (median, p95, max), HTTP error rate, WebSocket disconnects, event delivery latency, event recording rate, timer discrepancy sampled by a subset of VUs, CPU and memory of backend, database and DOMjudge.

**Pass condition for the concurrency metric.** At 50 simulated participants, all VUs complete the scripted scenario, with the event latency, recording rate and timer targets met under that load. An error-rate limit (*proposed*: below 1% of requests, excluding scripted negative tests) must be confirmed as a planning decision.

### 3.4 Restriction-bypass and security testing

Controlled tests by the developer, on a test environment, using test accounts and test contests only.

**Reliably detectable by a browser vs not.**

| Behavior | Detectable by a normal web page? | Note |
|---|---|---|
| Switching browser tab or minimizing | Generally yes (page visibility and focus events) | Event timing and availability vary between browsers. |
| Switching to another application window | Often (focus loss) | Depends on the browser and operating system. |
| Leaving fullscreen | Yes (fullscreen change event) | Fullscreen must be entered after a user action. |
| Copy, cut, paste inside the page | Yes for actions in the page | Content is deliberately not recorded (NFR-010). Copying outside the page is invisible. |
| Reload, close or navigate away | Partly | A closing tab may not get time to send an event; the server may only see a disconnect. |
| Network loss | Partly | Seen by the server as a disconnect; client cannot send until back online. |
| Use of another device, phone or a second computer | No | Outside the browser. |
| Screen sharing, remote control, virtual machine | No | Outside the browser. |
| Disabled or modified client code | Not reliably | Client code runs on the participant's machine and can be altered. |
| A window beside the contest without focus change (split screen) | Not reliably | May produce no event. |

The system does not claim to prevent or detect every form of cheating. Where bypass is technically possible, the result is recorded as a limitation, with a mitigation proposal or an explicit risk acceptance.

**Per-scenario details (preconditions, steps, expected behavior, evidence, known limitation)**

| ID | Scenario | Preconditions | Steps | Expected behavior | Evidence | Known limitation |
|---|---|---|---|---|---|---|
| SEC-001 | Tab switch | Student in an ongoing contest, monitoring on | Switch to another tab for about 10 s, return | Event "tab switch" stored with time and duration; warning shown | DB record, screenshot, latency sample | Browser differences |
| SEC-002 | Exit fullscreen | Fullscreen required and active | Press Esc | Event stored; reminder shown with a way to return | DB record, screenshot | Cannot force fullscreen without user action |
| SEC-003 | Clipboard action | Workspace open, clipboard monitoring on | Copy and paste in editor | Event stored; stored record has no pasted text | DB record inspection | Copying outside the page is not seen |
| SEC-004 | Reload or navigate away | Active session | Reload; navigate to another site and back | Session recovers; draft restored; event or disconnect recorded as designed | Logs, DB records | Closing may not allow the client to send an event |
| SEC-005 | Close and reopen tab | Active session | Close tab, reopen contest URL within contest time | Participant can resume (or the rule is documented); gap visible to teacher | Logs, timeline view | Exact server behavior  |
| SEC-006 | Network disconnect and reconnect | Active session | Disable network for 30 s, restore | Connection loss and restore events; buffered events delivered with original timestamps | DB records, timestamps | Events cannot leave the device while offline |
| SEC-007 | Client script manipulation | Developer tools available | Remove or disable event listeners; override visibility API | Document what happens: likely no event is generated by the client | Console log, DB records | Not preventable in a web page; risk accepted, or server-side heartbeat gaps used as a signal |
| SEC-008 | Forge or modify event data | Intercepting proxy or developer tools | Alter event type or timestamp; replay an old event; send events for another student | Server assigns or validates timestamps; events for other users rejected (NFR-009, NFR-007) | Request and response logs | Server cannot verify that the client event is true |
| SEC-009 | Suppress event transmission | Intercepting proxy | Block event requests or the WebSocket while continuing to work | Document outcome; server detects missing heartbeat or timer synchronization | Logs | Blocking is possible; recorded as limitation |
| SEC-010 | Student reaches teacher functions | Student account | Call teacher pages and endpoints directly; change identifiers in URLs | 401 or 403; no data returned (FR-002, NFR-007) | HTTP logs | None expected |
| SEC-011 | Second device | None | Use a phone beside the computer | No event (not detectable) | Written note | Out of scope for browser monitoring |

---

## 4. Quantitative Evaluation Metrics

All values are **acceptance targets**. No achieved value is claimed.

| Metric | Target | Measurement Method |
|---|---|---|
| Timer discrepancy | ≤ 1 second | Compare the displayed or recorded remaining time with a trusted reference clock over a defined interval |
| Event delivery latency | ≤ 2 seconds | Measure elapsed time from event generation to receipt or persistence at the designated backend component |
| Concurrent participants | ≥ 50 | Run a load test with at least 50 simulated concurrent participants |
| Test coverage | ≥ 70% | Run the relevant coverage tool and record the measured percentage |
| Violation/event recording rate | ≥ 90% | Compare the number of successfully recorded eligible test events against the number of events deliberately generated |
| System Usability Scale (SUS) | ≥ 70 | Administer the standard 10-item SUS questionnaire and calculate the score using the accepted scoring method |

### 4.1 Definitions for reproducibility

**M1. Timer discrepancy**
- *Reference clock:* the server's contest end time and server clock, with the server synchronized to an NTP source (Section 5.6). In the baseline, DOMjudge's own contest clock.
- *Sample:* a measurement client records, at a sampling interval (*proposed: every 10 s*) over the full test duration, the pair (displayed remaining time `D`, reference remaining time `R` at the same instant, using a synchronized clock).
- *Discrepancy per sample:* `|D − R|`.
- *Pass:* the maximum discrepancy over all samples is ≤ 1 s. The median and 95th percentile are also reported. The test must include at least one reload and one temporary network loss, so that resynchronization is exercised.
- *Boundary:* the displayed value is read from the rendered timer in the browser (automated browser test), not from internal state.

**M2. Event delivery latency**
- *Start (`t_gen`):* the instant the browser event handler fires. In scripted tests, the harness stamps it with a synchronized clock.
- *End (`t_persist`):* the instant the event is committed to the persistent store (database commit time or the backend's receipt log, whichever is designated ). Delivery to the teacher dashboard is measured separately as an additional value (compare NFR-004, proposed 5 s).
- *Latency:* `t_persist − t_gen` per event.
- *Pass (proposed interpretation, planning decision to confirm):* the 95th percentile is ≤ 2 s. Median, 99th percentile and maximum are also reported. Events generated while the client is offline are excluded from this metric and counted in M5 as delayed events.

**M3. Concurrent participants**
- *Numerator:* number of virtual users that complete the scripted scenario (login, workspace, events, submission) without a failure of their own.
- *Pass:* at least 50 simulated concurrent participants held for the hold period, with M1, M2 and M5 met under that load.
- The load generator and the system under test run on separate machines.

**M4. Test coverage**
- *Measured by:* the coverage tool of the repository ; *proposal:* Vitest coverage for the frontend and the backend language's standard tool.
- *Value:* line coverage as reported by the tool, for the project's own source code, excluding generated code, third-party code, test files and configuration files (the exclusion list must be recorded).
- *Pass:* ≥ 70% for the combined result, and each part (frontend, backend) is also reported. Whether each part must individually reach 70% is a planning decision to confirm.

**M5. Violation/event recording rate**
- *Eligible test events:* defined **before** each run as a written list: event type, scripted time, participant. An event is eligible if the client is online and the page is open at the time of generation. Events deliberately generated while the browser is closed or while detection is disabled (SEC-007, SEC-009) are not eligible and are reported separately.
- *Denominator `G`:* number of eligible events deliberately generated.
- *Numerator `P`:* number of those events found as persisted records, matched by event type, participant and a time window around the scripted time (*proposed: ±3 s*).
- *Rate:* `P / G × 100%`. Pass: ≥ 90%.
- *Also reported separately:* missing events (`G − P`), duplicate records (persisted records beyond one per eligible event), delayed events (latency above 2 s). A browser event that was detected but not delivered counts as **not recorded**.

**M6. System Usability Scale**
- Standard 10-item SUS. For items 1, 3, 5, 7, 9 the contribution is `score − 1`. For items 2, 4, 6, 8, 10 it is `5 − score`. The sum of contributions is multiplied by 2.5, giving 0 to 100 per participant.
- The reported value is the mean over participants, with the per-role means and the individual scores. Pass: mean ≥ 70. Procedure in Section 8.

---

## 5. Measurement Conditions

Placeholders in brackets must be filled in from the actual environment. Nothing here states the real hardware or deployment.

| # | Item | Specification |
|---|---|---|
| 5.1 | Server hardware | 4 vCPU, 8 ГБ RAM, 50 ГБ SSD |
| 5.2 | Server operating system and runtime | Ubuntu 22.04 LTS; Python 3.11+; Docker 24.0+ and Docker Compose |
| 5.3 | Client machines and browsers | Windows 11 ; browsers: current stable Chrome, Firefox and Edge 120+/122+ (NFR-014 proposal). Each functional and bypass test records the browser used. |
| 5.4 | Network | Standard Ethernet connection; latency and packet loss recorded with a ping or similar check before each run. Optional additional run with added latency (*proposal*: 100 ms) using a network-emulation tool. |
| 5.5 | Backend, database and DOMjudge | FastAPI; DB: PostgreSQL 15+ and Redis 7+; Judge system: DOMjudge 8.2+; DOMjudge judging hosts count 2; same configuration in all compared runs. |
| 5.6 | Time synchronization | All machines (server, database, DOMjudge, load generator, measurement clients) synchronized to the same NTP source before each run; clock offset recorded before and after. Timestamps recorded in UTC with millisecond precision. |
| 5.7 | Duration and repetitions | Warm-up, ramp and hold as in Section 3.3. At least 3 repetitions of each load run (*proposal*); report each run and the range, not only the best. |
| 5.8 | Logging and resource metrics | Application logs with event IDs and timestamps; request logs; response time, error rate; CPU, memory, disk and network of backend, database and DOMjudge hosts, sampled every few seconds with docker stats. |
| 5.9 | Test data and reset | A fixed test contest, fixed problems, test accounts (50 or more student accounts, teacher accounts) created by a script. Between runs: restore the database to the recorded snapshot, clear DOMjudge submissions for the test contest, clear browser state of measurement clients, and restart services if the procedure says so. |
| 5.10 | Build identifier | Commit hash and build date of the frontend, backend and DOMjudge recorded for every run. |

**Keeping the system and the baseline consistent.** Use the same hardware (or the same virtual machine size), the same DOMjudge version and judging host configuration, the same test contest and submissions, the same network, the same load script (steps that the baseline does not have are skipped, not changed), and the same measurement tools, on the same day where possible, in alternating order.

---

## 6. Baseline for Comparison

**Primary baseline.** The same contest workflow on the existing DOMjudge setup **without** the added proctoring functionality, compared with the proctoring-enabled system.

| Dimension | Baseline (DOMjudge only) | Proposed system |
|---|---|---|
| Functionality and submission correctness | Same set of test submissions with known expected verdicts | Same submissions; verdicts must be identical |
| Timer accuracy | DOMjudge's contest clock as displayed | Workspace timer, measured with M1 |
| Response time and resource use | Login, contest page and submission response times, CPU and memory under the same load | Same, measured under M3 load |
| Added latency from proctoring | — | Difference in response time and resource use between the two systems |
| Event detection and recording | Not available (expected to be none) | M2 and M5 |
| Usability | SUS of the DOMjudge interface, if participants are available | SUS of the proposed system (M6) |

**Feasibility and limits.** The comparison is feasible for submission correctness, response time and resource use through DOMjudge's own web interface or API. A direct comparison of the interface or of event recording is not meaningful, because DOMjudge has no proctoring events. Event metrics are therefore measured against the targets only, and the baseline column states "not available".

**Alternative baseline.** If DOMjudge cannot be run in an equivalent, separate configuration, use the pre-proctoring version of the same application (build with proctoring features disabled), under the same conditions.

**Status.** No baseline experiment has been performed.

---

## 7. Test Cases and Expected Results

Status values: `Planned` (not yet implementable or not scheduled), `Not Run` (ready but not run), `Passed`, `Failed`, `Blocked`. All entries are currently `Planned` or `Not Run`. Requirement IDs refer to `docs/06-requirements.md`. "No existing ID" marks a metric without a requirement; see Section 1.4.

| Test ID | Category | Scenario | Preconditions | Steps or procedure | Expected result | Related requirement | Metric or evidence | Status |
|---|---|---|---|---|---|---|---|---|
| UT-001 | Unit | Remaining time calculation | Timer module | Provide server end time and offset; compute remaining time at several instants, including zero and past end | Correct remaining time; never negative | FR-016 | Test log | Planned |
| UT-002 | Unit | Event creation for tab, fullscreen, clipboard | Event modules | Trigger handlers with simulated browser events | Each produces an event with type, timestamp, contest and student IDs; clipboard event has no content | FR-019, FR-020, FR-021, FR-022, NFR-010 | Test log | Planned |
| UT-003 | Unit | Duplicate suppression | Event module | Send identical events within and outside the interval | One record within the interval, two outside | FR-024 | Test log | Planned |
| UT-004 | Unit | Offline buffering | Event queue | Add events while offline, then go online | All events sent in order with original timestamps | NFR-005 | Test log | Planned |
| UT-005 | Unit | Access-control rules | Authorization module | Check student and teacher against protected operations | Student denied teacher operations; teacher allowed | FR-002, NFR-007 | Test log | Planned |
| UT-006 | Unit | Contest status and post-end submission | Contest logic | Evaluate upcoming, ongoing, completed; submit after end | Correct status; submission rejected after end | FR-004, FR-017 | Test log | Planned |
| UT-007 | Unit | Input validation and error handling | Forms and API client | Send empty and malformed input; simulate backend and DOMjudge failures | Validation messages; no crash; clear error shown | FR-001, FR-030 | Test log | Planned |
| UT-008 | Unit | Neutral warning wording | Warning component and text files | Render all warnings; compare against prohibited phrase list | No accusatory wording; contains event, time, "recorded for review" | FR-027, NFR-001 | Test log | Planned |
| UT-009 | Coverage | Measure test coverage | Unit and other automated tests exist | Run tests with coverage provider; apply exclusion list | Coverage report generated | No existing ID | M4: ≥ 70% | Not Run |
| IT-001 | Integration | Login and role routing | Test accounts | Log in as student and teacher; use invalid credentials | Correct start page; invalid credentials rejected | FR-001, FR-002 | HTTP logs | Planned |
| IT-002 | Integration | Contest list and details | Contests in all states in DOMjudge | Open list and details as an assigned and an unassigned student | Assigned student sees contests with status, time, duration; unassigned does not | FR-004, FR-005, FR-046 | Comparison with DOMjudge data | Planned |
| IT-003 | Integration | Rules acknowledgment gate | Contest requiring acknowledgment | Try to enter workspace without and with confirmation | Blocked without, allowed with | FR-006, NFR-011 | Screenshot, log | Planned |
| IT-004 | Integration | Submission to DOMjudge and verdict | Ongoing contest, known test solutions | Submit correct and wrong solutions | Submission appears in DOMjudge; verdict and status shown in the system; history lists fields | FR-012, FR-013, FR-014, FR-046 | DOMjudge records, screenshots | Planned |
| IT-005 | Integration | Timer discrepancy | Ongoing contest, NTP-synced machines | Sample displayed vs reference time every 10 s for the full duration; include reload and a network gap | Max discrepancy ≤ 1 s | FR-016 | M1 | Planned |
| IT-006 | Integration | Event delivery latency | Event generator with synchronized clock | Generate scripted events; compare `t_gen` and `t_persist` | 95th percentile ≤ 2 s | FR-022, FR-036 | M2 | Planned |
| IT-007 | Integration | Event persistence and fields | Recorded events | Inspect stored records | Each has type, time, student, contest, duration where feasible; stored separately from DOMjudge | FR-022, FR-047 | DB export | Planned |
| IT-008 | Integration | Dashboard shows events live | Teacher dashboard open | Generate events from student sessions | Table row, count and feed update without reload | FR-034, FR-036, FR-037 | Screen recording, delay | Planned |
| IT-009 | Integration | Participant timeline and notes | Participant with events and submissions | Open participant details; add and reload a note | Chronological timeline; note persists | FR-039, FR-040 | Screenshot, DB record | Planned |
| IT-010 | Integration | Contest creation | Teacher account | Create a contest with valid and invalid inputs | Contest saved and visible in DOMjudge; validation messages on invalid input | FR-030, FR-031, FR-032 | Records, screenshots | Planned |
| IT-011 | Integration | Results match DOMjudge | Finished contest | Compare results page with DOMjudge standings | Identical ranking, solved count and penalty | FR-043, FR-046 | Comparison table | Planned |
| IT-012 | Integration | No automatic consequence | Student with several events | Check submissions, score and standings after events | Unchanged; no numeric suspicion score shown | NFR-013, FR-042 | Screenshots, records | Planned |
| IT-013 | Integration | Problem statement and editor display | Ongoing contest | Open a problem; switch C++, Python, Java | Statement parts shown; editor highlights each language | FR-008, FR-009 | Screenshots | Planned |
| LT-001 | Load | 50 concurrent participants | Load tool, 50+ test accounts | Run Section 3.3 procedure at 50 VUs | All VUs complete the scenario; error rate within the confirmed limit | No existing ID | M3: ≥ 50 | Planned |
| LT-002 | Load | Event delivery under load | LT-001 running | Generate scripted events from all VUs | 95th percentile latency ≤ 2 s | FR-022 | M2 under load | Planned |
| LT-003 | Load | Recording rate under load | LT-001 running | Compare generated and persisted eligible events | Rate ≥ 90%; missing, duplicate and delayed counted | FR-022 | M5 | Planned |
| LT-004 | Load | Timer under load | LT-001 running | Measurement clients sample timer | Max discrepancy ≤ 1 s | FR-016 | M1 under load | Planned |
| LT-005 | Load | Resource use and baseline comparison | Baseline environment ready | Run the same load (without proctoring steps) on baseline and on the proposed system | Report response time and CPU and memory differences | No existing ID | Section 6 table | Planned |
| SEC-001 to SEC-011 | Security and bypass | As specified in Section 3.4 | See Section 3.4 | See Section 3.4 | See Section 3.4 | SEC-001..006: FR-019 to FR-023, FR-027; SEC-003: NFR-010; SEC-007..009: NFR-009; SEC-008, SEC-010: NFR-007, FR-002; SEC-011: limitation | Logs, screenshots, DB records | Planned |
| SEC-012 | Security and bypass | Event recording rate in browser scenarios | Eligible event list prepared | Repeat SEC-001 to SEC-006 at least 10 times each in the three browsers | Rate ≥ 90% over the eligible set; per-browser rates reported | FR-019 to FR-022 | M5 | Planned |
| SEC-013 | Security and bypass | HTTPS and credential storage inspection | Deployed test environment | Request over plain HTTP; inspect stored password data | HTTP not served; passwords stored as salted hashes | NFR-008 | Configuration and DB inspection | Planned |
| UX-001 | Usability | SUS evaluation, students | Participants recruited, tasks defined | Section 8 procedure | Mean SUS ≥ 70 (combined with UX-002) | NFR-001, NFR-002 | M6 | Planned |
| UX-002 | Usability | SUS evaluation, teachers | Participants recruited, tasks defined | Section 8 procedure | As above | NFR-001, FR-034, FR-039 | M6 | Planned |
| UX-003 | Usability | Warning comprehension | Warning prototype or screens | Ask participants what happened, whether it was recorded, and what to do | Participants state the event, that it is recorded, and the next action | FR-027, NFR-001 | Observation notes | Planned |

**Coverage of mandatory requirements.** Each Must Have requirement of `06-requirements.md` is linked to at least one case above (FR-008 and FR-009 by IT-013, NFR-008 by SEC-013). SEC-001 to SEC-011 are specified one by one in Section 3.4, and the SEC-001..SEC-011 row summarizes them. This coverage is a plan: it must be rechecked when requirement priorities change.

---

## 8. SUS Usability Evaluation Procedure

**Participants.** Students and teachers who represent the intended users. Sample size is **not decided**; this is a planning decision requiring confirmation with the supervisor. Recruit both roles, and report results per role as well as combined. SUS scores from very small samples have wide uncertainty, which will be stated in the report.

**Materials.** The Figma prototype (see `prototype/README.md`) or the implemented application, test accounts, the task sheet, and the standard 10-item SUS questionnaire (five-point agreement scale).

**Tasks**

| Role | Tasks |
|---|---|
| Student | Log in; find the ongoing contest; read the rules and monitoring notice; open a problem; write and submit a solution; find the verdict in the submission history; respond to a fullscreen warning and explain what was recorded. |
| Teacher | Log in; create a contest with problems and proctoring settings; open the monitoring dashboard; find the participant who needs review; open the timeline and add a note; open the results. |

**Procedure.** Explain the study, obtain consent, let the participant complete the tasks without help (recording completion, errors and comments), then administer SUS immediately afterwards. If only the prototype is used, the report states that the result evaluates the design and not the working system.

**Scoring.** As defined in M6: odd items contribute `score − 1`, even items contribute `5 − score`, sum multiplied by 2.5, giving 0 to 100. The target is a mean of at least 70.

**Recording and summarizing.** Responses are recorded on paper or in a form using participant codes (P-01, P-02, ...), not names. A table lists each participant's role, tasks completed, errors, comments and SUS score. The report gives the mean and range, per role and combined.

**Privacy.** No personal data beyond role and an anonymous code is collected. Data is stored only for the diploma project, and participants may withdraw at any time. Comments are quoted without identifying details.

**Use of feedback.** Problems seen in more than one session, or any task that participants could not complete, are listed with the proposed design change, and the affected requirements in `06-requirements.md` are updated.

**Status.** No participant has been recruited and no SUS data exists.

---

## 9. Evidence and Reporting

| Evidence | Content |
|---|---|
| Test execution logs | Command, date, build ID, pass and fail per test case |
| Coverage reports | Tool output and the exclusion list |
| Load-test results | Raw output of each run, summary table, resource graphs or data |
| Timer measurements | Sample pairs (`D`, `R`), maximum, median, p95 |
| Event latency measurements | `t_gen`, `t_persist` per event, statistics |
| Generated vs recorded events | The eligible list, matched records, missing, duplicate and delayed counts |
| Bypass tests | Screenshots, request and response logs, database records, a written outcome per SEC case |
| SUS data | Anonymous responses, calculated scores, summary |
| Environment | Filled-in Section 5 table, dates and build IDs for each run |


---

## 10. Risks, Limitations, and Acceptance Decision

| Risk or limitation | Effect | Handling |
|---|---|---|
| Browser API limits (visibility, focus, fullscreen, clipboard) | Some actions are not seen; behavior differs per browser | Document per browser; state limits in the interface and the report; test in three browsers |
| Event loss from network interruptions | Lower recording rate; delayed events | Client buffering (NFR-005); report missing and delayed events separately |
| Client and server clock differences | Wrong event times and timer drift | Server-assigned or verified timestamps (NFR-009); NTP synchronization in tests |
| Browser and OS differences | Results valid only for tested versions | Record versions; limit claims to tested configurations |
| Simulated users differ from real participants | Load results may not match real behavior | State this in the report; consider a pilot contest |
| Client code can be altered | Events can be suppressed or forged | Server-side checks, heartbeat gaps as a review signal (*proposal*), risk acceptance recorded |
| Misreading events as proof of misconduct | Unfair treatment | Interfaces and reports describe events as review signals; human review only (NFR-013) |
| Small SUS sample | Wide uncertainty | Report the sample size and ranges |

**Acceptance decision.** The system is accepted against this plan only when **all six metrics meet their thresholds** in runs that have real evidence stored under Section 9, with environment records and repeated runs. For each metric the report states the measured value, the number of runs and the pass or fail result. A failed target is reported with its observed value and a corrective action (for example an optimization, a design change, or an explicit risk acceptance approved by the supervisor), and the affected test is repeated after the change. A target is never reported as met without stored evidence.

---

