# Interviews with Users and Evidence of Relevance

**Project:** Development of a Proctoring System for Online Programming Contests
**Document:** `docs/02-evidence-of-relevance.md`
**Research period:** 19-23 October 2026


---

## 1. Introduction

The project extends a DOMjudge-based online programming contest environment with a dedicated interface for students and teachers and with proctoring functionality. Proctoring touches sensitive topics: fairness, privacy and trust. Design decisions made without hearing from the people involved are likely to cause friction, for example warnings that confuse students during a timed contest, or dashboards that show teachers too much and tell them too little.

User interviews are a low-cost way to surface these problems before implementation. In this project they are intended to help identify:

- **Student usability problems:** what causes confusion or lost time in a timed contest.
- **Teacher monitoring needs:** what a teacher must see in order to supervise many participants at once.
- **Concerns about privacy and fairness:** what students consider acceptable monitoring and what they fear.
- **Requirements for event recording and incident review:** what must be recorded, how it should be presented, and who decides what an event means.
- **Opportunities to improve the contest experience:** small changes that remove avoidable stress.

## 2. Research Objectives

| ID | Objective |
|---|---|
| RO-1 | Understand what information students need before and during a contest (rules, time, submission status). |
| RO-2 | Explore how students would react to proctoring notifications and what makes a notification understandable and fair. |
| RO-3 | Identify privacy and false-positive concerns related to tab, fullscreen and clipboard monitoring. |
| RO-4 | Understand how teachers currently supervise contests and what makes remote monitoring difficult. |
| RO-5 | Determine what information teachers need to review a recorded event responsibly, without presuming misconduct. |
| RO-6 | Identify what both roles expect from submission history and post-contest results. |

## 3. Research Methodology

**Format.** Semi-structured interviews with a prepared question list. The interviewer could ask follow-up questions and skip questions that had already been answered. Questions were phrased neutrally and did not assume that the participant had experienced a particular problem.

**Participant profiles (fictional).**

- **P01:** a student with some contest experience, focused on clarity and convenience.
- **P02:** a teacher who supervises students and reviews contest results.
- **P03:** a student comfortable with programming, concerned about interruptions, monitoring rules and fairness.

**Duration.** Approximately 10–20 minutes per interview.

**Main topics.** Contest experience and difficulties; understanding of rules; coding workspace content; reactions to proctoring warnings; privacy and fairness; teacher supervision practice; event review; post-contest information.

**Organizing the findings.** Each interview was summarized into short observations. Observations from different participants were grouped into findings (F-xx). For each finding the document records supporting participants, an evidence type, a confidence level, a design implication and open questions. Findings are then linked to provisional requirements in Section 8.

**Limitations of simulated responses.** Because the responses are written by the researcher, they reflect the researcher's expectations and cannot show how real people think or behave. Section 10 covers this in detail.

**Distinction from empirical research.** This is a documentation exercise. It demonstrates how real research would be recorded and traced, but it does not replace that research.

## 4. Participant Overview

| Participant ID | Role | Relevant Experience | Interview Date | Duration | Main Focus |
|---|---|---|---|---|---|
| P01 | Student / Contest Participant | Third-year student; has taken part in a handful of online contests, mostly university-organized | 12 Oct 2026 | 14 min | Clarity, convenience, avoiding confusion under time pressure |
| P02 | Teacher / Contest Organizer | Lecturer in algorithms; has organized in-class and online contests and reviewed results | 14 Oct 2026 | 18 min | Monitoring efficiency, review of evidence, avoiding unfair accusations |
| P03 | Student / Contest Participant | Second-year student; confident programmer; has had connection problems during an online exam | 16 Oct 2026 | 16 min | Privacy, false positives, technical interruptions, clarity of rules |

---

## 5. Interview Protocols and Transcripts

### Interview P01

| Field | Value |
|---|---|
| Interview ID | INT-01 |
| Participant ID | P01 |
| Participant role | Student / Contest Participant |
| Date | 12 October 2026 |
| Duration | 14 minutes |
| Interview objective | Learn what a student needs to see and understand before and during a timed contest, and how they would respond to proctoring notifications (RO-1, RO-2, RO-6). |

**Interview questions and responses**

**Q1. Can you describe your experience with online programming contests?**
> I've done maybe five or six. Mostly the university ones, plus a couple of open contests where I just wanted to practice. I'm not fast, but I usually solve two or three problems.

