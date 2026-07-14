# Day 45 — Decision Strategist

**Project:** `decision-strategist.html` — a single-page interactive Decision Report generated from a 4-question interview
**Challenge:** #60DayClaudeChallenge — Day 45
**Assumed decision:** Finish the 60-Day Challenge vs. pause it for job applications vs. compress the remaining days (a real trade-off matching the user's MSBA + F-1 context)
**Type:** Single-file HTML/CSS/JavaScript. No frameworks, no CDN except Google Fonts (Inter). Works fully offline after first load.

---

## What I built

A full interactive Decision Report — the kind an expensive consultant would print for you after a strategy session. Eight sections in a single-column card layout, 720px max-width, dark editorial UI. Every section has its own accent color for scannability, a 3-4px left border in that color, and a hover lift.

Each section is a distinct analytical frame — the real decision, the case for each option, an assumption buster (with named cognitive biases), a decision matrix with animated bars, a premortem for the top two options, a 7-day test plan, the verdict, and three shareable mini-cards for LinkedIn.

The point isn't to give the user an answer. It's to force them into the exact reasoning steps a good strategist would walk them through — reframe, contrast options, expose biases, quantify trade-offs, imagine failure, plan a test, decide with a hard truth.

---

## The 4-question interview

Per the spec, the tool interviews with 4 questions, one at a time:
1. What's the decision you're stuck on?
2. What's your goal + timeline?
3. What does your gut say + what's stopping you?
4. What are you most scared of getting wrong + is it reversible?

The user's answers seed every section. In this build, the user asked me to assume a realistic scenario — so I picked one that fits the MSBA / F-1 job-search context we've been discussing across the challenge: whether to continue the challenge at current pace, pause it for full-time applications, or compress the remaining 15 days.

---

## The eight sections

**01 · The Real Decision.** Reframes the surface-level question ("which is the best allocation of hours") into the actual underlying decision ("do you trust the compounding of visible work over the certainty of applying more"). Names the trade-off in one line and the emotional difficulty in another.

**02 · The Case For Each Option.** Three option cards (Continue / Pause / Compress). Each has: strongest case for it, hidden upside most people miss, biggest weakness, and a "Best if you value: ___" line so the user can spot which option matches their real preferences.

**03 · Assumption Buster.** Six chips — 3 assumptions the user may be making, 2 named cognitive biases (Sunk Cost Fallacy, Status Quo Bias), and 1 thing they're definitely ignoring. This is the section that does the actual challenging work.

**04 · Decision Matrix.** Seven dimensions × two primary options = fourteen animated horizontal bars. Bars fill from 0% on page load with staggered delays. Winner (higher total) gets a glowing gold border on its total. The narrative below the matrix names the specific dimensions that decided the ranking.

**05 · Premortem.** For the top two options, imagines they failed at 12 months. Three plausible reasons for each failure, one early warning sign, one prevention action. Turns abstract risk into concrete monitoring criteria.

**06 · 7-Day Test Plan.** Not "do the research." Specific daily actions. Days 1-2 are research. Days 3-4 are one small experiment. Days 5-6 are one conversation. Day 7 is decision day with explicit criteria that map back to what was tested.

**07 · The Verdict.** Picks one option decisively. Explains why in 2 lines. Names exactly what could flip the verdict. Ends with a one-sentence hard truth in italics. In this build, the verdict overrides the matrix because reversibility is the tiebreaker.

**08 · Shareable Mini-Cards.** Three cards designed to look good as individual screenshots: matrix summary with totals, verdict, and a LinkedIn-ready hook + caption + CTA. Each has a small "Built with Claude AI" watermark.

---

## Design specifications applied

The task prompt was unusually detailed about the visual design — specific color hex codes, specific typography rules, specific spacing. I followed the spec verbatim:

- CSS variables for the exact palette (--bg-main #0f0f14, --bg-card #1a1a24, five accent colors)
- Inter from Google Fonts, weights 400-800
- 720px max-width single-column card layout
- 16px border-radius on cards, 12px on inner cards
- Section-specific left borders in accent colors (blue for Real Decision, green for Options, purple for Assumptions, red for Premortem, gold for Verdict)
- Verdict card is larger with a subtle 20px gold glow shadow
- Decision matrix bars use option-specific gradient fills, animate from 0% width via `@keyframes growBar` with 0.8s cubic-bezier easing
- Reduced-motion fallback removes animations
- Below 600px viewport, matrix labels stack above bars, shareable cards go 1-column

---

## The decision itself

The assumed scenario is deliberately realistic for the user (MSBA candidate on F-1 visa, 45 days into a 60-day public build challenge, active job hunt with real time pressure). The three options are:

- **A · Continue** the challenge at current 2-3 hr/day pace
- **B · Pause** the challenge for 30 days, all-in on applications
- **C · Compress** the remaining 15 days into 8, then all-in

Matrix scored B highest (51/70), but the verdict overrides to A because A's reversibility (8/10) means the user can switch to B any day the signal warrants — but the reverse move (B → A) forfeits the 45-day streak that would be very hard to rebuild.

This is exactly the kind of "the matrix says X but the reasoning overrides to Y" pattern that separates decision analysis from decision math.

---

## Tech notes

- **Single HTML file.** ~40 KB. Vanilla CSS + JavaScript.
- **Google Fonts (Inter)** — the one external dependency, loaded once, then cached.
- **CSS-only animations** for matrix bars using `@keyframes` and `animation-delay`.
- **`prefers-reduced-motion` respected** — bars set to final width without animation.
- **Fully responsive** — grid layouts collapse to single-column below 600px.
- **No framework, no build step, no external assets beyond one Google Font.**

---

## What this exercise demonstrates

### 1. Structured decision analysis is a UX problem, not a math problem

The matrix score alone would have picked B. The report picks A because a decision framework has to weight *reversibility* — a math-optimal choice with high downside is worse than a slightly lower-scored choice with easy escape. Naming that tiebreaker explicitly is the whole point of a strategist.

### 2. Naming biases is where the teaching happens

Every user of a decision tool secretly wants confirmation. The Assumption Buster is the section that pushes back — sunk cost, status quo bias, and the thing they're ignoring. Without that section, this is just a fancy pro/con list.

### 3. The premortem is more useful than the analysis

Imagining specific failure modes 12 months out surfaces early warning signs and prevention actions the analysis would miss. The premortem paragraph pattern (three failure reasons + one warning sign + one preventive action) generalizes to any strategic decision.

### 4. Shareable cards are the growth loop for these tools

Someone who screenshots and posts the verdict card is signaling to their network that they use AI for real decisions. That's exactly the audience this tool grows through.

---

## Narrative continuity

- **Day 41** — Interactive Learning Studio (SQL Window Functions)
- **Day 42** — Personal Financial Command Center
- **Day 43** — AI Workflow Architect (Data Analysis)
- **Day 44** — LinkedIn Roast & Rebuild
- **Day 45 — Decision Strategist**

Personal-productivity arc continues. Day 42 optimized money. Day 43 optimized analytical work. Day 44 optimized professional visibility. Day 45 optimizes the thing that determines whether any of the other three matter — the calls you make about what to do next.

---

## Tags

`#60DayClaudeChallenge` `#DecisionMaking` `#Strategy` `#AITools`
