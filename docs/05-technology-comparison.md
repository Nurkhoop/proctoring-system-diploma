# 5. Technology Comparison

**Project:** Development of a Proctoring System for Online Programming Contests
**Version:** prob-period-1

## 5.1 Method

Seven key technical decisions were compared, each with two or three reasonable alternatives (the requirement is at least three decisions). Every decision is judged against the requirements of section 6 (FR/NFR identifiers are given in the criteria column), not against general popularity.

Scores are the team's engineering judgement made in October 2026, before implementation: **++** strong fit, **+** adequate, **–** weak or costly for this project. They are not benchmark results. Where a claim comes from documentation, the source is cited in 5.9. Performance claims will be checked by the load test in section 9 (metric M3), not assumed here.

| ID | Decision |
|---|---|
| T1 | Server platform |
| T2 | Persistent data storage |
| T3 | Real-time channel to the teacher |
| T4 | Client application framework |
| T5 | In-browser code editor |
| T6 | Integration method with DOMjudge |
| T7 | Method of participant-side integrity control |

## 5.2 T1 — Server platform

Needs: REST API of at least 25 endpoints with a formal contract (NFR-05), WebSocket support (FR-21), request validation, tests (NFR-04), and work in parallel with the frontend through an agreed contract.

| Criterion | FastAPI (Python) | Django + DRF (Python) | Node.js (NestJS / Express) |
|---|---|---|---|
| WebSocket support | ++ built in through Starlette [R1] | + needs Channels | ++ native |
| Contract-first API (OpenAPI) | ++ OpenAPI generated from typed code | + needs extra package | + needs extra packages |
| Request/response validation | ++ Pydantic models | + serializers | + with libraries |
| Async I/O for many waiting connections | ++ | + (async support is partial) | ++ |
| Fit with the team's backend skills (Python) | ++ | ++ | – |
| Amount of ready-made features (admin, ORM) | + | ++ | + |

**Decision: FastAPI.** The OpenAPI contract is the boundary between the two team members' parts, so generating it from the code removes a manual step. Django offers more ready-made features but they are not needed here; Node.js would add a second language to the team's backend stack.
**Risk accepted:** FastAPI does not validate WebSocket message payloads automatically, so they are validated manually with Pydantic [R2].

## 5.3 T2 — Persistent data storage

Needs: relations between contests, participants, submissions and events (section 7.4), time-ordered event queries for reports (FR-23), flexible event details, transactions.

| Criterion | PostgreSQL | MongoDB | MySQL |
|---|---|---|---|
| Relational integrity (foreign keys, transactions) | ++ | – | ++ |
| Flexible event payloads | ++ JSONB column | ++ documents | + JSON column |
| Reporting queries (joins, aggregation by participant and time) | ++ | + | ++ |
| Migration tooling with the chosen stack (SQLAlchemy, Alembic) | ++ | – | ++ |
| Team familiarity | ++ | + | + |

**Decision: PostgreSQL** for persistent data, **plus Redis** for short-lived data (timer cache, publish/subscribe between backend workers, rate limiting). Redis is not an alternative to PostgreSQL here: it holds nothing that must survive a restart. MySQL is a close second; PostgreSQL was preferred for its JSONB support for event details.

## 5.4 T3 — Real-time channel from the server to the teacher

Needs: teacher sees a violation within 2 s (NFR-02), the contest timer remains synchronized (NFR-01), connection loss is survivable.

| Criterion | WebSocket | Server-Sent Events (SSE) | HTTP polling |
|---|---|---|---|
| Delay of event delivery | ++ immediate push | ++ immediate push | – bounded by polling interval; a 2 s target needs polling more often than every 2 s |
| Two-way messages (acknowledgements, heartbeats, timer sync) | ++ | – one-way only | + via separate requests |
| Server load for 50+ clients | + one connection each | + one connection each | – many repeated requests |
| Reconnection handling | + must be written | ++ built into the browser API | ++ trivial |
| Support in the chosen backend | ++ [R1] | + | ++ |

**Decision: WebSocket** for the teacher's live monitoring and for the participant's session channel (heartbeat and timer synchronization). Participant events themselves are sent by ordinary HTTPS requests in batches, so they are not lost when the socket drops. SSE would be sufficient for one-way push to the teacher; WebSocket was chosen because the same technology also covers the two-way participant heartbeat. Polling is kept as the **fallback** if the connection cannot be established.

## 5.5 T4 — Client application framework

Needs: at least 12 screens for two roles (NFR-06), real-time updates, localization into three languages (FR-24), typed code shared with the API contract.

| Criterion | React + TypeScript (Vite) | Vue 3 + TypeScript | Angular |
|---|---|---|---|
| Fit with the frontend developer's plan and skills | ++ | + | + |
| Ready-made editor integrations | ++ | + | + |
| Internationalization libraries (i18next) | ++ | ++ | ++ |
| Learning and setup cost in the available time | + | + | – larger framework |