**Q2. What usually causes the most difficulty during a timed contest?**
> Honestly, losing track of things. I lose track of time, or I forget which problems I've already submitted. Once I thought I'd sent problem B and I hadn't. I only noticed with ten minutes left.

**Q3. How do you understand the contest rules before starting?**
> Usually there's a PDF or a message from the teacher. I skim it. Sometimes I miss things, like whether you can use the internet for documentation. I think a short rules page right before the start would help, as long as it's actually short.

**Q4. What information do you need to see in the coding workspace?**
> The problem statement and the editor side by side. The timer, always. And the status of my last submission, because I keep wondering if it's still being judged or if it's stuck. I'd also like to see which problems I've already solved without opening each one.

**Q5. How would you react if the system detected that you switched browser tabs?**
> Depends how it tells me. If a big red box appears I'd panic a bit, even if I only checked something innocent. If it says plainly "we recorded that you left the contest tab", then I'd just go back.

**Q6. What would make a proctoring warning understandable and fair?**
> Say what happened and when. Say what I should do now. And don't make it sound like I'm being accused. "Recorded" is OK, "cheating detected" is not.

**Q7. What concerns would you have about clipboard or fullscreen monitoring?**
> Fullscreen I don't mind, it's like an exam. Clipboard is less clear to me. I copy my own code sometimes, for example into a scratch file. I don't know if that counts. I wouldn't be against it, I'd just want to know the rule in advance.

**Q8. What should happen if a warning is triggered accidentally?**
> I guess I'd want a quick way to see what was recorded. Maybe a note that says "if this was a mistake, tell your teacher". I wouldn't want to stop the contest to write an explanation.

**Q9. How useful would a submission history be?**
> Very. I'd look at it a lot. Time, verdict, which problem. Also which language, because I switched from Python to C++ once and lost track.

**Q10. Is there anything about online contests you would change?**
> The waiting. Sometimes the verdict takes ages and there's no sign of progress. Even a "queued / judging" label would be better than a blank screen.

**Interviewer notes**
- Participant spoke quickly and practically. Most answers came from one specific past incident (the unsent problem B).
- Showed no strong objection to monitoring; the concern was tone and clarity, not the monitoring itself.
- Clipboard rule was the only area of real uncertainty.

**Preliminary observations**
- Rules should be short and shown immediately before the contest.
- Timer and submission status are the key persistent elements.
- Notification wording matters more to this participant than the technical detail of what is detected.
- Needs a way to see per-problem solved status.

---

### Interview P02

| Field | Value |
|---|---|
| Interview ID | INT-02 |
| Participant ID | P02 |
| Participant role | Teacher / Contest Organizer |
| Date | 14 October 2026 |
| Duration | 18 minutes |
| Interview objective | Understand how a teacher supervises contests today, what they need from a monitoring dashboard, and how they would review events responsibly (RO-4, RO-5, RO-6). |

**Interview questions and responses**

**Q1. How do you currently organize or supervise programming contests?**
> In class, I just walk around. For online ones it's a video call with cameras optional, plus the judging system's scoreboard. I set up the problems in the judge and share a link. Honestly the setup takes longer than the contest.

**Q2. What is difficult about monitoring multiple participants remotely?**
> I can't see what anyone is doing. With thirty students I can't watch thirty screens anyway. Mostly I notice problems after the fact, when two solutions look suspiciously alike. By then it's hard to say anything for sure.

**Q3. What information would you want on a monitoring dashboard?**
> A list of students, who is online, what they've solved, when they last did something. If someone's offline for ten minutes I want to know, because they may have a real problem. I don't need pretty charts. A table I can sort is enough.

**Q4. How should recorded events be presented?**
> Short and in order. "14:32, left fullscreen, back after 8 seconds." Not a score. If it says "risk 82%" I'd not know what to do with that, and a colleague might treat it as proof.

**Q5. How do you distinguish a suspicious event from an ordinary technical issue?**
> Context. One tab switch means nothing; people check the clock. A student who switches many times right before a correct submission, that deserves a conversation. But even then it's a conversation, not a verdict. Also, connection drops: if the log shows a disconnect around the same time, that explains a lot.

