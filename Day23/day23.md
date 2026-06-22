# Day 23 — Customer & MVP Blueprint

**Project:** `customer-mvp-blueprint.html` — a 14-section product strategy document
**Challenge:** #60DayClaudeChallenge — Day 23
**Builds on:** Day 22 Startup Validation Report (FMF 78/100, Conditional GO)
**Document type:** The kind of customer-and-MVP strategy doc a product manager or strategy consultant produces before any engineering investment.

---

## What I built

A structured product strategy document organized around 14 sections that any business analyst, product manager, or strategy consultant would recognize:

1. Executive Summary
2. Ideal Customer Profile (firmographic definition + explicit disqualifiers)
3. Buyer Persona (primary user + secondary buyer, separated)
4. Top 10 Customer Pain Points
5. Customer Journey (Awareness → Consideration → Purchase → Retention)
6. Key Customer Objections — with counter-positioning for each
7. Key Buying Triggers
8. MVP Recommendation — what to build, what's out of scope, success metrics
9. MoSCoW Prioritization (Must / Should / Could / Out-of-v1-Scope)
10. Pricing Hypothesis — three-tier anchor model
11. Top 5 Risks — with severity ratings and mitigations
12. 30-Day Validation Plan — week-by-week with measurable milestones
13. Founder Action Sheet — Top 10 next actions with deadlines
14. Final Verdict — color-coded maturity signal

Plus a 4-score readiness strip at the top (Customer Clarity, Problem Severity, PMF Potential, MVP Readiness).

The output is a single self-contained HTML file under 8 pages — print-ready, PDF-export friendly, no frameworks.

---

## The four readiness scores

The framework produced these on a 0-100 scale, each measuring a different dimension of readiness:

| Score | Value | What it measures |
|---|---|---|
| **Customer Clarity** | **74 / 100** | How precisely the ICP and persona are defined |
| **Problem Severity** | **68 / 100** | How recurring and painful the problem is |
| **PMF Potential** | **62 / 100** | Composite — limited by the one variable only customer behavior can resolve |
| **MVP Readiness** | **81 / 100** | How quickly a real MVP can be built and tested |

**Verdict: 🟢 Proceed to Validation.** The blueprint defines a precise customer, a clear MVP scope, a defensible pricing hypothesis, and a measurable 30-day plan. The right next step for an idea at this stage.

---

## Skills the framework forces

The point of this exercise wasn't to validate or kill one specific startup idea — it was to practice four product strategy disciplines that transfer to any product, analyst, or strategy role:

### 1. Distinguishing the user from the buyer

The person who *uses* a product is rarely the person who *approves the spend*. The blueprint defines Maya (the analyst — primary user) and Daniel (her manager — secondary buyer) as two separate personas with two different jobs-to-be-done. Designing for one and not the other breaks either retention or conversion.

### 2. Writing the explicit "out of scope" list

Most product specs describe what gets built. The harder discipline is naming what gets cut from v1, and why. The MoSCoW framework's fourth column (Out of v1 Scope) is more strategic than the Must Have column — because cutting is harder than adding.

### 3. Separating analysis scores from validation evidence

Three of the four scores (Customer Clarity, Problem Severity, MVP Readiness) can be high based on rigorous analysis. The fourth (PMF Potential) cannot exceed mid-60s until real customers behave a certain way. Distinguishing what desk research can resolve from what only the market can resolve is the single most useful mental model in the entire blueprint.

### 4. Designing measurable validation milestones

A plan that says "let's see how it goes" never gets evaluated. A plan with a Day 30 decision milestone tied to specific evidence (≥5 paying customers at $20/mo) does. Binary milestones remove emotional discretion from the next strategic decision.

---

## What I learned

**1. Customer journey mapping is the conversion funnel in plain language.**
Awareness → Consideration → Purchase → Retention. Each stage has a specific trigger, a specific friction point, and a specific conversion event. Writing it out reveals where the funnel breaks. For OpsBrief, the riskiest step is the user forwarding the brief to her manager — because that's the only step the founder cannot control.

**2. Pricing is a hypothesis, not a decision.**
$20 / $40 / $120 looks confident in the report. It's not. It's a 3-anchor test designed to learn what people will actually pay. The blueprint commits to charging $20 for the first 10 customers, watching what upgrades, and rebuilding the tiering from real data.

**3. Risk severity ratings prevent attention spread.**
"Output accuracy" and "Competitive timing" are both risks. One is a HIGH-severity Day-1 design constraint (source-link-back is mandatory). The other is a MEDIUM-severity timing pressure. Without severity ratings, every risk feels equally urgent and mitigation effort gets spread thin.

**4. The validation plan is the document.**
A blueprint without a 30-day plan is a pitch deck. A blueprint *with* a 30-day plan with measurable weekly milestones is an executable strategy. The plan is what makes the rest of the document operational.

---

## Tech notes

- Single self-contained HTML file. ~31 KB. No frameworks, no CDN, no images.
- Print-ready under 8 pages. Designed for direct PDF export.
- McKinsey/consulting visual language (matches Day 22 report aesthetic for series consistency).
- All visual elements in pure CSS — pricing cards, MoSCoW columns, journey map, risk severity grid, action sheet.

---

## Narrative continuity

- **Day 22** (Validation Report): "Should this idea move forward?" → Conditional GO, FMF 78/100.
- **Day 23** (Customer & MVP Blueprint): "Exactly what gets built, for whom, and how do we test it?" → Detailed product strategy document with 30-day validation milestones.

This is the first time in the 60-day challenge where two consecutive days build on the same artifact. That stacking is the actual lesson — tools and deliverables compound across days, and structured frameworks let one day's output become the next day's foundation.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