**Decision: React with TypeScript, built with Vite**, as stated in the supervisor's endorsement. The three options are comparable technically; the deciding factors are the developer's skills and the available editor integrations.

## 5.6 T5 — In-browser code editor

Needs: syntax highlighting for C++, Python, Java (FR-07), control over the clipboard and context menu (FR-17), draft autosave (FR-08).

| Criterion | Monaco Editor | CodeMirror 6 | Plain textarea |
|---|---|---|---|
| Syntax highlighting for the three languages | ++ | ++ | – none |
| Interception of copy, paste, cut and context menu | + needs overriding editor commands | ++ lighter and easier to extend | ++ simple DOM events |
| Familiarity for participants (resembles a desktop IDE) | ++ | + | – |
| Bundle size | – large | ++ small | ++ |

**Decision: Monaco Editor.** It gives participants an interface close to common IDEs. The cost is a larger bundle and extra work to override its clipboard commands; this is a risk for FR-17 and is covered by the violation-capture test (metric M5). CodeMirror 6 is the planned fallback if Monaco's clipboard handling cannot be controlled reliably.

## 5.7 T6 — Integration with DOMjudge

Needs: contests, problems, submissions, verdicts and scoreboard (FR-04, FR-09, FR-10, FR-11) without modifying DOMjudge.

| Criterion | DOMjudge REST API | Direct access to DOMjudge's database | Command-line submit script |
|---|---|---|---|
| Supported, documented interface | ++ [D1] | – internal schema may change | + |
| Does not modify DOMjudge | ++ | – risk of corrupting its data | ++ |
| Full set of operations (read verdicts, scoreboard) | ++ | ++ | – submit only |
| Safe across DOMjudge upgrades | + | – | + |

**Decision: DOMjudge REST API**, accessed only through one backend module (the DOMjudge adapter, section 7). This keeps the integration replaceable and testable.

## 5.8 T7 — Participant-side integrity control

Needs: detect tab or window change, leaving full screen, copy and paste (FR-15 – FR-17) without camera or screen recording (NFR-10), in the participant's ordinary browser.

| Criterion | Browser APIs (Page Visibility, Fullscreen, Clipboard events) | Browser extension | Locked-down desktop application |
|---|---|---|---|
| Installation by participants | ++ none | – must install per browser | – must install per operating system |
| Strength of control | – can detect, cannot guarantee prevention | + wider access to browser | ++ operating-system level; comparable products use it for their strictest mode [H5] |
| Privacy impact | ++ low | + | – high |
| Development effort for a two-person project | ++ | – | – |
| Works without administrator rights | ++ | + | – |

**Decision: browser APIs**, with the model **"prevention plus recording"**: block what can be blocked, record what cannot, and validate and score events on the server. The Page Visibility API fires when the user switches tabs, minimizes the browser or switches app [R3]. This approach cannot stop a determined user (for example one using a second device); this is a stated limit of the project (section 9.8), and the desktop-application option is listed as a possible future extension.

## 5.9 Summary of decisions

| ID | Chosen | Main reason | Verified by |
|---|---|---|---|
| T1 | FastAPI | OpenAPI contract, WebSocket, Python skills | Contract tests, M8 |
| T2 | PostgreSQL + Redis | Relational integrity, JSONB, fast ephemeral data | Reports, integration tests |
| T3 | WebSocket (HTTPS for events, polling as fallback) | Push within 2 s, two-way channel | M2, M3 |
| T4 | React + TypeScript + Vite | Developer skills, ecosystem | M9 |
| T5 | Monaco Editor | IDE-like experience | M5 |
| T6 | DOMjudge REST API | Supported interface, replaceable | Integration tests |
| T7 | Browser APIs | No installation, low privacy impact | M5, M10 |

Decisions will be recorded as architecture decision records (ADR) in `docs/adr/` when implementation starts, and revisited if measurements contradict the assumptions above.

## 5.10 Sources

| Tag | Source |
|---|---|
| R1 | FastAPI documentation, WebSockets: https://fastapi.tiangolo.com/reference/websockets/ |
| R2 | Note on manual payload validation for FastAPI WebSockets (community documentation copy): https://huggingface.co/spaces/Nomearod/agentbench/blob/main/data/tech_docs/fastapi_websockets.md |
| R3 | MDN, `visibilitychange` event: https://developer.mozilla.org/docs/Web/Events/visibilitychange |
| D1 | DOMjudge overview (REST API): https://dinao.com/en/conteneur/domjudge |
| H5 | HackerRank Desktop App Mode: https://hackerrank-knowledge-base.help.usepylon.com/articles/5973590014 |

Claims in the comparison tables that have no tag (for example the effort of learning Angular, or Django's partial async support) are the team's judgement and should be treated as such.