**Q6. What information should be available when reviewing a participant?**
> The event list, their submissions with times, and what they were working on. And whether they were warned. If they were warned three times and kept going, that's different from never being warned.

**Q7. Would review notes or an event timeline be useful? Why?**
> The timeline, yes. Notes, I think yes, but only if they're quick. I'd write "spoke with student, was checking the contest clock" and I'd want that to stay attached. Otherwise I'd forget in a week. I'm less sure whether the student should see my notes.

**Q8. What information is needed after a contest ends?**
> Results with who solved what and the penalty times, obviously. And a list of the flagged participants, so I can review those without hunting through everyone. I'd export the results for grading; not sure how much of the event data I'd need in the export.

**Q9. What should the system avoid doing automatically?**
> Disqualifying anyone, or blocking submissions because of an event. Also sending anything to the student's department. It can warn the student, fine, but any decision is mine.

**Q10. What would make this tool useful in practice?**
> If it saves me time. If I open it and in thirty seconds see who needs attention, that's useful. If I have to click through every student, I'll go back to the video call. I'm also unsure whether I'd want live alerts. It might be distracting during the contest, I'd probably prefer to glance at it when I choose.

**Interviewer notes**
- Participant was cautious about anything resembling automated judgment and returned to the point several times.
- Was undecided on live alerts versus periodic review, and said so openly.
- Mentioned colleagues treating numeric scores as proof, as a general worry, with no specific case given.

**Preliminary observations**
- Dashboard should be a sortable table with minimal decoration.
- Events should be shown as a plain chronological list, with no numeric suspicion score.
- Disconnection information is needed to interpret events.
- Review notes are wanted, but who may read them is unresolved.
- Live alert preference is unresolved.

---

### Interview P03

| Field | Value |
|---|---|
| Interview ID | INT-03 |
| Participant ID | P03 |
| Participant role | Student / Contest Participant |
| Date | 16 October 2026 |
| Duration | 16 minutes |
| Interview objective | Explore student concerns about privacy, false positives and technical interruptions, and what explanation of monitoring they would consider adequate (RO-2, RO-3, RO-6). |

**Interview questions and responses**

**Q1. Can you describe your experience with online programming contests?**
> A few, mostly through the faculty. I like them, they're a good way to practice. I also had an online exam last year where my connection dropped, so I'm a bit sensitive about this topic.

**Q2. What usually causes the most difficulty during a timed contest?**
> Besides the problems themselves? Technical stuff. Connection, the page freezing, not knowing whether my submission went through. When the clock is running you can't afford to wonder.

**Q3. How do you understand the contest rules before starting?**
> Mostly I don't read them properly, I'll admit. If it's a long text I skip. But with monitoring, I'd actually read it, because I want to know what exactly is recorded. Is it only that I left the tab, or the contents of the clipboard, or what? That difference matters to me.

**Q4. What information do you need to see in the coding workspace?**
> Timer, statement, editor, results. And a way to know I'm connected. If the connection drops I want it to say so clearly, so I don't keep typing into nothing. Also, saved code. I'd want to know my code is not lost if the page reloads.

**Q5. How would you react if the system detected that you switched browser tabs?**
> It depends on whether it was me. If I clicked away on purpose, fine. But sometimes a notification pops up on my laptop and the window loses focus without me doing anything. That is what worries me. I'd feel annoyed if I got a warning for it.

**Q6. What would make a proctoring warning understandable and fair?**
> Explain exactly what triggered it. And tell me what happens next: does the teacher see it, does it affect my score? If I don't know the consequences I'll assume the worst. Also don't count the same thing five times because the window flickered.

**Q7. What concerns would you have about clipboard or fullscreen monitoring?**
> Clipboard more than fullscreen. I'm fine with "you pasted something". I'm not fine with anything that reads what's inside my clipboard, because it might have personal stuff in it. Also, the problem statement has sample inputs; I'd copy those to test. That must not be treated as suspicious.

**Q8. What should happen if a warning is triggered accidentally?**
> I should be able to tell someone. A button like "this was accidental" would be good, even if it only adds a note for the teacher. And the teacher should see that note next to the event.

**Q9. How useful would a submission history be?**
> Useful, but mainly for reassurance. To prove it was submitted and when. If something goes wrong, that history is my evidence.

