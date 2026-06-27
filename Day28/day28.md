# Day 28 — Hospital Admission Readiness Simulator

**Project:** `hospital-admission-readiness-simulator.html` — task-first hospital admission coordinator simulation
**Challenge:** #60DayClaudeChallenge — Day 28
**Builds on:** Days 26–27 (Prior Authorization workflow + story simulators)
**Type:** Healthcare operations education tool where the user plays the role of a Hospital Admission Coordinator.

---

## What I built

A single self-contained HTML application that simulates the operational workflow behind admitting a patient to a hospital. The user plays the **Hospital Admission Coordinator**, makes decisions across six operational domains, manages the Prior Authorization branch (Approved / Pending / Denied), reduces four risk types, and works the case toward an admit-ready score that crosses a 90% threshold to trigger the final admission decision.

The simulator is **task-first** — there is no dashboard on initial load. The user is presented with a focused intake form. Only after clicking *Analyze Admission Readiness* does the dashboard appear with status tiles, action buttons, PA branch logic, risk tracking, timeline, care coordination cards, and the readiness score.

---

## The setup screen (Stage 1)

The intake form collects exactly six fields:

1. **Provider (Hospital)** — illustrative training data, defaults to *Lakeside Regional Medical Center*
2. **Attending Physician** — illustrative training data
3. **Diagnosis** — one of: Acute MI, CHF, Pneumonia, Elective Surgery, Hip Fracture
4. **Admission Type** — Inpatient, Observation, Emergency, ICU, or Same-Day Surgery
5. **Prior Authorization Status** — Approved, Pending, Denied, or Not Required
6. **Planned Admission Date**

When Admission Type is set to **Observation**, the simulator surfaces a yellow callout:

> *CMS 2-Midnight Rule applies — different cost-sharing, SNF eligibility, and billing than inpatient. Medicare patients require written MOON (Medicare Outpatient Observation Notice).*

That note carries forward to the dashboard once analysis is triggered.

---

## The dashboard (Stage 2)

### Readiness Score — top right

The score is a weighted composite across six domains:

| Domain | Weight |
|---|---|
| Prior Authorization Status | 25% |
| Clinical Documentation | 20% |
| Physician Orders | 20% |
| Insurance Verification | 15% |
| Consent Forms | 10% |
| Bed Status | 10% |

Initial readiness lands between **30 and 60%** because the actionable domains (consent, bed, documentation, orders, insurance) all start at modest values. The PA domain alone reflects the status the user selected — Approved gives 100, Pending gives 55, Denied gives 15.

**Hard cap:** *Denied PA + ICU admission cannot reach 70% from admin tasks alone.* This enforces a real operational truth — when the payer has denied and the clinical acuity is highest, administrative completion can only do so much.

### Six Initial Status Tiles

Each operational domain renders as a tile with a colored status badge (good / warn / bad) and a short description of what the domain represents. Tiles update live as actions are completed.

### Seven Workflow Actions

Click any of these to advance the score and reduce related risks:

| Action | Improves | Reduces |
|---|---|---|
| Assign Bed | Bed +25 | Bed Risk −30 |
| Verify Insurance | Insurance +25 | Insurance Risk −30 |
| Upload Documentation | Clinical Doc +25 | Documentation Risk −30 |
| Complete Consent | Consent +30 | — |
| Contact Physician | Physician Orders +25 | Clinical Risk −15 |
| Notify Nursing | Bed +5, Consent +5 | Clinical Risk −10 |
| Prepare Patient Arrival | Bed +10, Consent +5 | — |

Once completed, each action turns green with a ✓ DONE badge.

### Prior Authorization Branch

The PA branch panel changes based on the initial PA status:

- **Approved or Not Required** → green note, proceed
- **Pending** → three resolution paths (Follow Up with Payer, Upload Additional Docs, Contact Physician for Letter of Necessity) — each one accelerates the pending PA score
- **Denied** → three resolution paths (Review Denial Reason, Contact Insurance, **Submit Appeal Expedited**) — the appeal path successfully overturns the denial and converts the PA to Approved

