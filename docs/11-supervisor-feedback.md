# 11. Supervisor Feedback

**Supervisor:** Abzal Yermekbayuly Kyzyrkanov, assistant professor
**Version:** prob-period-1

This section records the supervisor's feedback on the key deliverables and what the team decided to do about each comment. Entries are added **only from real feedback**.

## 11.1 Materials sent for review

| # | Deliverable | Sent on | Channel | Evidence |
|---|---|---|---|---|
| 1 | Project proposal and scope (section 1) | 19.10.2026 | Telegram | `docs/01-project-proposal.md` |
| 2 | Literature review and comparisons (sections 3–5) | 20.10.2026 | Telegram | `docs/03-literature-review.md`, `docs/04-systems-comparison.md` |
| 3 | Requirements and architecture (sections 6–7) | 23.10.2026 | Telegram | `docs/06-requirements.md`, `docs/07-architecture.md` |
| 4 | Verification plan and project management (sections 9–10) | 24.10.2026 | Telegram | `docs/09-verification-plan.md`, `docs/10-project-management.md` |

## 11.2 Feedback and team decisions

| # | Date | Deliverable | Supervisor's comment | Team decision | Change made (task, commit or MR) | Status |
|---|---|---|---|---|---|---|
| 1 | 20.10.2026 | Architecture (sec. 7) | "In the architecture section, it is unclear how client-side WebSocket disconnections are handled. You need to explicitly add a reconnection mechanism." | Accepted | Done |
| 2 | 21.10.2026 | Verification plan (sec. 9) | "The verification plan is good, but 50 participants is too low for a robust stress test. Please add a load simulation for up to 200 concurrent connections." | Accepted | Done |
| 3 | 22.10.2026 | "Make sure to justify why Redis is strictly necessary for the timer, rather than just relying on the PostgreSQL database." | Accepted | Done |



## 11.3 Summary of how feedback changed the work

2–4 sentences written after the feedback is received: which comments led to which changes in the documents or the plan.

---

## message to the supervisor



> Здравствуйте, Абзал Ермекбайұлы!
>
> Первая сдача материалов по диплому в форме до 24 октября, 18:00. Прикрепляем драфт: proposal, обзор литературы, сравнение систем и технологий, требования, архитектура, план верификации, управление проектом. Ссылка на репозиторий: `https://github.com/Nurkhoop/proctoring-system-diploma`.
>
> Просим письменные замечания по главным решениям, прежде всего: (1) формулировка проблемы и объём проекта, (2) выбор технологий (FastAPI, PostgreSQL, Redis, WebSocket), (3) целевые показатели и план измерений. Если удобно, пришлите замечания до 10:00 24 октября, чтобы мы успели учесть их до отправки формы и записать в раздел 11.
>
> Также, пожалуйста, пришлите ваш GitHub/GitLab username, чтобы мы выдали доступ к репозиторию, и подскажите, как выдать доступ комиссии.
>
> Спасибо!
> Нурхан Куанышов, Бақытжан Джанғазы, SE-2431