**Q10. Is there anything about online contests you would change?**
> Tell us more about what's happening. Rules, monitoring, what's recorded and who sees it. I don't think students mind rules if they're stated honestly.

**Interviewer notes**
- Participant was calm but pointed about privacy, and kept returning to what is stored and who sees it.
- Raised an interesting tension: copying sample input is legitimate but looks like clipboard activity.
- Previous connection-loss experience clearly shaped the answers.

**Preliminary observations**
- The explanation of what is recorded (and what is not) should be stated up front, including that clipboard contents are not read.
- Focus loss can be unintentional, so warnings need wording and possibly a way to flag an accident.
- Connection state and autosave are trust-critical.
- Copying sample data from the statement needs a defined policy.

---

## 6. Consolidated Findings

All findings are **synthetic and unvalidated**. "Confidence" refers only to how consistently the finding appears across the three fictional interviews, not to real-world certainty. No quantitative claims are made.

### F-01 — Clear contest rules before starting
- **Description:** Participants want a short, readable statement of the rules, including what is monitored, before the contest begins.
- **Supporting participants:** P01, P03
- **Relevant response (paraphrase):** P01 wants a short rules page just before the start. P03 said she would actually read the monitoring rules because she wants to know what is recorded.
- **Evidence type:** Synthetic interview statement
- **Confidence:** Medium (two participants, same direction)
- **Design implication:** Show a concise rules and monitoring summary on the contest details screen, with an acknowledgment step before entering the workspace.
- **Unresolved:** How long is "short"? Do students read it, or just tick the box?

### F-02 — Visible timer and submission status
- **Description:** A persistent timer and a clear status for the latest submission reduce uncertainty under time pressure.
- **Supporting participants:** P01, P03
- **Relevant response (paraphrase):** P01 wants the timer always visible and a "queued / judging" label instead of a blank screen. P03 does not want to wonder whether a submission went through.
- **Evidence type:** Synthetic interview statement
- **Confidence:** Medium
- **Design implication:** Keep the timer in the workspace header; show per-submission states (queued, judging, verdict).
- **Unresolved:** Does a prominent timer increase stress for some students?

### F-03 — Transparent proctoring notifications
- **Description:** Notifications should say what was recorded, when, and what the participant should do, in neutral language.
- **Supporting participants:** P01, P03
- **Relevant response (paraphrase):** P01: "recorded" is acceptable, "cheating detected" is not. P03 wants to know exactly what triggered a warning and what happens next.
- **Evidence type:** Synthetic interview statement
- **Confidence:** Medium
- **Design implication:** Warnings with event name, timestamp, a statement that the event is recorded for review, and a recovery action. Severity proportional to the event.
- **Unresolved:** Do students prefer detailed or minimal warnings?

### F-04 — Human review of recorded events
- **Description:** Recorded events should be interpreted by a teacher, not turned into automatic outcomes.
- **Supporting participants:** P02, P03
- **Relevant response (paraphrase):** P02 said any decision is the teacher's. P03 wants to know whether warnings affect the score.
- **Evidence type:** Synthetic interview statement
- **Confidence:** Medium
- **Design implication:** No automatic disqualification or blocking; the review workflow ends with a teacher-set status.
- **Unresolved:** Which review statuses are needed?

### F-05 — Participant activity timeline
- **Description:** Teachers want a chronological, plain list of events per participant, alongside submissions and warnings.
- **Supporting participants:** P02
- **Relevant response (paraphrase):** P02 wants entries such as time, event and duration away, and wants to see whether the student was warned.
- **Evidence type:** Synthetic interview statement (single participant)
- **Confidence:** Low (one participant)
- **Design implication:** A participant detail view combining events, submissions and warnings on one timeline.
- **Unresolved:** Is the same view needed by other teachers? How much history is useful?

### F-06 — Protection against unfair automated conclusions
- **Description:** Both teachers and students are wary of scores or labels that read as proof of misconduct.
- **Supporting participants:** P02, P03
- **Relevant response (paraphrase):** P02 would not know what to do with a risk percentage and worries colleagues might treat it as proof. P03 wants a way to flag accidental events.
- **Evidence type:** Synthetic interview statement
- **Confidence:** Medium
- **Design implication:** Avoid suspicion scores; use neutral labels such as "needs review"; allow student-added context for an event.
- **Unresolved:** Is any attention indicator acceptable, and in what form?

