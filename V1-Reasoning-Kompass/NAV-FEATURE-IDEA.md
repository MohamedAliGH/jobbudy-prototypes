# Feature Idea — Flow Navigation (Back) for Prototype V1

**Status:** Spec agreed — decisions locked below. Ready to build next.
**Location:** lives with the prototype — `V1-Reasoning-Kompass/NAV-FEATURE-IDEA.md`. No new path is created for this feature; all work stays inside `V1-Reasoning-Kompass/index.html`.
**Scope:** Prototype V1 „Reasoning-Kompass". One file, no build, no deps.
**Origin:** First moderated user test — testers could not return to a previous screen to re-check what they had done.

---

## 1. Problem (from the test)

The flow is linear: `Welcome → Onboarding → Stelle hinzufügen → Zusammenfassung → Passungscheck → Entscheidung → Abschluss`.

Forward works (each screen has a primary call-to-action). **Going back does not** — it is inconsistent and partial: there is no reliable way to step back one screen and re-read what you just did. In a new UI/flow this raises friction and anxiety.

## 2. Goal

Give every target user a **visible, subtle, predictable navigation toolbar** to step **back one screen at a time** through the flow, so they can review previous screens without losing what they entered — and without the control competing with the task.

Works for all target users (job seekers with mixed digital confidence, incl. screen-reader / keyboard-only).

## 3. Decisions (locked with stakeholder)

