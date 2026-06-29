# Day 30 — Supply Chain Builder

**Project:** `supply-chain-builder.html` — beginner-friendly interactive supply chain design simulator
**Challenge:** #60DayClaudeChallenge — Day 30
**Type:** Single-file React app for learning enterprise supply chain fundamentals through hands-on decision-making.

---

## What I built

A single HTML application that lets a complete beginner build a supply chain from scratch in five sequential decisions. Each step starts with a beginner-friendly explainer of what the concept means, why it matters, and how it affects a real business. After every choice, the simulator surfaces the plain-English trade-off the user just accepted, and five live business metrics animate to reflect the impact.

The flow has eight stages: Welcome → Random Company Generation → five decision steps (Suppliers, Factory Location, Warehouse Strategy, Transportation, Inventory Strategy) → Executive Dashboard. Every playthrough randomizes the company (industry, products, countries served, revenue, customer base, demand profile) and produces a different final score and personalized recommendations.

---

## The five decisions

### 1. Suppliers — Single vs. Multiple

The explainer covers what a supplier is and why supplier strategy is the foundation of supply chain risk. The choice is single supplier (cheaper, fragile) vs. multi-supplier (more expensive, resilient).

### 2. Factory Location — Domestic vs. Offshore

Beginner explainer covers labor cost vs. lead time vs. risk exposure. The choice is the one most companies regret first when a crisis hits — because it's the hardest to change later.

### 3. Warehouse Strategy — Central vs. Regional

Explainer covers how the warehouse network controls whether the company can credibly promise 2-day delivery. Central is cheap and slow; regional is expensive and fast.

### 4. Transportation — Road, Rail, Sea, or Air

Four-way choice with explainer on cost-per-mile, speed, and sustainability. Each mode has a distinct combination of trade-offs that the simulator surfaces explicitly after the choice.

### 5. Inventory — Lean, Balanced, or High Safety Stock

The decision that connects supply chain to the balance sheet. Explainer covers working capital impact, stockout risk, and obsolescence. CFOs care about this one more than any other.

---

## The live business metrics

After every decision, five metrics update with animated progress bars and visible deltas:

| Metric | What it captures |
|---|---|
| **Cost** | Total cost position. Lower is better. |
| **Delivery Speed** | How fast products reach customers. |
| **Risk** | Disruption exposure. Lower is better. |
| **Customer Satisfaction** | Net outcome customers experience. |
| **Sustainability** | Carbon footprint, mode efficiency, sourcing impact. |

The user can see in real time which decisions are trading one metric against another. That visible feedback loop is what turns the simulator from a quiz into a teaching tool.

---

## The executive dashboard

At the end, the user sees:

- **Overall Supply Chain Score (0–100)** — a weighted blend of all five metrics
- **Score band** — Resilient · Solid · Workable but Fragile · High-Risk
- **Top two Strengths** — with explanations of what the user got right
- **Bottom two Weaknesses** — with explanations of what's underperforming
- **Biggest Risk** — single most consequential exposure, computed from the user's actual choices (offshore + single supplier surfaces a different recommendation than central warehouse + lean inventory)
- **Three Practical Improvements** — calibrated to the specific weaknesses, not pre-written

All feedback is computed from the user's choices and metric values, not hardcoded. Same simulator, different recommendations every playthrough.

---

## Tech notes

- **Single HTML file.** ~41 KB. React + Babel via CDN. No frameworks, no Tailwind, no images, no APIs, no external assets.
- **State machine across 8 stages.** A single `App` component holds the index, the company, accumulated choices, and the current + previous metric snapshots.
- **Reusable React components.** `Stepper`, `KpiBar`, `LiveMetrics`, `Welcome`, `CompanyProfile`, `DecisionStep`, `FinalDashboard`. All factored cleanly.
- **Animated progress bars** via CSS transitions on width with cubic-bezier easing.
- **Delta indicators** show how each KPI changed after the latest decision (+8, −12, etc.) right next to the current value.
- **Computed feedback functions** (`strengthExplain`, `weaknessExplain`, `biggestRiskExplain`, `improvementsFor`) produce different output for different score patterns. The dashboard reads like coaching, not like a generic results page.
- **Premium dark enterprise theme.** Navy + cyan accents, rounded cards, hover effects, smooth transitions.