### F-07 — Useful teacher monitoring dashboard
- **Description:** A compact, sortable table showing who is online, progress, recent activity and event counts is valued over decorative visuals.
- **Supporting participants:** P02
- **Relevant response (paraphrase):** P02: a table she can sort is enough; she wants to see who needs attention in thirty seconds. Disconnections matter.
- **Evidence type:** Synthetic interview statement (single participant)
- **Confidence:** Low
- **Design implication:** Participants table with connection status, last activity, event count and filters, rather than charts.
- **Unresolved:** Live alerts or periodic checking? P02 was undecided.

### F-08 — Accessible submission history
- **Description:** Both students and the teacher want to see submissions with time, problem, language and verdict.
- **Supporting participants:** P01, P02, P03
- **Relevant response (paraphrase):** P01 uses history to track what has been sent and in which language. P03 sees it as proof of submission. P02 uses submissions in review.
- **Evidence type:** Synthetic interview statement
- **Confidence:** Medium
- **Design implication:** History available inside the workspace and in teacher review, with the same fields.
- **Unresolved:** How much detail (e.g. test case results) is appropriate during a contest?

### F-09 — Connection state and recovery
- **Description:** Connection drops are a real source of stress and can resemble suspicious activity in the event log.
- **Supporting participants:** P03, P02
- **Relevant response (paraphrase):** P03 wants a clear "connection lost" indicator and confidence that code is saved. P02 said a disconnect near an event explains a lot.
- **Evidence type:** Synthetic interview statement
- **Confidence:** Medium
- **Design implication:** Connection indicator for students; connection events shown in teacher timeline; autosave visible.
- **Unresolved:** How should the system treat events recorded around a disconnect?

### F-10 — Clipboard policy needs to be explicit
- **Description:** Participants differ on clipboard monitoring. One is unsure whether copying own code counts; another accepts "paste detected" but not any reading of clipboard content, and wants copying sample input to be allowed.
- **Supporting participants:** P01, P03
- **Relevant response (paraphrase):** P01 would accept the rule if stated in advance. P03 distinguishes recording an action from reading its contents.
- **Evidence type:** Synthetic interview statement (divergent views)
- **Confidence:** Low (views differ)
- **Design implication:** State what clipboard events are recorded (action only, not content). Decide how copying from the statement is treated.
- **Unresolved:** Is distinguishing in-page sample copying technically and ethically sufficient?

---

## 7. Verified Facts, Synthetic Findings, and Assumptions

### A. Verified Project Facts

> **Source note:** These facts come from the project brief and design task supplied by the project author. They should be cross-checked against the repository's requirements specification and architecture documents when finalizing this document, and any difference resolved in favor of the repository.

- The project title is *Development of a Proctoring System for Online Programming Contests*.
- The system is a web-based platform built on top of DOMjudge, which handles contest administration, problem statements, submission processing, automated judging, verdicts and standings.
- The system adds a dedicated user experience for students and teachers, and proctoring functionality.
- There are two primary roles: Student / Contest Participant and Teacher / Contest Organizer.
- Students can log in, view contests, join a contest, read problems, write code with syntax highlighting, select a language, submit, view history and verdicts, see remaining time and receive warnings.
- Teachers can log in, create and configure contests, view participants, monitor them, see proctoring events in real time, identify participants who may need attention, and review incidents and results.
- Events to be detected and recorded include tab or window switching, leaving fullscreen, and copy or paste attempts, plus other events defined by the project requirements.
- The approach is prevention plus detection. Browser-based restrictions are not presented as foolproof; events are recorded and reviewed.
- The monitoring interface should support informed review and must not present a suspicion score as proof of cheating.
- Out of scope: AI-based face recognition, webcam recording and biometric analysis.
- The planned frontend stack is React, TypeScript, Vite, Tailwind CSS, Monaco Editor, TanStack Query, WebSocket and i18next.

### B. Synthetic Interview Findings

**These are synthetic and unvalidated.** Patterns across INT-01 to INT-03:

- Students (P01, P03) wanted short, clear rules and plain-language, non-accusatory warnings that say what happened and what to do next.
- Students wanted the timer, submission status, autosave and connection state to be clearly visible.
- The teacher (P02) wanted a compact table and plain chronological events, with decisions left to a human.
- Teacher and student both distrusted automated labels or scores.
- Clipboard monitoring was the least settled topic.
- Submission history was valued by all three, for different reasons.

