# Day 31 — AI Supply Chain Control Tower

**Project:** `ai-supply-chain-control-tower.html` — real-time supply chain disruption management game
**Challenge:** #60DayClaudeChallenge — Day 31
**Type:** Single-file vanilla JavaScript application. No React, no Tailwind, no backend, no images, no APIs.

---

## What I built

A 3-minute real-time game where the player plays the **Head of Operations** at a global supply chain company. Operational alerts arrive at the control tower with priority levels and time-remaining bars. The player picks one of four actions per alert. Each action affects six live business KPIs. At the end of the shift, the player gets a performance grade from A+ to D plus an operational summary computed from how they actually played.

This is a live-tension simulator. Unlike Days 29–30 (which let the user think between decisions), this one runs on a clock. Alerts age out automatically. The final minute spawns multiple alerts simultaneously. The player has to triage by priority because they cannot solve everything.

---

## The eight KPIs

Live across the top of the dashboard, animated bars on every decision:

| KPI | Direction | Why it matters |
|---|---|---|
| **Service Level %** | Higher = better | On-time delivery rate. Boards ask about it first. |
| **Customer Satisfaction** | Higher = better | NPS-style score. Below 60 is churn risk. |
| **Inventory Health** | Higher = better | Stockout vs. obsolescence balance. |
| **Transportation Efficiency** | Higher = better | How well your logistics network flows. |
| **Operating Cost** | Lower = better | Air freight + rush carriers blow this up fast. |
| **Revenue Protected** | Higher = better | Dollars saved from disruption response. |
| **Score** | Higher = better | Composite — the basis of the final grade. |
| **Time Remaining** | Counter | 3 minutes, no extensions. |

---

## The ten alert types

Each one is a recognizable real-world disruption with a realistic description and business impact:

1. 🚨 **Port Congestion** — LA Terminal, 6 containers stuck
2. 🚨 **Supplier Delay** — Tier-1 vendor 9 days behind, plant shutdown in 5
3. 🚨 **Truck Breakdown** — Linehaul tractor failed near Phoenix
4. 🚨 **Warehouse Running Out of Stock** — Top 5 SKUs hit reorder point at DC-3
5. 🚨 **Customs Inspection** — 40ft container at Newark, 48–72h hold
6. 🚨 **Demand Spike** — Hero SKU +180% volume after viral mention
7. 🚨 **Factory Machine Failure** — Bottling line 4 down for 18h
8. 🚨 **Weather Disruption** — Atlantic storm forecast at Savannah port + DC
9. 🚨 **Wrong Inventory Count** — 380-unit discrepancy at DC-2
10. 🚨 **Damaged Shipment** — 18 damaged units to top-10 B2B account

Each one ships with a title, description, impact statement, priority (Critical / High / Medium / Low), time-remaining bar, and four action buttons.

---

## The eight action types

The actions are the same vocabulary a real ops team uses:

- **Expedite Shipment** — fast but costly recovery
- **Use Backup Supplier** — risk mitigation, slightly higher price
- **Reroute Trucks** — flexibility move
- **Increase Production** — volume catch-up at sister plants
- **Transfer Inventory** — network rebalance
- **Approve Air Freight** — last-resort speed, kills sustainability and margin
- **Ignore** — sometimes the cheapest right answer, usually a mistake
- **Delay Decision** — almost always the wrong call in live ops

One action per alert is marked as ✓ recommended. The other three carry varying degrees of consequence.

---

## Game logic that matters

**Priority-weighted spawn rate.** Early game (first 60s): mostly low/medium alerts. Mid-game (60s–120s): more high-priority. Late game (last 60s): critical alerts dominate, multiple at once.

**Time-remaining auto-fail.** If an alert ages out before the player acts, KPIs take damage scaled to priority. Critical auto-fails cost about 4x what low-priority ones do. This is the single biggest score leak in the game.

**Delayed consequences.** ~18% of wrong actions trigger a delayed fallout 14 seconds later. The player sees their score dip even after they thought they handled the situation. Models real downstream effects.

