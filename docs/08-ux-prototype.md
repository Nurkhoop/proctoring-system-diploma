# UX Prototype

**Project:** Development of a Proctoring System for Online Programming Contests

## Purpose

This is the UX prototype of the proposed proctoring system. It shows the main screens that students and teachers would use in a DOMjudge-based programming contest environment. The prototype is a visual representation of the proposed system. It is not a working implementation, and the data in it is fictional demo data (the screens are branded "ContestGuard" and "Northbridge University").

## Figma prototype

**[Open the Figma file](https://www.figma.com/design/hIkutC91r7j0Vr8Anh0HPg/Untitled?node-id=0-1)**

```text
https://www.figma.com/design/hIkutC91r7j0Vr8Anh0HPg/Untitled?node-id=0-1
```

> **Manual check required:** public access (viewing without a Figma account) has not been verified. Before submission, open the link in a private browser window while signed out of Figma.

## Repository mockups

The repository also includes six static screen mockups with editable HTML/CSS sources. These are local illustrative assets and are separate from the 21 frames in the Figma file.

| Screen | Preview | Editable source |
|---|---|---|
| Login | [s1-login.png](../prototype/s1-login.png) | [s1-login.html](../prototype/src/s1-login.html) |
| Contest list | [s2-contest-list.png](../prototype/s2-contest-list.png) | [s2-contest-list.html](../prototype/src/s2-contest-list.html) |
| Problem and editor | [s3-problem-editor.png](../prototype/s3-problem-editor.png) | [s3-problem-editor.html](../prototype/src/s3-problem-editor.html) |
| Integrity warning | [s4-warning.png](../prototype/s4-warning.png) | [s4-warning.html](../prototype/src/s4-warning.html) |
| Create contest | [s5-create-contest.png](../prototype/s5-create-contest.png) | [s5-create-contest.html](../prototype/src/s5-create-contest.html) |
| Live monitoring | [s6-monitoring.png](../prototype/s6-monitoring.png) | [s6-monitoring.html](../prototype/src/s6-monitoring.html) |

Shared styles are in [common.css](../prototype/src/common.css).

## Scenarios covered

### Student

| Scenario | Frame |
|---|---|
| Login | `Login` |
| Contest list | `Contest list` |
| Programming task page with code editor and timer | `Programming workspace` |
| Proctoring warning for a browser event (tab change, fullscreen exit) | `Student proctoring activity` (static fullscreen notification and tab-change banner), also `UI kit · Warning banners` and `UI kit · Modals` |

Supporting student frames: `Student dashboard`, `Contest details`.

### Teacher

| Scenario | Frame |
|---|---|
| Contest creation | `Create contest` |
| Contest monitoring dashboard | `Live monitoring` |

Supporting teacher frames: `Teacher dashboard`, `Incident review`, `Contest results`.

### UI kit samples

`UI kit · Badges`, `Buttons`, `Code editor`, `Inputs`, `Modals`, `Navbar`, `Sidebar`, `Tables`, `Timeline`, `Warning banners`.

In total the prototype contains 21 frames: 11 screens and 10 UI kit samples.

## Design notes

- The screens state that proctoring events are review signals, not proof of cheating, and that scores and eligibility are not changed automatically.
- Frames show a "Demo role / static preview" switch. According to the frames, role samples are illustrative and are not authentication controls.
- The UI kit frames are labeled as reusable samples with no prototype behavior.

## Limits of this documentation

- The frame list above was taken from PDF exports of the 21 frames, not from the live Figma file. Clickable prototype connections (links between frames) cannot be seen in the exports and are not documented here.
- The screens are static designs. No code, authentication, DOMjudge connection or event detection is shown to be implemented by them.