### C. Assumptions Requiring Validation

| # | Assumption | How it could be validated |
|---|---|---|
| A-1 | Students prefer a short, specific warning over a detailed one. | A/B test of warning wording in a usability session; post-task questions. |
| A-2 | Teachers prefer checking a dashboard on demand to receiving live alerts. | Interview more teachers; observe a pilot contest; compare alert and no-alert conditions. |
| A-3 | Tab switches, fullscreen exits and clipboard actions are the most useful event types for review. | Review recorded events from a pilot contest with teachers and ask which they actually used. |
| A-4 | A timeline view gives enough detail for incident review. | Task-based testing: ask teachers to review a staged incident and note what they miss. |
| A-5 | Students accept recording of clipboard actions if the rule is stated in advance. | Survey and interviews with a larger sample; ask separately about actions versus contents. |
| A-6 | Students should be able to add context to an event. | Test a prototype of the "add context" action; ask teachers whether the notes affect their judgement. |
| A-7 | Teachers need notes that are private to teachers. | Ask several teachers and students about note visibility. |
| A-8 | A prominent timer does not increase stress to a harmful degree. | Usability testing with a timed task and a short stress questionnaire. |

---

## 8. Requirements Traceability Matrix

The IDs are **provisional**. If the repository contains a requirements specification, its IDs should replace these and the matrix should be updated. Validation status labels: `Supported by project scope`, `Suggested by synthetic research`, `Requires real-user validation`.

| Finding ID | Evidence Type | User Need | Requirement ID | Proposed Requirement | Priority | Validation Status |
|---|---|---|---|---|---|---|
| — | Project scope | Access the platform with the correct role | FR-01 — Authentication | Users log in and are directed to the student or teacher interface. | High | Supported by project scope |
| — | Project scope | Find contests by state | FR-02 — Contest List | Show available, ongoing, upcoming and completed contests with start time and duration. | High | Supported by project scope |
| F-01 | Synthetic interview + project scope | Know the rules and monitoring before starting | FR-03 — Contest Details and Rules | Show rules and monitored events on the details screen; require acknowledgment before entering. | High | Supported by project scope; acknowledgment step suggested by synthetic research |
| F-02, F-09 | Synthetic interview + project scope | Stay oriented under time pressure | FR-04 — Programming Workspace | Provide statement and editor, language selection, persistent timer, autosave indicator and connection indicator. | High | Supported by project scope; indicators suggested by synthetic research |
| F-02 | Synthetic interview + project scope | Know whether a submission was received | FR-05 — Code Execution and Submission | Run and submit code; show queued, judging and final verdict states. | High | Supported by project scope; intermediate states suggested by synthetic research |
| F-08 | Synthetic interview + project scope | Track what was submitted | FR-06 — Submission History and Results | List submissions with problem, language, time, verdict, runtime and memory. | High | Supported by project scope |
| F-03, F-06 | Synthetic interview + project scope | Understand a warning without feeling accused | FR-07 — Proctoring Notifications | Show event, timestamp, "recorded for review" wording and a recovery action; severity proportional to the event. | High | Supported by project scope; wording suggested by synthetic research |
| F-06, F-10 | Synthetic interview | Add context to an accidental event | FR-07a (proposed extension of FR-07) | Allow the participant to add a short note to a recorded event. | Medium | Requires real-user validation |
| F-09, F-10 | Synthetic interview + project scope | Reliable event data | FR-08 — Proctoring Event Recording | Record event type, time and duration; include connection state events; record clipboard actions without clipboard contents. | High | Supported by project scope; content exclusion and connection events suggested by synthetic research |
| F-07 | Synthetic interview + project scope | See who needs attention quickly | FR-09 — Teacher Monitoring Dashboard | Sortable participants table with progress, connection status, last activity, event count and filters. | High | Supported by project scope; layout details require real-user validation |
| F-05, F-04 | Synthetic interview + project scope | Review one participant with context | FR-10 — Participant Event Review | Timeline of events, warnings and submissions; review notes and a status set by the teacher. | High | Supported by project scope; timeline suggested by synthetic research |
| F-08 | Synthetic interview + project scope | Grade and conclude | FR-11 — Contest Results | Scoreboard with solved problems and penalty time; list of participants flagged for review. | Medium | Supported by project scope; flagged list suggested by synthetic research |
| F-01, F-02, F-03 | Synthetic interview | Clear interface under pressure | NFR-01 — Usability and Clarity | Plain language, consistent status colors, minimal distraction in the workspace. | High | Suggested by synthetic research; requires real-user validation |
| F-04, F-06 | Synthetic interview + project scope | Fair handling of events | NFR-02 — Fairness and Human Review | No automatic penalties or numeric suspicion scores; events are labelled as indicators for human review. | High | Supported by project scope |
| F-03, F-10 | Synthetic interview | Know what is stored and who sees it | NFR-03 — Privacy and Transparency | Publish what is recorded, what is not, and who can access it. | High | Suggested by synthetic research; requires real-user validation |
| F-07 | Assumption A-2 | Choice of live alerts or periodic review | FR-09 (open item) | Decide whether real-time alerts are needed in addition to the table. | Low | Requires real-user validation |