This matches real US healthcare workflow where most denied PAs are appealable and clinically defensible appeals frequently succeed.

### Acute MI / CHF criteria note

When the diagnosis is Acute MI or CHF, the dashboard surfaces a navy callout:

> *InterQual / Milliman thresholds apply — ensure documentation meets medical necessity standards before Utilization Review.*

These two diagnoses are the most-scrutinized inpatient admissions by Utilization Review teams in real hospitals because they sit on the boundary between observation and inpatient.

### Four Risk Types

| Risk | Initial | Higher For |
|---|---|---|
| Documentation | 70 | Pending PA |
| Insurance | 60 | Denied PA |
| Bed | 60 | — |
| Clinical | 50 | Acute MI, CHF, ICU |

Risks decrease as the user completes workflow actions. Real ops teams track these four dimensions exactly because each one has a different escalation path (Doc → physician follow-up, Insurance → payer call, Bed → nursing leadership, Clinical → UR + attending).

### Care Coordination — Five Role Cards

Each card represents a role responsible for one piece of the admission:

1. **Attending Physician** — owns clinical orders and admission justification
2. **Case Manager (Sarah Mendoza, RN)** — length-of-stay, payer touchpoints, post-acute planning
3. **Nursing (Floor 4B Charge Nurse)** — bed prep, intake, monitoring orders
4. **Utilization Review (Lakeside UR Team)** — **concurrent review against payer policy, flags denial risk early, uses InterQual and Milliman criteria**
5. **Discharge Planner (Maya Choi, MSW)** — SNF placement, home health, transportation

The Utilization Review card is the most operationally important one because it explicitly names the standards (InterQual, Milliman) and the function (concurrent review, denial-risk identification) that real UR teams perform.

### Timeline — Nine Milestones

Renders as a horizontal stepper showing where the case currently sits across:

PA Review → Insurance Verification → Bed Assignment → Documentation → Consent → Patient Arrival → Registration → Clinical Assessment → Admission Complete

Each milestone shows as Complete (green), In Progress (navy), or Pending (gray) based on which underlying domain scores have hit their thresholds.

### Governance Snapshot — appears at Readiness ≥ 75%

When readiness crosses 75%, a new card unlocks showing three industry-benchmark estimates:

- **PA Turnaround: 3–5 days** (typical payer routine cycle)
- **Inpatient Denial Rate: ~8–10%** (industry estimate, CMS)
- **PA Rework Cost: ~$11/transaction** (industry estimate, CAQH)

Explicitly labeled as *estimates only — directional, not patient-specific.* These give the user system-level context that operations leaders actually track.

### Final Admission Decision — at Readiness ≥ 90%

When readiness hits 90%, the simulator declares **ADMIT — All Systems Ready** with confetti and a summary card. Below 90%, a yellow **NOT READY** state lists remaining actions and remaining high-severity risks until the user closes the gap.

---

## What this exercise demonstrates

### 1. Task-first UI is more honest than dashboard-first UI

Most simulators dump every metric on the screen at once. A real coordinator's day starts with a single intake task — they don't see the dashboard until they've collected the basics. Mirroring that flow teaches the work pattern, not just the metrics.

### 2. Weighted scoring with a hard cap teaches an operational truth

The 25/20/20/15/10/10 weighting reflects real admission priorities — PA status and clinical documentation matter most, bed assignment matters least. The hard cap on Denied PA + ICU at 69% encodes the reality that some combinations of payer status and clinical acuity cannot be solved by administrative throughput alone. The user has to resolve the PA before they can admit.

### 3. Conditional UI based on diagnosis and admission type is what makes the simulator credible

Observation triggers the CMS 2-Midnight Rule note. Acute MI and CHF trigger the InterQual/Milliman criteria note. ICU + Denied PA caps the readiness ceiling. These conditional behaviors are what separate a generic admission simulator from one that healthcare administration students could actually use as a training tool.