1. **No separate Forward control.** The visible Back toolbar + the browser Back button are enough. Forward = the existing primary CTA on each screen. (Avoids aimless back-and-forth that pulls focus off the task.)
2. **Browser / OS Back button is wired** to step back inside the flow (History API), instead of leaving the page — most natural for many users.
3. **One screen back per press; the floor is the job-description screen (`s-add` „Stelle hinzufügen").** Repeated presses walk back step by step down to that screen. **Onboarding and Welcome are NOT reachable by Back.** No multi-screen jumps — the user stays in control, one step at a time.
4. **The Back control is visible on screen** (not hidden behind a gesture). Subtle styling, but always discoverable.
5. **Auto-refresh after editing.** If the user goes back and changes an input (onboarding answer, pasted title/description), the downstream summary / reasons / score **refresh automatically** on return — the user never sees stale results.
6. **Invisible usage instrumentation** — an in-memory per-screen counter of Back usage, with **no visible UI**, so the test can answer "do users over-navigate / lose focus?".
7. **Stepper stays orientation-only** in this version (it shows progress, it is *not* a multi-jump navigator) — this keeps decision 3 ("max one screen back") true.

## 4. Non-goals — what this feature must NOT be / do

- **Not undo/redo of data.** It moves between screens; it does not revert edits, clear inputs, or change decisions.
- **Not a multi-screen jump** and **not a menu/sidebar/all-screens navigator**. One step back at a time.
- **Not reachable into Onboarding or Welcome** via Back (floor = job-description screen).
- **Not URL routing / deep links.** History API is used only to capture the hardware Back press.
- **Not a change to the linear flow logic** or step order.
- **Not touching the A/B research control** („Teststeuerung", Reasoning↔Score, „Eigene Stelle") or how analysis is generated.
- **Not persistent across a page reload** (prototype-grade; in-memory only).
- **Must never cover or overlap** headings, the primary CTA, the job panel, the stepper, or the research control — at any screen width.

## 5. Proposed design

- **A visible, quiet navigation toolbar** at the top-left of the content area (aligned with the brand / left of the stepper), containing a single **`← Zurück`** control. Text/ghost style — secondary to the primary CTA.
- **Behaviour:** one screen back per activation, along the path actually taken, clamped so it never lands on Onboarding/Welcome. On the job-description screen the Back control is hidden/disabled (nothing valid to go back to).
- **Forward** = the existing primary CTA on each screen (unchanged).
- **Browser Back** = same one-step-back behaviour via `history.pushState` / `popstate`.
- **State preserved + auto-refresh:** navigating never wipes entered data; if an upstream input changed, the analysis re-runs automatically so downstream screens are always consistent.
- **Invisible counter:** `navBackCount[screen]++` in memory (no DOM), readable in console for the moderator.
- **Accessibility:** real `<button>`, `aria-label`, keyboard-focusable, visible focus ring, ≥ 44×44px target, respects `prefers-reduced-motion`.
- Stays inside the single `index.html`, no dependencies, consistent with Design Direction 03 „Clear Workspace" and the calm / no-pressure tone.

### Minor defaults (say if you want otherwise)
- Existing per-screen links („Andere Stelle einfügen", „← Zurück zur Einschätzung", „Neue Stelle prüfen") are **kept** alongside the global Back.
- Label is German „Zurück".

## 6. Acceptance criteria

- [ ] **User sign-off (primary gate).** You open the running prototype, click the Back control (and press the browser Back button), step through the flow — **if it works for you, it is accepted.**
- [ ] **Works correctly.** From every screen above the job-description screen, Back steps exactly one screen along the path taken; never lands on Onboarding/Welcome; no dead ends; entered data and generated analysis are preserved; auto-refresh shows no stale results after an edit.
- [ ] **Hides no important detail.** The control overlaps no heading, CTA, job text, stepper, or research control — at desktop **and** mobile (≤ 560 / ≤ 780 / ≤ 880px breakpoints).
- [ ] **Subtle / not overwhelming.** Visually secondary to the primary CTA; the eye still goes to the task first.
- [ ] **Clear & easy to use.** Visible, obvious label/icon; ≥ 44×44px tap target; keyboard-focusable with visible focus; correct ARIA; respects reduced motion.

## 7. Test plan (dev-ready)

**Automated (headless jsdom — same harness already used on this prototype):**
- [ ] Back steps exactly one screen from each eligible screen (full matrix).
- [ ] Back is hidden/disabled on the job-description screen, Onboarding, and Welcome.
- [ ] Repeated Back walks `Entscheidung → Passung → Zusammenfassung → Stelle` and stops there.
- [ ] Branch handled: `Welcome → (skip onboarding) → Stelle` then Back is unavailable (floor).
- [ ] Browser `popstate` performs the same one-step back without unloading the page.
- [ ] Data preserved + auto-refresh: change an onboarding answer or re-paste the job after going back → downstream summary/reasons/score update on return, nothing stale.
- [ ] A/B arms and „Eigene Stelle" mode behave identically after navigation.
- [ ] Back-usage counter increments per screen and exposes no visible UI.

**Manual (you + dev):**
- [ ] Click-through: Back on every screen returns one step; browser Back does the same.
- [ ] Keyboard-only walk-through (Tab/Enter, focus visible).
- [ ] Screen-reader label check on the Back control.
- [ ] Responsive check at the three breakpoints — no overlap/cover.
- [ ] Reduced-motion on — no jarring transitions.

## 8. Research questions to observe during the next test

(The feature is partly an experiment — observations, not pass/fail.)
- Do users use Back to **review**, then continue — or loop without progressing?
- Does the control **distract** from the task, or reduce anxiety?
- Back frequency per screen (from the invisible counter).
- Any user who **fails to find** Back when they clearly want it.

## 9. Appendix — current flow & technical notes

- Screens (IDs): `s-welcome, s-onboarding, s-add, s-summary, s-fit, s-decision, s-finish`.
- Navigation today: a global click handler on `[data-go]` + `show(id)` that toggles `.show`, updates the stepper, scrolls to top, focuses the first heading. **No history stack** — adding a one-step, floored history is the core of this feature.
- Stepper stage map: `s-add/s-summary → Stelle(1)`, `s-fit → Passung(2)`, `s-decision/s-finish → Entscheidung(3)`; Welcome/Onboarding = stage 0 (stepper hidden).
- Floor screen for Back = `s-add` („Stelle hinzufügen" / job description).
