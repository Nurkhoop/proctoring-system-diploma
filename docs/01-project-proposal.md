# 1. Project Proposal

## 1.1 Project title

**Development of a Proctoring System for Online Programming Contests**

## 1.2 Team and supervisor

| Member | Group | Role |
|---|---|---|
| Nurkhan Kuanyshov | SE-2431 | Backend: core system, DOMjudge integration, server-side timer, proctoring events |
| Bakytzhan Dzhangazy | SE-2431 | Frontend and UX/UI: client application, code editor, client-side proctoring, teacher dashboard |

**Supervisor:** Abzal Ermekbayuly Kyzyrkanov, assistant professor, School of Software Engineering.

**Project period:** October 2026 to May 2027.

## 1.3 Problem statement

Online contests, lab works and exams in programming have become routine for universities and schools, but control of participant integrity remains a weak point. A participant can copy ready-made code from external sources, switch between windows and messengers, or use hints outside the contest rules. The teacher does not see what happens on the participant's side and lacks reliable data to resolve disputed cases.

Existing solutions cover the problem only partially:

- Judging systems such as DOMjudge verify solutions and maintain the scoreboard, but are oriented to ICPC-style contests and are not designed to control participant behaviour on the participant's side.
- General-purpose proctoring services do not account for the specifics of working with code and are hard to embed into a programming contest workflow.
- A university needs an open, easy-to-deploy tool that can be adapted to its own courses.

> Statements about existing systems are verified in section 4 (Existing system comparison); findings from user discussions are in section 2 (Evidence of relevance). Until then, the claims above are working assumptions.

## 1.4 Target users

- **Teachers and organizers** who run programming contests, labs and exams online (role *teacher*).
- **Students and school pupils** who participate in contests (role *student*).
- **Developers** who integrate with the system through the REST API.

## 1.5 Aim

To develop a web-based proctoring system for online programming contests that is integrated with DOMjudge, controls participant integrity on the client side, and gives the teacher real-time monitoring and post-contest reports.

## 1.6 Objectives

1. Analyze DOMjudge (architecture, API, roles, judgehost), existing contest and proctoring systems, and relevant literature; collect and verify requirements.
2. Specify functional and non-functional requirements with priorities and acceptance criteria.
3. Design the architecture, data model and REST API contract (OpenAPI) for the roles *student* and *teacher*.
4. Prototype the interfaces of both roles and check the prototype with users.
5. Implement the backend: REST API, authentication, DOMjudge integration, server-side timer, proctoring event intake, risk scoring, real-time monitoring and reports.
6. Implement the client: code editor with syntax highlighting, copy/paste blocking, fullscreen and tab-switch control, timer, teacher dashboard, localization (ru, kk, en).
7. Test the system (unit, integration, contract, load, bypass checks) and run a pilot contest with a group of students.
8. Evaluate the results against the target metrics and prepare the explanatory note.

## 1.7 Expected outcomes

- A working web system: participant client, teacher dashboard and backend integrated with DOMjudge.
- Client-side control mechanisms: syntax highlighting, copy/paste blocking, fullscreen and tab-switch control, timer.
- Documented REST API (OpenAPI), at least 25 endpoints.
- Client application with at least 12 screens for two roles.
- Real-time violation monitoring, incident log and post-contest reports.
- Results of a pilot contest and an explanatory note with technical documentation.

### Target metrics

| Metric | Target |
|---|---|
| Client/server timer drift | ≤ 1 s |
| Proctoring event delivery to teacher | ≤ 2 s |
| Concurrent participants | ≥ 50 |
| Test coverage of key backend modules | ≥ 70% |
| Share of violations detected in the prepared test scenario | ≥ 90% |
| Usability score (SUS) | ≥ 70 |

## 1.8 Project scope

### In scope

- Roles *student* and *teacher*, JWT authentication.
- Contests, tasks and participants; submission of solutions to DOMjudge; verdicts and scoreboard.
- Code editor (C++, Python, Java) with syntax highlighting, language selection, templates, draft autosave.
- Client-side proctoring: blocking copy, paste, cut and context menu in the editor; fullscreen control; tab and window switch detection; warnings to the participant; events sent to the server.
- Server-side timer: single time source, start, extension, automatic closing of submissions, synchronization with the client, recovery after connection loss.
- Intake, validation and storage of proctoring events; protection against forged events; risk score per participant.
- Teacher side: contest creation, participant list, real-time monitoring (WebSocket), incident log, scoreboard, reports and results export.
- Localization: Russian, Kazakh, English.
- Containerized deployment and CI.

### Out of scope

- Webcam, microphone and screen video recording.
- Replacement or modification of the DOMjudge judging engine.
- Guaranteed prevention of every possible bypass in the browser. The approach is "prevention plus recording" with server-side verification; limitations will be documented.
- Automatic accusations of cheating: the system provides evidence and a risk score, the decision stays with the teacher.

## 1.9 Engineering complexity

- **Reliability of client-side control:** the browser cannot guarantee blocking of all actions, so a model of prevention plus recording with server-side validation and resistance to bypass is required.
- **Server-side timer:** single time source, client synchronization, resilience to connection loss and page reload, correct closing of submissions.
- **Integration with DOMjudge:** mapping of users, contests and tasks, submitting solutions, retrieving verdicts and scoreboard, handling judging system failures.
- **Security:** separation of roles, protection of the API and proctoring events against forgery, protection of participants' personal data.
- **Incident analysis:** risk scoring without false accusations, event log for disputed cases, reports.
- **Load and operations:** simultaneous start and submission by dozens of participants, real-time monitoring, container deployment.

## 1.10 Division of work

The system is split into two contours, each owned by one student. The boundary is fixed by the REST API contract (OpenAPI), so individual contributions can be traced through commit history, tasks and code review. Design of the overall architecture, requirements, integration testing, the pilot contest and the defense are done jointly.

## 1.11 Plan of the next stage

| Period | Work | Result |
|---|---|---|
| Nov 2026 | Architecture, data model, REST API contract, interface prototype, DOMjudge test stand | Architecture documentation, OpenAPI, prototype, working stand |
| Dec 2026 | MVP-1: roles, authentication, contests and tasks, submission to DOMjudge, code editor | Demo of MVP-1 |
| Jan-Feb 2027 | MVP-2: server timer, client proctoring, events and monitoring, teacher dashboard | Demo of MVP-2 |
| Mar 2027 | Testing, load tests, bypass checks, pilot contest | Test reports, pilot feedback |
| Apr-May 2027 | Final measurements, explanatory note, pre-defense, defense | Full text, working system |