### 4. The PA branch is the heart of the simulator

Pending → Follow Up / Upload Docs / Contact Physician. Denied → Review Reason / Contact Insurance / **Submit Appeal**. Each path mirrors the real options an admission coordinator has in front of a payer portal. Successful appeals converting Denied → Approved is the single highest-value action the user can take.

---

## Tech notes

- **Single HTML file.** ~42 KB. All CSS via Tailwind CDN. All logic in vanilla JavaScript.
- **Task-first stage management.** Setup stage active by default; dashboard hidden until *Analyze* is clicked. Stage switching via CSS class toggling, animated entrance.
- **Live state machine.** Domain scores, risks, action completion, PA branch state, governance visibility, and final decision all managed in a single `state` object that's the source of truth for every render.
- **Weighted readiness math.** Six domains × six weights = composite score. Hard cap applied for Denied PA + ICU. Score band thresholds drive UI visibility (75 → governance, 90 → final decision + confetti).
- **Conditional callouts.** CMS 2-Midnight Rule appears for Observation; InterQual/Milliman appears for Acute MI/CHF; both notes carry forward from setup to dashboard.
- **Pure-CSS confetti** on admit success. No library.
- **Toast notifications** confirm each PA branch action without breaking the flow.
- **Restart button** clears all state and returns to the setup stage.
- **All names labeled as illustrative training data** at the bottom of both stages and at the top of the setup form.

---

## Key learnings

**1. Task-first beats dashboard-first for operational simulations.**
Showing the dashboard immediately hides the *work*. Forcing the user through an intake form first means they've done the same thing a real coordinator does — which makes the dashboard feel earned, not assumed.

**2. The hardest UI decision was the hard cap.**
Capping readiness at 69% when PA is Denied and Admission Type is ICU is the difference between "this is a game where you grind to 100" and "this is a simulator where you have to resolve a real blocker." The cap teaches that not all problems can be solved by completing more checkboxes.

**3. Real terminology beats generic terminology every time.**
*Utilization Review.* *Concurrent review.* *InterQual.* *Milliman.* *MOON notification.* *Letter of Medical Necessity.* These terms cost nothing to include and make the simulator usable in a healthcare-admin classroom. Generic labels would make the same simulator unteachable.

**4. The governance snapshot only reveals at 75% — and that timing matters.**
Showing industry benchmarks too early creates noise. Showing them as a reward for working the case to ≥75% teaches the user that benchmarks are context for an already-functional process, not a substitute for one.

**5. Confetti is a reward function.**
Triggering it only at the 90% admit threshold makes the threshold feel real. If it triggered on every action, it would feel empty. Restraint on celebrations is part of what gives the simulator stakes.

---

## What surprised me most

The "Denied PA + ICU" hard cap turned out to be the most teaching-rich design choice in the entire build. Without it, a user could grind workflow actions and hit 100% readiness even when the payer had denied authorization. That would teach the wrong lesson. With it, the user is forced to use the Appeal path in the PA Branch panel — and watching that single action lift the score from 69% to 95% is the moment the simulator clicks. The teaching wasn't in any single button. It was in the *requirement to take the right one.*

---

## Narrative continuity with Days 26 and 27

- **Day 26** (Prior Authorization Workflow Simulator) — drag-and-drop across three swim lanes, probabilistic outcomes, the user *operates* the workflow.
- **Day 27** (Prior Authorization Story Simulator) — 8-chapter conversation between a patient and an operations specialist, the user *experiences* the workflow from inside.
- **Day 28** (Hospital Admission Readiness Simulator) — the user plays the role of the *Hospital Admission Coordinator* themselves, with operational decisions, risk tracking, and a weighted readiness score.

Three consecutive days on the same domain (US healthcare admissions and PA workflow) using three completely different educational modalities — operate, experience, *be the operator*. That stacking is itself the lesson.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