---

## 9. UX and Product Recommendations

Source labels: **[PS]** project scope, **[SF]** synthetic finding, **[AV]** assumption requiring validation.

### Student interface

| Recommendation | Source |
|---|---|
| Show a rules and monitoring summary on the contest details screen; require acknowledgment before entering the workspace. | [PS] rules and monitoring; [SF] F-01 for the acknowledgment step |
| Keep a countdown timer visible in the workspace header. | [PS] remaining time; [SF] F-02 |
| Show submission states (queued, judging, verdict) and a per-problem solved indicator. | [SF] F-02, F-08 |
| Use proportionate, neutral warnings: event, time, "recorded for review", what to do. | [PS] warnings; [SF] F-03 |
| Include a plain explanation of monitored events, including that clipboard contents are not read. | [PS] event types; [SF] F-10; the content-exclusion rule requires [AV] A-5 |
| Make clear that a recorded event is not an accusation or a decision. | [PS] human review; [SF] F-04, F-06 |
| Offer recovery guidance (return to fullscreen, an optional "add context" note). | [SF] F-06; [AV] A-6 |
| Show autosave and connection indicators. | [SF] F-09 |

### Teacher interface

| Recommendation | Source |
|---|---|
| Concise participants table with progress, last submission, connection, event count and last activity. | [PS] participant monitoring; [SF] F-07 |
| Severity labels that describe the event, not the person. | [PS] no suspicion score as proof; [SF] F-06 |
| Filtering and sorting (e.g. participants with incidents, offline participants). | [PS] identify participants needing attention; [SF] F-07 |
| Participant-specific timeline combining events, warnings and submissions. | [PS] incident review; [SF] F-05 |
| Review notes attached to events; decision on note visibility pending. | [PS] review; [SF] F-05; visibility is [AV] A-7 |
| Access to submissions and contest info from the review screen. | [PS] results and submissions; [SF] F-08 |
| A neutral review process: statuses such as "not reviewed", "reviewed – no concern", "follow up". | [PS] human review; [SF] F-04; exact statuses are [AV] |
| Optional live alerts. | [AV] A-2 |

---

## 10. Research Limitations

- The interviews are fictional and written by the researcher.
- No real participants were recruited, and no real conversations or observations took place.
- The responses reflect the researcher's expectations and may emphasize issues the researcher already considered important.
- With three fictional participants, no claim about how common any view is, or about statistical significance, is possible.
- Real students and teachers may raise different priorities, such as concerns not anticipated here, or may disagree with these.
- Only one teacher perspective is represented, and there are no administrators or other staff.

**Why the document is still useful.** It works as a prototype for research documentation: it fixes a protocol, a findings format and a traceability method that can be applied unchanged to real data. It also records design hypotheses (Section 7C) and links them to requirements, so that real research can be targeted at the points of highest uncertainty.

## 11. Conclusion

The simulated interviews suggest that the system should concentrate on clarity, transparency, usability, teacher oversight and fair handling of events. For students, this means short, honest rules, visible time and submission status, and warnings that explain without accusing. For teachers, it means a compact monitoring view and a timeline that supports judgement, with the decision left to a person. These are design directions and hypotheses. The synthetic interviews do not prove the system's relevance, which needs to be established through real research.