**Recommended action vs. wrong action.** Choosing the recommended action increases score and protects KPIs. Choosing "Ignore" or "Delay Decision" on a critical alert almost always tanks customer satisfaction, service level, and revenue.

**Final grade calibration.** A+ at 200+ score. A at 140+. B at 80+. C at 30+. D below 30. Most first-time players land at C or B because they over-use "Delay Decision" trying to read the situation perfectly.

---

## Tech notes

- **Single HTML file.** ~45 KB. All CSS and JavaScript inline. No frameworks, no CDN, no APIs, no images.
- **State machine in plain JS.** A single `state` object holds running flag, paused flag, timer, KPI values, active alerts, history (correct/wrong/auto-failed), delayed consequences.
- **Setinterval game loop** — fires every 1000ms. Ages alerts, spawns new ones, triggers delayed consequences, updates KPIs.
- **CSS animations** — pulse glow on critical alerts, flash on KPI updates, scrolling event log fade-in, modal pop entrance.
- **Color-coded priority system** — Critical (red with pulse), High (red, no pulse), Medium (amber), Low (blue).
- **Reactive DOM updates** — re-render alerts and KPIs on every tick. No diffing needed at this scale.
- **Help modal** with full instructions on how alerts and KPIs work.
- **Pause / Sound (visual toggle) / Help / Restart** all functional.
- **Responsive layout** — single column on mobile, two-column on desktop, KPI grid collapses gracefully.

---

## What this exercise demonstrates

### 1. Real-time games teach triage, not just decisions

Day 29's Operation Lifeline let the player think between decisions. Day 31 doesn't. The clock forces the player to develop a triage instinct — *which alert do I act on first?* That's the actual skill of a Head of Operations and it's the skill the game teaches by removing the option to think too long.

### 2. The "Ignore" and "Delay Decision" options are the actual lesson

Most players overuse these in their first playthrough. Watching their KPIs collapse teaches them — without ever saying it explicitly — that in live operations, the cost of waiting almost always exceeds the cost of acting on the second-best option. That's the entire industry lesson the simulator surfaces by making it expensive in real time.

### 3. The grade is a coaching tool, not a verdict

The final summary text is computed from the player's actual decision pattern: too many auto-fails, low CSAT, high operating cost, accuracy ratio. The same A grade looks different depending on *why* the player got there — and the summary names which lever they pulled best.

### 4. Vanilla JS scales to real interactive applications

This is the most complex single-file game in the challenge so far — real-time clock, multiple concurrent active items, delayed effects, animated UI, modals, responsive layout — and it shipped in 45 KB of plain HTML/CSS/JS. No build step, no framework, no dependency drift.

---

## Key learnings

**1. Pressure changes the lesson the game teaches.**
Untimed simulators teach analysis. Timed simulators teach triage. Both are valid. They are *different* skills, and a Head of Operations needs the timed one more often than the untimed one in any given week.

**2. Priority badges work harder than urgency text.**
A red "CRITICAL" pulse badge gets clicked first, every time, regardless of which alert actually has the biggest dollar impact. That's a real ops-room dynamic — visual priority shapes attention more than written priority — and the game surfaces it without ever explaining it.

**3. Color and motion are the actual UI of a live dashboard.**
The pulse glow on critical alerts, the flash on KPI updates, the time bar draining visibly — that motion is what makes the dashboard feel "live." A static version of the same data is forgettable. A moving version is unforgettable.

**4. Delayed consequences are the single most teaching-rich mechanic.**
A wrong decision that *only* shows up in the score 14 seconds later changes how the player thinks about every future decision. That's how real operational debt works — and most ops simulators don't model it.

---

## Narrative continuity

- **Day 29 (Operation Lifeline)** — strategic crisis response. Untimed. Player makes 6 reflective decisions.
- **Day 30 (Supply Chain Builder)** — foundational design. Untimed. Player builds a supply chain before any crisis hits.
- **Day 31 (Control Tower)** — operational triage. **Timed**. Player runs a 3-minute live shift managing concurrent disruptions.

Three angles on the same domain — strategy, design, execution — across three educational modalities — reflective, foundational, real-time. The compounding teaching is the actual point of the three-day arc.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
