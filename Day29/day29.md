# Day 29 — Operation Lifeline: Supply Chain Crisis Lab

**Project:** `operation-lifeline.html` — randomized enterprise crisis simulation
**Challenge:** #60DayClaudeChallenge — Day 29
**Type:** Interactive React-based business simulation. Single HTML file. React via CDN + Babel JSX. No Tailwind, no APIs, no images, no external assets.

---

## What I built

A single self-contained HTML application that lets the user play the role of a Chief Supply Chain Officer leading a randomized enterprise through a randomized crisis. Eight stages, six decision points, six scored dimensions, and a personalized executive dashboard at the end. Every playthrough produces a different company, crisis, supplier behavior, and outcome.

The eight stages:

1. **Welcome** — title, subtitle, *Start Simulation* button
2. **Company Generation** — a fictional enterprise with industry, revenue, factory count, warehouse count, supplier count, inventory days, lead time, operating countries
3. **Crisis Briefing** — one of eight crisis types (factory fire, supplier bankruptcy, port strike, ransomware, regional flood, raw material shortage, trade tariff escalation, carrier capacity collapse) with urgency badge and business impact
4. **War Room** — six possible response actions, the player picks three; each one affects Cost, Inventory, Profit, Delivery Speed, and Customer Satisfaction via animated progress bars
5. **Negotiation** — four rounds with the largest supplier; each round the player picks one of four approaches (hard-line, partnership, volume, walk-away); supplier Trust, Cost Position, and Lead Time drift in response
6. **CEO Boardroom** — five multiple-choice leadership questions covering price pass-through, dual-sourcing, crisis playbook, customer communication, and resilience budget allocation; answers are scored 1–10 each
7. **AI Strategy** — pick two of five AI investments (Demand Forecasting, Inventory Optimization, Supplier Risk Monitoring, Warehouse Computer Vision, Procurement Copilot); each describes expected business impact
8. **Final Dashboard** — Overall Crisis Score (0–100), six sub-scores (Leadership, Negotiation, Resilience, Cost Control, Risk Management, Customer Satisfaction), personalized feedback (best decision, biggest mistake, AI strategy summary, expert recommendation, lessons learned), and a Replay button

---

## What this exercise demonstrates

### 1. React via CDN is enough for serious interactive simulations

No build step. No npm. No bundler. Three script tags (`react`, `react-dom`, `@babel/standalone`) inside a single HTML file, and the entire simulator runs offline. For an educational tool meant to be opened by a single click on a teacher's laptop, this is the right architecture — and it ships in 45 KB.

### 2. Randomization is the durability layer

A scripted simulation gets played once and explained. A randomized simulation gets replayed because the user wants to see what happens with a different combination of company × crisis × decisions. Eight crisis types × eight companies × six war-room combinations × four negotiation approaches across four rounds gives the simulator effectively unlimited playthroughs from a small content base.

### 3. The hardest design choice was the scoring rubric

Naïve scoring would let any user hit 90 by clicking everything. The simulator scores six dimensions independently and combines them — which means a user who chose a price surcharge gets a Cost Control bump but a Customer Satisfaction penalty. A user who picked the partnership negotiation approach builds Negotiation score but trades short-term cost. The user has to actually trade off, the way a real CSCO has to.

### 4. Personalized feedback is what makes the simulation feel coached, not just scored

The final dashboard surfaces the user's *best* answer, their *worst* answer, their AI strategy, an expert recommendation, and lessons learned. None of those are pre-written. They are computed from the actual choices the user made. That turns a quiz score into a coaching session — which is the actual goal of an enterprise crisis simulation.

---

## Tech notes

- **Single HTML file.** ~45 KB. React + Babel via CDN. No frameworks, no Tailwind, no images, no APIs.
- **Eight-stage React state machine.** A single `App` component holds the stage index and the artifacts collected at each stage. Each stage is its own component that calls `onComplete(result)` to advance.
- **All randomization seeded fresh on Replay.** New company. New crisis. New supplier behavior. Same simulator.
- **useState everywhere — no Redux, no Context, no router.** State stays local to each stage component except for the artifacts that need to flow into the final dashboard.
- **Animated KPI bars** via CSS transitions on width.
- **Reusable `<KpiBar>` and `<Stepper>` components** factored out of the otherwise self-contained stage components.
- **Premium dark enterprise dashboard theme** — navy + cyan + gold accents, hover effects, smooth transitions, rounded cards, modern typography.

---

## Key learnings

**1. A single-file React app is the right tool for educational simulations.**
Distributable as one HTML file. Opens in any browser. Works offline. No setup. For tools meant to be assigned in a classroom or shared on a Slack channel, the file-as-package architecture beats a deployed web app every time.

**2. Randomization changes the unit of replay.**
A scripted simulation is a "session." A randomized simulation is a "playthrough." The unit-of-experience shrinks, which means the user comes back more often — and learns more, faster.

**3. Multi-dimensional scoring forces real trade-offs.**
The user who tries to maximize one number sacrifices another. That's the entire point of executive decision-making, and it's the lesson the simulator teaches by enforcing it numerically.

**4. Personalized feedback is computed, not pre-written.**
Surfacing the user's *highest-scoring boardroom answer* by name in the final dashboard is what turns a generic "great job!" into "this specific decision is the one a board would respect." Same code, infinitely different outputs.

**5. The hardest UI engineering was the bar animations on commit.**
The user picks three war-room actions, then clicks Commit. The bars animate from their current state to the new state over ~900ms before the next stage loads. That ~1 second of visible feedback is what gives the decision weight. Skip it, and the simulator feels like clicking through a survey.

---

## What surprised me most

The negotiation stage is the most operationally accurate part of the simulator. The four approaches (hard-line, partnership, volume, walk-away) each shift three variables (trust, price, lead time) in different directions across four rounds. A user who hard-lines every round loses trust catastrophically and walks away with marginally better prices but a permanently damaged supplier relationship. A user who partnerships every round builds enormous trust but pays slightly higher prices. The optimal play is *mixing* approaches — exactly the way real procurement directors operate.

The simulator wasn't told to teach that. It teaches it because the math forces it.

---

## Narrative continuity

- **Days 26–28** focused on US healthcare operations (Prior Authorization workflow, story simulator, hospital admission readiness).
- **Day 29** shifts to enterprise supply chain crisis management — a different domain but the same underlying lesson: real operational decisions are multi-dimensional trade-offs that cannot be solved by completing more checkboxes.

The challenge keeps reinforcing the same insight across domains: workflow simulators teach faster than flowcharts because the user has to *commit* to a decision before they see the consequence.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
