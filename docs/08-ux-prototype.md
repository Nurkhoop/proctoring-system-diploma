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
