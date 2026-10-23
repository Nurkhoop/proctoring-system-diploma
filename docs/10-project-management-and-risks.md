# 10. Project Management and Risks

**Project:** Development of a Proctoring System for Online Programming Contests
**Team:** Nurkhan Kuanyshov, Bakytzhan Dzhangazy (group SE-2431)
**Supervisor:** Abzal Yermekbayuly Kyzyrkanov, assistant professor
**Period:** October 2026 – May 2027

## 10.1 Git repository

| Item | Value |
|---|---|
| Repository | https://github.com/Nurkhoop/proctoring-system-diploma |
| Tag for this submission | `prob-period-1` |
| Visibility and access | Team members and supervisor have access; committee access granted before submission |
| Main branch | `main` is the reviewed baseline |

**Repository structure**

| Folder / file | Content |
|---|---|
| `README.md` | Title, problem, users, team and roles, scope, status, technologies, links, setup instructions when code is runnable |
| `docs/` | Proposal, evidence of relevance, literature review, system and technology comparisons, requirements, architectural decisions, verification plan, management and risks, supervisor feedback |
| `diagrams/` | Editable diagram sources and readable exports |
| `src/` | Application code by component (backend, frontend) |
| `tests/` | Automated tests and supporting materials |
| `config/` | Dependency definitions and configuration examples with placeholder values |
| `.gitignore` | Excludes secrets, keys and personal data |

**Working rules**

- Every meaningful work item has a YouTrack task. Branch name contains the task ID, for example `DIP-23-database-schema`; commit messages start with it, for example `DIP-23: Add initial database schema and ER diagram`.
- Changes go through a pull request that explains the change, references the task and describes how the work was checked. For this team project the other team member reviews before merging.
- Each student commits from their own account with identifiable author details; no shared accounts.
- Passwords, API keys and personal data are never committed; configuration examples use placeholders.
- Submitted tags are never moved or replaced. Later submissions get their own tags (`pre-defense-1`, `pre-defense-2`, `final-pre-defense`).

## 10.2 YouTrack project

| Item | Value |
|---|---|
| Project | Proctoring Diploma, key `DIP` |
| URL | https://proctoring-diploma.youtrack.cloud/projects/DIP |
| Template | Kanban, with Assignee, Priority, Stage and Due date fields |
| Members | Both students and the supervisor |

Each task states the responsible student, the expected outcome, acceptance criteria and a deadline. A task is moved to the final stage only when its acceptance criteria are met, and the link to the pull request or commit is added to the task.

## 10.3 Allocation of responsibilities

