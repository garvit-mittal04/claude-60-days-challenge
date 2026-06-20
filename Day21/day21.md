# Day 21 — Digital Privacy Intelligence Dashboard

**Project:** `privacy-dashboard.html` — single-file interactive cybersecurity dashboard
**Challenge:** #60DayClaudeChallenge — Day 21
**Dataset:** Prompt's sample stack — 15 services, 11 parent companies
**Output:** Premium-feel SaaS dashboard with 9 sections + Final Verdict, no frameworks, no build step

---

## The dataset

The prompt provides a 15-service footprint treated as **Facts**: Instagram, Snapchat, TikTok, YouTube, Discord, WhatsApp, iMessage, Spotify, Roblox, PUBG Mobile, Amazon, Meesho, Google Search, Google Pay, Google Photos.

Parent companies inferred from the services: **Alphabet (4), Meta (2), Snap Inc., ByteDance, Discord Inc., Apple, Spotify, Roblox Corp., Krafton/Tencent, Amazon, Meesho** — 11 distinct entities.

---

## The headline numbers

| Metric | Value | Band |
|---|---|---|
| Digital Footprint Score | **79 / 100** | 🟠 Significant |
| Privacy Score | **50 / 100** | 🟠 Fair |
| Total Services | 15 | Reported |
| Parent Companies | 11 | Distinct |
| Ecosystem Concentration (HHI) | 12.9 | Alphabet-dominant |
| Unique Data Types Tracked | 31 | Across the stack |

**Scoring model (explainable, no black box):**

- Digital Footprint = 35% from service count + 35% from average sensitivity + 30% from average tracking intensity.
- Privacy Score = `100 − (footprint × 0.6) − (concentration × 0.25)`. Inverse of exposure.
- Both produce numbers that map cleanly onto the 0–30 / 31–60 / 61–80 / 81–100 bands defined in the prompt.

---

## What the dashboard reveals

**1. One ecosystem owns 35% of the entire footprint.**
Alphabet's 4 services (Search, Pay, Photos, YouTube) are responsible for **35.3 of the 99.0 total exposure score** across all 15 services. That's not a small bias — it's the dominant single fact about this user's digital life. Meta follows at 14.6. ByteDance and Amazon tie at 9.0 each.

**2. Search is the single most valuable asset on the stack.**
Google Search scores 100/100 on the exposure heatmap (10/10 sensitivity × 10/10 tracking). It captures *intent before action* — the single hardest signal to fake and the single most commercially valuable.

**3. Behavioral risk is the peak axis, not Financial or Identity.**
On the 6-dimensional risk radar, Behavioral hits **7/10**, Identity **6**, Location **6**, Financial **5**, Communication **4**, Biometric **3**. The combination of TikTok + YouTube + Instagram + Google Search creates a behavioural profile denser than any single app reveals.

**4. The biggest improvement lever isn't quitting an app.**
The Privacy Improvement Simulator shows that turning off Google ad personalization (+6 points) and disabling location history (+5 points) — actions that take ~5 minutes — lift the Privacy Score from 50 (Fair) to 61+ (Good). No app deletion required.

**5. Categorical gaps are honestly labeled.**
The dashboard displays **"Not enough information provided"** for income bracket, exact location precision, health conditions, and political leaning. Every behavioural inference is tagged with an orange **EST** badge. Every count or service-level fact is tagged with a green **FACT** badge. The line between data and inference is visible, not hidden.

---

## Sections shipped

1. **KPI strip** — Services, parent companies, ecosystem concentration (HHI), tracking surface
2. **Score cards** — Digital Footprint (79) + Privacy (50) with explainable contributors
3. **Exposure Heatmap** — 15-cell grid colored by tracking × sensitivity
4. **Company Exposure Ranking** — Parent-weighted, bar-chart sorted
5. **Data Collection Matrix** — Full per-service table with sensitivity pill, tracking pill, and explicit data types
6. **Risk Radar** — 6-axis SVG polygon, no chart library
7. **Digital Twin Profile** — 4 quadrants (Demographic Est, Behavioural Est, Service Facts, Inference Gaps)
8. **Most Valuable Data Assets** — 6 grade cards (A+ to B+)
9. **Privacy Improvement Simulator** — 8 interactive actions, live score recalculation
10. **Final Verdict** — One pull-quote + three prioritized actions

---

## Key learnings

**1. The dashboard's job is to make invisible relationships legible.**
The user already "knows" they use Google services. The dashboard's value isn't restating that — it's showing that *4 services equals 35% of total exposure*. That math, made visual, is the entire product.

**2. Facts vs Estimates labelling is the trust layer.**
A privacy report that claims to know your income is a privacy report you don't trust. The prompt's rule — every behavioural conclusion tagged as Estimate, every count tagged as Fact — is the single most important design constraint, not a footnote. The orange/green pill is doing more reputational work than any chart.

**3. The Ecosystem Concentration metric isn't a vanity number — it's the diagnosis.**
HHI of 12.9 with one entity at 26.7% market share *of one user's life* is a structural finding. It reframes the conversation from "which apps are bad" to "which single company holds disproportionate inference power over you."

**4. The simulator is the only section that changes the user's behaviour.**
The scores diagnose. The simulator prescribes. Every other section can be read passively; the simulator forces the user to click and see the score move. That feedback loop is what converts a dashboard from "interesting" to "actionable."

**5. The Final Verdict should not summarize — it should commit.**
Generic verdict: *"Your privacy could be improved."* Useful verdict: *"The single biggest lever isn't quitting an app — it's narrowing one ecosystem (Google) that currently sees over a third of your digital life."* The second one is a position. The first is a sentence.

---

## Honoring the prompt's critical rules

| Rule | How it shows up |
|---|---|
| Never claim access to private databases | All inferences computed from the 15 self-reported services only; no external lookups, no breach databases, no scraping language used. |
| Never claim certainty about inferred traits | Every demographic and behavioural claim tagged with `EST` badge. No "you are" statements; only "likely" / "Estimated" / "Not enough information provided." |
| Separate Facts from Estimates | Twin Profile splits into 4 explicit quadrants. Data Collection Matrix shows raw service-level facts. KPI strip shows counts (Facts). Risk Radar values labeled as exposure model output, not claims. |
| Single HTML artifact starting with `<style>` | File opens with `<style>` block, ~34 KB, no external dependencies, no markdown wrapper. |

---

## Tech notes

- Single HTML file. ~34 KB. No frameworks, no CDN, no images.
- SVG for the Risk Radar polygon (radial gradient fill + 6-axis grid, hand-computed coordinates).
- Pure CSS grid for the heatmap, score cards, asset cards, and twin profile. No JS layout libraries.
- One small inline `<script>` for the Privacy Improvement Simulator — sums checked points, recomputes Privacy Score, recolors the band live.
- Notion / Stripe / Linear inspired: dark surface, generous spacing, monochrome with two accents (violet 7c5cff, cyan 00d4ff), pill-shaped badges for taxonomy.

---

## What surprised me most

That the **Ecosystem Concentration metric** turned out to be more interesting than either headline score. Footprint of 79 is high — but you'd expect that with 15 active services. Privacy of 50 is fair — also unsurprising. What's actually surprising is that a *single corporate parent sees 35% of the entire digital life of this user*. That's the line the dashboard should lead with, and it's the line the Final Verdict pulls forward.

Most privacy reports tell you which apps are dangerous. This one tells you which *company* is structurally over-represented. That reframe — from app to entity — is the part worth keeping.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
