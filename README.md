# Proctoring System for Online Programming Contests

Diploma project: **Development of a Proctoring System for Online Programming Contests**
(Russian: «Разработка системы прокторинга для онлайн-соревнований по программированию»)

> Status: **analysis and design stage** (first submission, tag `prob-period-1`). No runnable application yet; setup instructions will be added when code is available.

## Description

A web system for running online programming contests and exams with integrity control. It is built on top of the open-source judging system [DOMjudge](https://www.domjudge.org/), which checks solutions automatically, and adds what DOMjudge does not provide: a participant client with a code editor, a server-synchronized timer and browser-based integrity control, and a teacher dashboard with real-time monitoring of violations. Components communicate through a REST API (OpenAPI) and WebSocket.

The system has two roles: **student** (participant) and **teacher** (organizer).

## Problem statement

Online programming contests and exams are common in universities and schools, but integrity control is weak. Participants can copy code from external sources, switch to other windows or messengers, and use hints outside the contest rules, while the organizer cannot see what happens on the participant's side and has no reliable data for reviewing disputed cases. Judging systems such as DOMjudge check solutions well but do not control participant behaviour; general-purpose proctoring services do not fit programming contests and are hard to embed. A simple, open tool that universities can deploy and adapt is missing.

## Target users

- teachers and organizers of programming contests, labs and exams held online;
- students and school pupils as participants;
- developers who integrate with the system through the REST API.

## Expected outcomes

- a working web system (participant client, teacher dashboard, backend) integrated with DOMjudge;
- client-side controls: syntax highlighting, copy/paste blocking, full-screen and tab-switch control, timer;
- documented REST API (OpenAPI) for the two roles;
- real-time violation monitoring, incident log and contest reports;
- results of a pilot contest and an explanatory note with technical documentation.

## Team and supervisor

| Name | Group | Role and responsibilities | GitHub |
|---|---|---|---|
| Nurkhan Kuanyshov | SE-2431 | Backend: REST API and data model, roles and authentication, DOMjudge integration, server timer, proctoring event intake, risk score, real-time monitoring, reports, tests and deployment | `Nurkhoop` |
| Bakytzhan Dzhangazy | SE-2431 | Frontend and UX/UI: interfaces for both roles, code editor, client-side proctoring, timer, teacher dashboard, localization, tests | `dzhangazy` |

**Supervisor:** Abzal Yermekbayuly Kyzyrkanov, assistant professor.

## Scope and current status

In scope: participant client and teacher dashboard; backend with REST API and WebSocket; integration with DOMjudge; browser-based integrity control (tab/window switching, full-screen exit, copy/paste/cut, context menu); server timer; risk score; reports.
Out of scope at this stage: camera, microphone and screen recording; automatic punishment of participants; a mobile application; a new judging engine.

| Stage | Status |
|---|---|
| Analysis (proposal, relevance, literature, system and technology comparison) | Submitted for `prob-period-1` |
| Requirements, architecture, prototype, verification plan | Submitted for `prob-period-1` |
| MVP-1 (December 2026), MVP-2 (January–February 2027), testing and pilot (March 2027), defence (May 2027) | Planned |

## Technologies

Backend: Python, FastAPI, Pydantic, SQLAlchemy, Alembic, PostgreSQL, Redis, WebSocket, DOMjudge REST API, OpenAPI, pytest, k6, Docker, GitHub Actions.
Frontend: TypeScript, React, Vite, Monaco Editor, Tailwind CSS, TanStack Query, WebSocket, i18next, Vitest, Playwright, Figma.
Choices are justified in [`docs/05-technology-comparison.md`](docs/05-technology-comparison.md).

## Documentation

| № | Document |
|---|---|
| 1 | [Project proposal](docs/01-project-proposal.md) |
| 2 | [Evidence of relevance](docs/02-evidence-of-relevance.md) |
| 3 | [Literature review](docs/03-literature-review.md) |
| 4 | [Existing system comparison](docs/04-systems-comparison.md) |
| 5 | [Technology comparison](docs/05-technology-comparison.md) |
| 6 | [Requirements specification](docs/06-requirements-specification.md) |
| 7 | [Architecture and diagrams](docs/07-architecture-and-diagrams.md) |
| 8 | [UX / prototype](docs/08-ux-prototype.md) |
| 9 | [Verification and evaluation plan](docs/09-verification-evaluation-plan.md) |
| 10 | [Project management and risks](docs/10-project-management-and-risks.md) |
| 11 | [Supervisor feedback](docs/11-supervisor-feedback.md) |

Diagrams (editable sources and PNG exports): [`diagrams/`](diagrams/).
UI mockups: [`docs/prototype/`](docs/prototype/) · Figma prototype: `<FIGMA_URL>`.
Task tracker (YouTrack, project `DIP`): `<YOUTRACK_PROJECT_URL>`.

## Repository structure

```
.
├── README.md
├── .gitignore
├── docs/          proposal, relevance evidence, literature review, comparisons,
│                  requirements, architecture, prototype, verification plan, management, feedback
├── diagrams/      editable diagram sources (.dot, .mmd) and PNG exports
├── src/           application code by component (backend/, frontend/)
├── tests/         automated tests and supporting materials
└── config/        dependency definitions and configuration examples (placeholder values only)
```

## Working agreement

- Every work item has a YouTrack task `DIP-N`. Branch: `DIP-N-short-name`. Commit message: `DIP-N: what was done`.
- Changes go through a pull request that references the task and describes how the work was checked; the other team member reviews before merging.
- `main` is the reviewed baseline. Submitted versions are tagged (`prob-period-1`, later `pre-defense-1`, `pre-defense-2`, `final-pre-defense`) and tags are never moved.
- No passwords, keys or personal data in the repository; use `config/*.example` files with placeholders.

## Setup, run and test

Not available yet: the application code is planned for MVP-1 (December 2026). This section will describe the Docker Compose setup, how to run the backend and frontend, and how to run the tests.