**Areas of responsibility (from the supervisor's endorsement)**

| Student | Role | Area |
|---|---|---|
| Nurkhan Kuanyshov | Backend: system core | REST API and data model, roles and authentication, DOMjudge integration, server timer, intake and storage of proctoring events, risk score, real-time monitoring, reports, testing and deployment |
| Bakytzhan Dzhangazy | Frontend, UX/UI | Interfaces for both roles, code editor, copy/paste prohibition, full-screen and tab control, timer, teacher dashboard, localization, testing |

The boundary between the two parts is fixed by the REST API contract (OpenAPI).

**Task allocation for the first submission (all due by 24 October 2026)**

| ID | Task | Responsible |
|---|---|---|
| DIP-1 | Project proposal and scope | Nurkhan |
| DIP-2 | Evidence of relevance: interviews | Bakytzhan |
| DIP-3 | Literature review | Bakytzhan |
| DIP-4 | Existing systems comparison | Nurkhan |
| DIP-5 | Technology comparison | Nurkhan |
| DIP-6 | Requirements specification | Bakytzhan |
| DIP-7 | Architecture and diagrams | Nurkhan |
| DIP-8 | UX prototype | Bakytzhan |
| DIP-9 | Verification and evaluation plan | Bakytzhan |
| DIP-10 | Project management and risks | Bakytzhan |
| DIP-11 | Supervisor feedback | Bakytzhan |
| DIP-12 | Repository setup and README | Nurkhan |
| DIP-13 | Consolidated report PDF | Nurkhan |
| DIP-14 | Tag and form submission | Nurkhan |

Joint work covers requirements, overall architecture and the API contract, integration testing, the pilot contest and the defence. Where a section covers both parts (requirements, verification), each student writes the part belonging to their area.

## 10.4 Milestones

| Period | Work | Checkpoint result |
|---|---|---|
| October 2026 | Topic refinement, DOMjudge and analogue analysis, requirements | Requirements and user stories; first submission (`prob-period-1`) |
| November 2026 | Architecture, data model, REST API contract, interface prototype, DOMjudge test stand | Architecture documents, data schema, OpenAPI, prototype, working stand |
| December 2026 | MVP-1: roles, authentication, contests and problems, submission to DOMjudge, code editor | MVP-1 demonstration, first-stage report |
| January – February 2027 | MVP-2: server timer, client proctoring, events and monitoring, teacher dashboard, scoreboard | MVP-2 demonstration, draft of main chapters |
| March 2027 | Testing, load tests, bypass checks, pilot contest | Test reports, pilot participants' feedback |
| April 2027 | Final measurements, results chapter, thesis text, originality check, pre-defence | Full text, pre-defence protocol |
| May 2027 | Revisions after comments, presentation, defence | Admission to defence, working system |

**Supervision:** a weekly team meeting of 45–60 minutes, individual consultations on request, a monthly demonstration of working functionality, and code review as pull requests arrive.

## 10.5 Dependencies

| Dependency | Why it matters | How it is handled |
|---|---|---|
| DOMjudge and its REST API | Judging, verdicts and scoreboard come from it | Test stand deployed early; integration isolated behind one backend module; API version recorded |
| REST API contract between the two parts | Backend and frontend work in parallel | OpenAPI agreed in November 2026 and changed only through reviewed pull requests |
| Browser APIs (Page Visibility, Fullscreen, Clipboard) | Client proctoring relies on them | Behaviour tested per browser; limits documented |
| Interviewees and pilot participants | Evidence of relevance and pilot results | Contacts arranged early; supervisor helps with the pilot group |
| Supervisor review | Feedback is needed for section 11 and for decisions | Draft sent in advance with a stated deadline |
| Hosting for the pilot | Pilot needs a reachable deployment | Docker-based deployment; hosting agreed with the university or the supervisor |
| YouTrack Cloud and Figma | Task evidence and prototype | Free plans; share links checked from an external account |

## 10.6 Risks

Probability and impact are the team's own estimates (L = low, M = medium, H = high).

| ID | Risk | P | I | Mitigation | Owner |
|---|---|---|---|---|---|
| R1 | A browser cannot reliably block all actions, so prevention is incomplete | H | M | Model of prevention plus recording; server-side validation; limits stated in the report | Both |
| R2 | DOMjudge integration is harder than expected or its API differs from the documentation | M | H | Early test stand; integration module with retries and error handling; fallback to a reduced set of integration features | Nurkhan |
| R3 | The timer drifts or breaks after reload or network loss | M | H | Single server time source, client synchronization, recovery tests (M1) | Nurkhan |
| R4 | False accusations: legitimate behaviour is flagged as a violation | M | H | Risk score based on an event log, not a single event; human review by the teacher; false-positive check (M10) | Both |
| R5 | Start-of-contest load exceeds capacity | M | M | Load test with 50 users (M3); caching with Redis; load tuning | Nurkhan |
| R6 | Events are forged or replayed by a participant | M | H | Server-side validation and checks; bypass-resistance tests | Nurkhan |
| R7 | The two parts drift apart because the API contract changes | M | M | OpenAPI as the single source; contract tests; reviewed changes only | Both |
| R8 | Schedule slip in a two-person team | M | H | Weekly meetings; if behind by more than a week, an individual plan with checkpoints every 3–4 days; critical functions first | Both |
| R9 | Not enough interviewees or pilot participants | M | M | Contact people early; use the supervisor's network; report honestly how many discussions took place | Bakytzhan |
| R10 | Personal data of participants is handled improperly | L | H | Record only events needed for the contest; no camera or screen capture; aggregated reporting; participants informed | Both |
| R11 | Committee or supervisor cannot open a link or repository | M | H | Access granted before submission; links checked in a private browser window or from an external account | Nurkhan |
| R12 | Unverified source claims in the literature review or comparison | M | M | Each student checks sources assigned to them; claims without evidence are marked as unverified | Both |

## 10.7 Next steps

1. Complete the evidence of relevance (interviews) and record verified findings separately from assumptions.
2. Receive the supervisor's feedback on the first-submission materials and record the decisions in section 11.
3. Fix the OpenAPI contract and start the DOMjudge test stand (November 2026).
4. Deliver the prototype for review by 5 respondents.
5. Start MVP-1 (December 2026) with every change linked to a YouTrack task and reviewed by the other team member.
