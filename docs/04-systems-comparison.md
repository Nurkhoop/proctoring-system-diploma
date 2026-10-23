# 4. Existing Systems Comparison

## 4.1 Method

**Systems selected (5).** The selection covers three groups of tools that an organizer might use for an online programming contest or exam:

| Group | System | Why included |
|---|---|---|
| Open-source contest judge | DOMjudge | The judging base the project integrates with |
| Public contest platform | Codeforces (mashups, groups) | Widely used for online contests; shows what an organizer gets without a special tool |
| Commercial assessment platforms with proctoring | HackerRank, CodeSignal, Codility | Closest existing solutions to browser-based proctoring of coding tasks |

**Criteria (9).**

| # | Criterion | What it shows |
|---|---|---|
| C1 | Primary purpose and format | Whether the system targets contests or hiring/assessment |
| C2 | Deployment, license and data control | Self-hosting vs vendor-hosted, open vs proprietary |
| C3 | Automatic judging of submissions | Verdicts on test cases |
| C4 | Browser-level integrity events | Tab/focus loss, copy-paste, full-screen |
| C5 | Camera, screen recording, ID verification | Intrusive proctoring |
| C6 | Code similarity checking | Plagiarism between submissions |
| C7 | Organizer review of violations | Real-time vs post-hoc |
| C8 | Integration and extensibility | API, ability to embed in a course workflow |
| C9 | Fit for a self-hosted educational contest | Overall fit for our target users |

**Evidence rule.** Each cell is supported by a source code from section 4.4. "Not documented in the sources reviewed" means exactly that: the feature was not found in those sources, not that it does not exist.

## 4.2 Comparison table

| Criterion | DOMjudge | Codeforces | HackerRank | CodeSignal | Codility |
|---|---|---|---|---|---|
| **C1 Purpose** | ICPC-style contests, also teaching [D2, D4] | Public rated contests; private contests via mashups and groups [F1] | Technical hiring tests; also used by organizations for assessments [H1] | Hiring and certified assessments [C1, C2] | Hiring assessments and interviews [K1] |
| **C2 Deployment, license, data** | Open source, self-hosted, no per-contestant fees [D3] | Hosted by the platform; mashups cannot be made public and are for training [F1] | Vendor-hosted proprietary SaaS [H1] | Vendor-hosted proprietary [C1] | Vendor-hosted proprietary [K1] |
| **C3 Automatic judging** | Yes: sandboxed judgehosts, ICPC scoring, scoreboard [D2, D3, D4] | Yes: contest platform with automatic verdicts [F1] | Yes: coding assessments [H1] | Yes: score computed automatically, separate from verification [C1] | Yes: automated assessments [K1] |
| **C4 Browser-level events** | Not documented in the sources reviewed | Not documented in the sources reviewed | Copy/paste tracking; tab proctoring (can be enabled); Secure Mode: full-screen, tab-switch alerts, copy/paste blocking, multi-monitor check [H1, H3] | Copy-paste activity is among the fraud signals [C2] | Paste detection, focus loss, timing, copy events, typing patterns; settings are locked after the first candidate is invited [K1, K2] |
| **C5 Camera / screen / ID** | Not documented in the sources reviewed | Not documented in the sources reviewed | Optional: Proctor Mode adds screenshot analysis and webcam anomaly detection [H1, H2] | Camera, microphone and screen sharing plus government photo ID for proctored assessments [C1, C2] | ID verification and visual/video proctoring offered as premium options [K1, K3] |
| **C6 Code similarity** | Not documented in the sources reviewed | Yes: automatic "significantly coincides" notices to participants [F2] | Yes: plagiarism detection in the integrity stack [H1, H4] | Yes: suspicion scores include solution similarity [C2] | Yes: cross-candidate similarity check [K1, K3] |
| **C7 Organizer review** | Not documented in the sources reviewed | Post-contest notices to participants [F2] | Post-test integrity report; tab and copy/paste data reviewed in reports [H2, H3] | Recording reviewed by CodeSignal staff after the test, typically 1-3 business days [C3] | Reviewable record with code timeline and playback after the test [K1] |
| **C8 Integration** | Documented REST API and Contest API specification [D3] | Polygon for problem authoring; no embedding API in sources reviewed [F1] | Not documented in the sources reviewed | Not documented in the sources reviewed | Not documented in the sources reviewed |
| **C9 Fit for self-hosted educational contest** | Strong for judging and control of the contest; no participant-side integrity layer documented | Possible for small private contests, limited organizer control over participants | Hiring-oriented, closed, cannot be self-hosted | Intrusive proctoring, hiring-oriented, closed | Hiring-oriented, closed |

