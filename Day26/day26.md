# Day 26 — Prior Authorization Workflow Simulator

**Project:** `prior-auth-simulator.html` — gamified, drag-and-drop healthcare workflow simulator
**Challenge:** #60DayClaudeChallenge — Day 26
**Type:** Interactive business workflow education tool — single HTML file, no backend, no frameworks.

---

## What I built

A working browser-based simulator that teaches the US healthcare Prior Authorization (PA) workflow through interactive, drag-and-drop gameplay. Single HTML file. No frameworks, no CDN, no localStorage. All state lives in JavaScript memory and resets on Restart.

The simulator visualizes the PA workflow as **three swim lanes** — Patient, Provider, Payer — with a **seven-stage journey** moving a case from intake all the way through to the payer's final decision. Users drag the patient case card from stage to stage to advance the workflow.

**Four patient scenarios** modeled with realistic clinical and insurance specifics:
1. Elective Surgery — Right Knee Arthroscopic Meniscectomy (CPT 29881), BlueCare Choice PPO
2. Diagnostic MRI — Lumbar Spine without contrast (CPT 72148), Aetna Standard HMO
3. Specialty Medication — Adalimumab (Humira) for Crohn's Disease, UnitedHealthcare Choice Plus
4. Inpatient Admission — Cardiac Catheterization with possible PCI, Medicare Advantage (Humana)

