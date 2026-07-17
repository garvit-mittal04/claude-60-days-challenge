# Day 48 — Compare & Decide Builder

**Project:** `compare-decide-builder.html` — an interactive weighted-criteria decision tool with sourced data
**Challenge:** #60DayClaudeChallenge — Day 48
**Comparison selected:** LLM APIs — Claude Sonnet 4 vs GPT-4.1 vs Gemini 2.5 Pro vs Llama 3.3 70B
**Sourcing:** Real public sources (vendor pricing pages, official model cards, SWE-bench leaderboard) with estimates and judgments flagged
**Type:** Single-file HTML/CSS/JavaScript. No frameworks, no CDN.

---

## What I built

An interactive comparison tool that ranks four frontier LLM APIs against five criteria and lets you adjust the weight of each criterion with sliders. Ranking updates live as you drag. Every data point is either sourced from a publicly-linkable reference or explicitly flagged as an estimate or judgment call.

Five presets baked in — Indie dev, MSBA student, Team lead, Enterprise, Balanced — each with a distinct weighting profile. Click a preset and watch the ranking rearrange.

---

## The four-question intro

Per the spec, I asked four MCQ-format questions:

1. **Which comparison?** → LLM APIs (given user's "latest trends" pick — this is the hottest ongoing comparison in the AI space)
2. **Audience?** → Everyone (built with 5 presets covering distinct personas)
3. **Criteria?** → My pick, given "see according to you"
4. **Sourcing?** → Real sources + weightable criteria (the strong-recommended option)

---

## The five criteria

Chose these because they're all publicly measurable and cover the four biggest levers in an API decision:

| Criterion | Direction | Source type | Flag |
|---|---|---|---|
| **Blended $/M tokens** | Lower better | Vendor pricing pages | EST (assumes 2:1 input:output ratio) |
| **MMLU (reasoning)** | Higher better | Vendor model cards | Sourced |
| **SWE-bench Verified (coding)** | Higher better | Vendor reports + SWE-bench leaderboard | Sourced |
| **Context window (K tokens)** | Higher better | Vendor API docs | Sourced |
| **Openness / Deployment** | Higher better | Judgment (1-3 proprietary → 8-10 open weights) | JUDGMENT |

Every table cell displays its source name and, if applicable, an `EST` or `JUDGMENT` flag so the user knows what's citable vs. what's my call.

---

## The data (May 2025 snapshot)

Publicly-documented numbers from vendor pages:

| | Claude Sonnet 4 | GPT-4.1 | Gemini 2.5 Pro | Llama 3.3 70B |
|---|---|---|---|---|
| Blended $/M | 7.00 | 4.00 | 4.17 | 0.60 |
| MMLU | 88 | 88 | 85 | 86 |
| SWE-bench Verified | 65 | 55 | 63 | 41 |
| Context (K) | 200 | 1,000 | 1,000 | 128 |
| Openness | 3 | 3 | 3 | 9 |

**Sources cited** in the visible Sources panel:
- Anthropic API pricing page
- Anthropic Claude Sonnet 4 model card
- OpenAI API pricing page
- OpenAI GPT-4.1 launch announcement (April 2025)
- Google Gemini API pricing
- Google Gemini 2.5 documentation
- Together AI pricing (host of choice for Llama)
- Meta Llama 3.3 model card
- SWE-bench Verified leaderboard (swebench.com)

Every one is linkable and verifiable.

---

## The five presets

Each preset is a specific weighting profile that models a persona:

- **👩‍💻 Indie dev** — Price 8, SWE-bench 7, Openness 6, Context 5, MMLU 4. Cost-sensitive, cares about openness and coding capability.
- **🎓 MSBA student** — Price 9, SWE-bench 6, MMLU 5, Context 4, Openness 3. Budget-first learning.
- **👥 Team lead** — SWE-bench 7, Context 6, MMLU 6, Price 6, Openness 4. Balanced with capability lean.
- **🏢 Enterprise** — Openness 8, Context 7, MMLU 6, SWE-bench 6, Price 4. Deployment flexibility and control matter most.
- **⚖️ Balanced** — All at 5. The starting point for exploration.

Under **Indie dev**, Llama tops the ranking because openness and price dominate. Under **Enterprise**, Claude and Gemini compete based on how you weight openness. This is the whole point of a weighted-criteria tool — the "right" answer depends on what you care about.

---

## The methodology panel

A collapsible "How this was researched" section at the bottom explains:

- **Blended cost estimation** — the 2:1 input:output ratio assumption and when it breaks
- **Openness scoring rubric** — how I mapped proprietary vs open weights to a 1-10 scale
- **Conflicts resolved** — Llama pricing varies by host (Together vs Groq vs Fireworks), context windows are advertised but degrade past ~200K, SWE-bench versions aren't cross-comparable
- **Update cadence** — this space changes every 30-60 days; rerun quarterly

Naming the conflicts is what makes the tool trustworthy. Hiding them would be easier and less useful.

---

## Tech notes

- **Single HTML file.** ~28 KB. Vanilla CSS + JavaScript. No frameworks, no CDN.
- **Live weighting** — sliders update the composite score in real time; ranking cards physically rearrange.
- **Criterion normalization** — for each criterion, values are normalized 0-100 across the 4 options. Weights are relative (don't need to sum to any target).
- **Preset system** — clicking a preset instantly rewrites all 5 weights and re-renders.
- **Sources panel** always visible at the bottom.
- **Methodology panel** collapsible.
- **Dark mode default + light mode toggle** via CSS custom properties.
- **`prefers-reduced-motion`** respected.
- **Fully responsive** — grid collapses to single-column below 760px.

---

## What this exercise demonstrates

### 1. A comparison tool is a values-elicitation tool

The point of the sliders isn't to compute a "right" answer. It's to force the user to declare what they actually care about. The moment someone slides Price down and Openness up, they've told themselves something about their real preferences that a fixed ranking never surfaces.

### 2. Sources are the whole product

A weighted comparison with unsourced data is astrology. Every cell in the data table has a citation label, and every citation has a linkable source in the panel below. The methodology panel names what's estimated and why. Trust in the tool is entirely a function of visible sourcing.

### 3. Naming what you don't know is credibility

The transparency callout at the top explicitly says: *"Data reflects publicly-available specs and pricing as of the assistant's May 2025 knowledge cutoff. Numbers move fast in this space — always verify against the linked pricing pages before committing."* That single sentence does more for credibility than a hidden footnote ever would.

### 4. Presets teach the tool

Five personas map to five weight profiles. Clicking through them is how you learn what the sliders actually do. The presets aren't a shortcut — they're a tutorial.

---

## Narrative continuity

- **Day 45** — Decision Strategist (single-decision analysis)
- **Day 46** — Autonomous Agent Studio (sequential multi-agent)
- **Day 47** — Content Intelligence Studio (parallel multi-agent)
- **Day 48 — Compare & Decide Builder (interactive weighted-criteria)**

Decision-tools arc caps here. Day 45 was one decision, deeply analyzed. Day 48 is many options, transparently ranked. Together they cover both endpoints of the "help me decide" spectrum — the deep single dive and the wide comparative sweep.

---

## Tags

`#60DayClaudeChallenge` `#DecisionMaking` `#LLMs` `#AITools`
