# Day 25 — AI Shark Tank Simulator

**Project:** `ai-shark-tank-simulator.html` — single self-contained HTML application
**Challenge:** #60DayClaudeChallenge — Day 25
**Type:** Interactive browser app demonstrating how AI can be used to build complete startup-education experiences without a backend.

---

## What I built

A working AI Shark Tank Simulator that any user can pitch a startup idea to in their browser. Single HTML file. No backend. Everything runs locally — input handling, judge logic, scoring, decision rules, leaderboard, downloadable pitch report — all client-side JavaScript.

The simulator walks the user through three stages:

**Stage 1 — Startup input.** Six fields: Startup Name, Target Audience, Problem Statement, Solution, Revenue Model, Funding Ask. A "Load Demo Startup" button populates the form with a realistic example so users can see it work in 5 seconds. Four AI judges are introduced with their names, roles, and what each one cares about.

**Stage 2 — Pitch round.** The user's pitch is displayed at the top of the screen. Each of the four judges asks two questions, dynamically templated from the user's actual inputs (their startup name, problem, revenue model, and ask all appear inside the questions). The user types an answer to each question. After submitting, the judge "reacts" with a short response calibrated to the answer's score. Progress dots fill across the top as questions get answered. Eight questions total before the verdict.

**Stage 3 — Results.** A 5-dimension scorecard (Market Potential, Innovation, Business Model, Execution, Investment Worthiness), a composite score, an investment verdict (Invest / Come Back Later / Acquire / Reject), suggested valuation, funding amount, and reasoning. A leaderboard saves the user's top 5 pitches to localStorage. The user can download a fully formatted pitch report as an HTML file, share their result, or pitch another startup.

---

## The four judges

Each judge has a name, a role, and a focus that shapes the questions they ask:

| Judge | Role | Focus | Questions calibrated around |
|---|---|---|---|
| **Vera Chen** | Venture Capitalist | Market size, scalability, defensibility | TAM, the path from 100 to 10,000 customers, moats against incumbents |
| **Marcus Reed** | Founder (two exits) | Execution, founder grit, what you build first | Smallest shippable version, founder's unfair advantage |
| **Priya Sharma** | Customer Advocate | Usefulness, real pain, day-to-day adoption | The moment of value in week one, why a spreadsheet won't do |
| **David Okafor** | Angel Investor | Profitability, capital efficiency, exit path | What the funding buys in months of runway, unit economics at 100 customers |

Each judge asks two questions per pitch. That's eight total questions per session, dynamically built from the user's actual inputs.

---

## How scoring works

The scoring engine isn't a black box — it's an explicit heuristic that rewards the things investors actually reward in real pitches:

- **Length sweet spot.** Answers between 12 and 80 words score highest. Walls of text get penalized.
- **Specificity.** Any number, percentage, or dollar amount adds points. "Saves 90 minutes a day" beats "saves time."
- **Domain vocabulary.** Each judge has a keyword set tied to their focus. Mentioning MRR or CAC in front of the angel investor earns points. Mentioning TAM or moats in front of the VC earns points.
- **Hedge penalty.** "Maybe," "I think," "sort of," "TBD" all subtract points. Investors interpret hedges as missing data.

Per-question scores are aggregated by judge, then mapped to the five dimensions of the scorecard. The composite is the average. The verdict thresholds are:

- **78+ → INVEST** (full ask, 12% equity, standard diligence)
- **62–77 → COME BACK LATER** (half-ask bridge, validate next milestone, return when numbers are real)
- **48–61 → ACQUIRE** (acqui-hire / product acquisition, not standalone scale)
- **Below 48 → REJECT** (not yet — gaps in validated pain, distribution, or unit economics)

The thresholds are tuned to roughly match how early-stage investors actually triage pitches in real meetings, where most ideas don't fit "yes" or "no" cleanly and the most common honest answer is "come back when you have evidence."

---

## What this exercise demonstrates

Four things this exercise demonstrates about building business-education tools with AI:

### 1. A simulator can teach domain knowledge without requiring an instructor

The user doesn't need a textbook on what an angel investor cares about — they learn it by reading David Okafor's questions, getting scored, and seeing why their answer wasn't sharp enough. The product *is* the lesson.

### 2. Rule-based judge logic is enough — no LLM call required

The judges feel responsive because the questions are templated from the user's actual inputs and the reactions are calibrated to answer quality. That doesn't require an API key or an internet connection. The whole app runs in a single HTML file. For a startup-education tool, this is a feature: it works on any laptop, in any classroom, with no setup.

### 3. The leaderboard turns a one-time exercise into a portfolio

Storing the top 5 pitches in localStorage means a user can come back, refine a pitch, and watch their composite score improve. The exercise compounds. That's the difference between a quiz and a tool.

### 4. The downloadable report extends the artifact past the session

The HTML report includes the full Q&A transcript, the scorecard, the verdict, and the reasoning. A user can attach it to an email, share it with a co-founder, or include it in an application. The simulator's value isn't only what happens in the browser — it's what the user takes away.

---

## Tech notes

- **Single HTML file.** ~42 KB. All CSS and JavaScript inline. No frameworks, no CDN dependencies, no backend.
- **Three-stage state machine.** Input → Pitch Round → Results. Clean DOM swap between stages.
- **Templated question generation.** Each judge has two question templates that substitute the user's actual startup name, audience, problem, revenue, and ask. The output reads custom even though the engine is rule-based.
- **Heuristic scoring engine.** Explicit, auditable scoring rules (length, specificity, keywords, hedge penalty). Per-answer scores roll up into per-judge averages, which roll up into the five-dimension scorecard.
- **Verdict + valuation + funding calculator.** Threshold-based with explicit reasoning per verdict tier.
- **localStorage leaderboard.** Top 5 by composite score persisted across sessions.
- **Downloadable HTML pitch report.** Full Q&A transcript + scorecard + decision exported as standalone HTML for sharing or saving as PDF.
- **Confetti animation on positive verdicts.** CSS keyframes, no library.
- **Responsive layout.** Works on desktop and mobile.

---

## Key learnings

**1. Templated questions feel custom when the template is good.**
The questions reference the user's actual startup name, problem, revenue model, and funding ask. Even though there are only two templates per judge, the output feels like the judge is reading the pitch. The trick is good substitution points, not more templates.

**2. A scoring heuristic doesn't need to be smart to be useful.**
Length + specificity + keyword match + hedge penalty produces scores that are directionally right 80% of the time. For a simulator, that's enough. Users learn what good answers look like by watching their scores go up when they include numbers.

**3. The hardest part wasn't the logic — it was the UX choreography.**
Showing the right judge bubble at the right time, animating progress dots, scrolling to the new question, calibrating the reaction message to the score — those interactions are what make the simulator feel alive. The judge logic is 100 lines. The UX choreography is 300.

**4. The verdict tiers matter more than the score.**
A composite of 73 versus 75 doesn't feel meaningfully different to a user. But "Come Back Later" versus "Invest" *does*. Designing four distinct verdict states — each with its own reasoning, valuation logic, and emotional weight — turned a number into a story. That's what makes the simulator memorable.

---

## What surprised me most

Watching the Demo Startup ("FreshRoute") get a different verdict each time I ran it with different answers — even though the pitch and the judges were identical. That's the right behavior. The exercise isn't about whether your *idea* is good. It's about whether your *answers* are sharp. The simulator scores the founder, not the startup.

That's a more honest model of how real investor meetings work than most pitch simulators I've seen.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