**Seven workflow stages** the user drags the case through:
1. Patient Intake
2. Provider Consult
3. Medical Necessity Evaluation
4. Assemble PA Request (where the user checks off required and optional documents)
5. Submit to Payer
6. Payer Review (the payer's decision runs automatically here)
7. Outcome — one of Approval, Pend, or Denial

**Five possible payer outcomes** the simulator models:
- **Approval** → confetti animation + workflow summary
- **Pend** → "more information needed," user returns to Assemble stage to add missing docs and resubmit
- **Denial** → option to file an Appeal or request Peer-to-Peer review with a payer medical director
- **Appeal** path — improved approval odds when documentation is complete
- **Peer-to-Peer Review** — same path, treated as an escalated appeal

**Live dashboard with five KPIs** tracking the workflow in real time:
- Current Stage (which of the 7 steps)
- Days Elapsed (cumulative, with typical PA cycle benchmark)
- Document Completeness (X of Y required documents added)
- Outcome (—, Approval, Pend, or Denial)
- Efficiency Score (out of 100, calibrated to speed + document completeness + outcome)

**Plus an educational explainer banner that updates at every step** — explaining what's happening in real US healthcare terms (InterQual / MCG criteria, electronic PA, step therapy, Peer-to-Peer reviews, etc.).

---

## What this exercise demonstrates

### 1. Drag-and-drop turns a flowchart into a learning game

A static diagram of the PA workflow has been written a thousand times. Most readers skim it. Making the user physically drag a case card from stage to stage forces engagement with each transition — and the system can refuse invalid moves (the simulator only allows one-step advancements), which teaches the workflow's actual sequence by enforcing it.

### 2. Document completeness drives outcome probability — and that's the actual lesson

The single biggest cause of PA pends and denials in real healthcare is incomplete documentation at submission. The simulator models this directly: outcome probabilities shift dramatically based on how many required documents the user collected before submitting. A fully documented request can hit 80% approval probability. A 50%-documented request flips to 60%+ pend/deny probability. Users learn the lesson by watching their own scenario play out.

### 3. Real-world clinical and insurance accuracy makes the simulation defensible

Each scenario uses a real CPT code, real medication name, real insurer plan type, and a realistic required-document list. Specialty medication uses step-therapy history and TB screening labs (which Humira actually requires). Inpatient cardiac uses ER triage notes and an admitting cardiologist's order. The simulator can be used as a teaching aid in a real healthcare-administration class without needing footnotes about which parts are simplified.

### 4. The Efficiency Score teaches operations thinking, not just clinical thinking

Many healthcare workflow simulators score on whether you got approval. This one scores on **how efficiently you got there**: speed (days elapsed vs baseline), documentation completeness, and outcome combined into a single 0–100 score. That mirrors how real revenue-cycle and operations leaders actually evaluate PA performance.

---

## Tech notes

- **Single HTML file.** ~52 KB. All CSS and JavaScript inline. No frameworks, no CDN dependencies, no backend.
- **No localStorage.** Per the prompt — all state held in JS memory, resets on Restart.
- **Editable scenario data near the top of the script.** Adding a new scenario takes 12 lines: title, patient, insurer, procedure, required docs, optional docs, and outcome weight tuning.
- **HTML5 drag-and-drop API.** Native browser drag-and-drop (no library) with valid drop-target validation that only allows one-step advancements.
- **State machine for the case progression.** Seven journey stages, each with its own UI state (action title, description, KPIs, educational explainer, button label).
- **Probabilistic outcome engine.** Per-scenario base weights (approve/pend/deny), then dynamically reweighted by document completeness ratio. A fully-documented elective surgery hits ~80% approval probability; a 50%-documented MRI flips to ~50% pend probability.
- **Appeal and Peer-to-Peer flow.** After a denial, the user can file an appeal — appeals get a probability boost when documentation is complete, modeling the real-world overturn rate for well-documented appeals.
- **Educational explainer banner** that updates per stage with real clinical/insurance language (InterQual, MCG, step therapy, electronic PA, P2P review).
- **Live dashboard KPIs** updated on every state change — current stage, days elapsed, document completeness, outcome, efficiency score.
- **Celebration animation on approval.** Pure CSS keyframe confetti, 90 colored slivers, no library.
- **Workflow summary table** on completion showing the full timeline: patient, procedure, insurer, days elapsed, docs submitted, outcome, appeals filed, efficiency score, total steps completed.
- **Modern blue + black UI.** Responsive desktop and mobile layout. Glassmorphism-free, accessible contrast, large drop targets for mobile drag.

---

## Key learnings

**1. Constraint is what makes the game teach.**
If the user could drop the case anywhere, the workflow would be a sandbox. Forcing one-step advancements means the user *learns the sequence* by being unable to skip steps. The teaching is in what the system refuses to allow.

**2. The educational banner is the actual product.**
The drag-and-drop is the gameplay. The educational banner that updates at each stage is the teaching. Without it, users would advance through stages without understanding what "medical necessity evaluation" actually means in payer-policy terms. The banner is what turns a clicker game into a workflow course.

**3. Probabilistic outcomes feel real because real PA outcomes are probabilistic.**
A documented PA request still gets denied sometimes. An incomplete one occasionally gets approved. Building randomness in (within reasonable weight bounds) makes the simulator feel like real healthcare ops — where you can do everything right and still lose, and the way you survive is by keeping your documentation rate above 90%.

**4. Real CPT codes and real insurer names anchor the simulation in credibility.**
"BlueCare Choice PPO" is a real plan type. "CPT 29881" is the right code for the knee meniscectomy. Adalimumab requires TB screening before initiation in real life. These details cost nothing to add and make the difference between a generic flowchart simulator and a tool that could be assigned in a healthcare-administration class.

**5. The hardest UX problem was the appeals flow.**
A denial isn't an endpoint — it's a fork. The user can accept, appeal, or escalate to Peer-to-Peer. Modeling that as a state machine where the case can re-enter the payer-review stage with adjusted probabilities (and a small day penalty for the appeal cycle) is what makes the simulator feel like an actual workflow tool instead of a one-shot quiz.

---

## What surprised me most

Running the same scenario (elective knee surgery) three times with different document collection patterns produced three distinctly different outcomes: a clean 7-day approval, a 12-day pend that became an approval on resubmission, and a 16-day denial that required an appeal to overturn. The same medical condition, the same patient, the same insurer — and the only thing that differed was how thoroughly the user assembled the PA package.

That's the entire teaching point of the PA workflow in real US healthcare. The simulator surfaces it without ever stating it as a lesson. The user learns it by experiencing it.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