## 4.3 Findings

1. **Judging is solved, behavioral control is not.** DOMjudge is mature and open, but the sources reviewed describe only judging, scoreboard and API. No participant-side integrity layer is documented.
2. **Commercial tools do implement browser events** (tab/focus loss, copy-paste) [H1, K1], and they confirm that these events are useful signals. Their limitations are context: they are built for hiring, proprietary and hosted by the vendor, so a university cannot adapt them or keep all data on its own servers.
3. **Strong proctoring is intrusive.** CodeSignal requires camera, microphone, screen sharing and photo ID [C1], and Codility and HackerRank offer visual proctoring as additional modes [H2, K3]. The literature shows that students worry about privacy and false flags with such tools [2, 3, 4] (numbering of section 3).
4. **Review is mostly after the fact.** In the sources reviewed, organizers see violations in a report after the test [H2, C3, K1]; real-time organizer monitoring was not documented.
5. **Codeforces similarity checks produce disputes.** Participants report receiving coincidence notices and contest them in blogs [F2]; these are anecdotal, but they illustrate the false-positive risk of automatic similarity decisions.

**What this means for the project.** There is room for a tool that (a) keeps DOMjudge as the open, self-hosted judging core, (b) adds browser-level integrity events without camera or ID, (c) shows violations to the teacher in real time, and (d) produces a risk score and evidence log while leaving the decision to a human.

## 4.4 Sources

| Code | Source |
|---|---|
| D1 | DOMjudge official site. https://www.domjudge.org/ |
| D2 | DINAO. DOMjudge hosting page (roles, API, judgehosts). https://dinao.com/en/conteneur/domjudge |
| D3 | Hostinger. DOMjudge application page (self-hosting, no per-contestant fees, REST API and Contest API). https://www.hostinger.com/au/applications/domjudge |
| D4 | T. Meggendorfer. DOMtutor: Automated Autograding for Logic in Computer Science. arXiv:2608.16899, 2026. |
| H1 | HackerRank Knowledge Base. Test Integrity. https://hackerrank-knowledge-base.help.usepylon.com/articles/1079706165 |
| H2 | HackerRank Knowledge Base. Proctor Mode. https://hackerrank-knowledge-base.help.usepylon.com/articles/5663779659 |
| H3 | HackerRank. Tab-Proctoring: What It Catches (and What It Misses). https://www.hackerrank.com/writing/tab-proctoring-what-it-catches-and-what-it-misses |
| H4 | HackerRank. Proctor Mode vs. Secure Mode. https://www.hackerrank.com/writing/proctor-mode-vs-secure-mode-hackerrank-detects-chatgpt-ai-cheats-2025 |
| C1 | CodeSignal Support. How proctoring works. https://support.codesignal.com/hc/en-us/articles/360039872174 |
| C2 | CodeSignal. Cheating and fraud detection FAQ. https://codesignal.com/?p=11259 |
| C3 | CodeSignal. Proctoring and ID verification one-pager (PDF). https://discover.codesignal.com/rs/659-AFH-023/images/CodeSignal-Proctoring-ID-Verification-One-Pager.pdf |
| K1 | Codility. Proctoring & Integrity. https://www.codility.com/1p/codility-proctoring-integrity/ |
| K2 | Codility Support. Assessment Integrity Features. https://support.codility.com/hc/en-us/articles/47800858996497-Assessment-Integrity-Features |
| K3 | Codility Support. Anti-Cheating Methods. https://support.codility.com/hc/en-us/articles/360043319434-Anti-Cheating-Methods |
| F1 | Codeforces. Polygon and mashups (blog entry 11698, quotes the mashup description). https://codeforces.com/blog/entry/11698 |
| F2 | Codeforces. Example of an automatic coincidence notice (user blog, entry 122956). https://codeforces.com/blog/entry/122956 |

All links accessed 23.10.2026.

## 4.5 Limitations of the comparison

- Feature availability depends on plan and configuration; vendor documentation changes frequently.
- Commercial products were evaluated from public documentation, not by hands-on testing.
- Codeforces information on organizer tools comes partly from community discussions; official documentation should be preferred when available.
- Prices and performance under load were not compared.