---

## What this exercise demonstrates

### 1. Teach-before-decide is the foundational pedagogical pattern

Every step shows the beginner-friendly explainer (concept, why, impact) before the choices appear. The user encounters the vocabulary in context, then makes a decision against that vocabulary. That ordering — concept first, decision second — is what separates an educational tool from a quiz.

### 2. Plain-English trade-offs are the unit of learning

The simulator never displays raw numbers without explaining what they mean. After each choice: *"You save 25–40% on labor. Your average delivery time triples. Every port strike, every tariff change, every fuel-price spike is suddenly your problem."* That's a sentence, not a chart. A beginner can read it and form a mental model.

### 3. Live metric feedback closes the learning loop

The user picks a choice, sees the trade-off written in English, watches five bars animate, and observes the deltas next to each metric. That's three different ways of seeing the same decision land. The reinforcement compounds.

### 4. Personalized recommendations make the dashboard feel coached

A user who picked offshore + single supplier sees different recommendations than a user who picked central warehouse + lean inventory. Same simulator, different coaching. That's what makes the user replay it — to see what happens when they trade differently.

---

## Key learnings

**1. Beginner-friendly is harder than expert-friendly.**
Writing "concept → why → impact" in plain English for each of the five decisions took more iterations than the React code did. The temptation is to use industry vocabulary. The discipline is to say it twice — once in jargon, once in English — and then delete the jargon.

**2. Trade-offs are more memorable than features.**
"You save 8–15% on unit cost" is forgettable. "You'll save 8–15% on unit cost. If your supplier has any kind of disruption — fire, bankruptcy, port strike — you have no backup. This is the choice that looks great until it doesn't" is the version the reader remembers. The trade-off framing is what makes the concept stick.

**3. The hardest design decision was the metric weighting.**
The Overall Score blends Cost (20%), Delivery Speed (22%), Resilience (20%), Customer Satisfaction (22%), and Sustainability (16%). Tuning that weighting determined whether the simulator rewarded penny-pinching, speed-chasing, or balanced design. The current weighting rewards balanced design — which is the actual lesson supply chain leaders want to teach.

**4. Feedback computed from choices beats feedback written for all users.**
The Biggest Risk panel calls out *"offshore factory + single supplier is the most exposed configuration in the model"* only when the user actually picked both. Same code, infinitely many outputs. That's what makes the dashboard feel earned.

**5. The Replay button is the most important button in the app.**
The simulator's value is in the replay loop. One playthrough teaches the user supply chain vocabulary. Three playthroughs teach them the trade-off space. Ten playthroughs teach them how to design for their own real-world situation. The Replay button is the conversion mechanism from "interesting demo" to "tool I use."

---

## Narrative continuity with Day 29

- **Day 29 (Operation Lifeline)** dropped the user into a crisis at an existing enterprise. The teaching was *react to disruption*.
- **Day 30 (Supply Chain Builder)** lets the user *design the supply chain* from scratch before any crisis hits. The teaching is *prevent disruption by structural choice*.

Two simulators, two angles on the same domain. The challenge keeps reinforcing the same lesson across formats: enterprise decisions are multi-dimensional trade-offs, and the best teaching happens when the user has to *commit* to a choice before they see the consequence.

---

## What surprised me most

The Transportation step turned out to be the most teaching-rich decision in the whole build. Four options (Road / Rail / Sea / Air), each with a wildly different cost-per-mile, speed, and sustainability footprint. A user who picks Air freight because they want fast delivery sees Sustainability collapse from 50 to 30 in a single click. The trade-off isn't subtle and the simulator surfaces it immediately.

That immediate, visible cost of optimizing one dimension is the entire lesson the simulator is trying to teach. The Transportation step is where it lands hardest.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
