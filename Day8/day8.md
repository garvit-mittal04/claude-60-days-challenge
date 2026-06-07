# Day 8 — Build Your First AI-Powered Dashboard

**Project:** 🌍 Personal Environmental Health Analyzer
**Challenge:** #60DayClaudeChallenge — Day 8
**Date:** June 7, 2026
**Deliverable:** Interactive HTML dashboard built entirely with Claude Artifacts

---

## Overview

Built a fully interactive, dark-themed environmental health dashboard that analyzes AQI, PM2.5, PM10 and water quality across 22 major US cities. The dashboard is a single self-contained `index.html` file — no build step, no backend, drop it anywhere and it runs.

Open `index.html` in any modern browser.

---

## Screenshots

> Drop screenshots of the dashboard here before committing:
>
> - `screenshots/01-overview.png` — top of dashboard with key metrics
> - `screenshots/02-charts.png` — AQI / PM2.5 / PM10 / ranking / distribution charts
> - `screenshots/03-report-card.png` — personal report card with score circle and grades
> - `screenshots/04-insights.png` — insights panel + city cards grid
> - `screenshots/05-recommendations.png` — personalized recommendations
> - `screenshots/06-mobile.png` — mobile responsive view

---

## What the dashboard does

**Key Metrics:** average AQI, highest/lowest AQI city, number of cities analyzed, environmental health score.

**Visualizations:**
- AQI comparison bar chart, color-coded by US EPA category
- PM2.5 line chart (µg/m³)
- PM10 line chart (µg/m³)
- City ranking horizontal bar (cleanest → most polluted)
- AQI distribution doughnut by category

**Interactive Filters:** city selector, AQI min/max range, pollutant toggle, health-risk filter, date filter, and a side-by-side city compare mode.

**City Detail Cards:** click any card to focus — shows AQI, PM2.5, PM10, air-quality category, air score and water-quality score.

**Environmental Health Analysis (per selected city):**
- Air impact on lungs, sleep, energy, exercise, long-term health
- Water impact on hair fall, hair dryness, scalp, skin dryness, acne, sensitive skin
- 🟢 Low / 🟡 Moderate / 🔴 High risk tags

**Personal Report Card:**
- Environmental Health Score (0–100) with conic-gradient score ring
- Air Quality Score, Water Quality Score, Overall Score with progress bars
- A–F grades for Air Quality, Water Quality, Hair Risk, Skin Risk

**Insights Panel:** top 3 cleanest, top 3 most polluted, biggest anomaly, most surprising observation.

**Personalized Recommendations:** daily actions, indoor air improvements, outdoor activity guidance, hair-care, skin-care, water-quality improvements — all dynamically tailored to the selected city.

---

## Data

Representative recent readings for 22 US cities based on EPA AirNow and IQAir public data. AQI buckets follow the official US EPA scale: Good (0–50), Moderate (51–100), Unhealthy for Sensitive Groups (101–150), Unhealthy (151–200), Very Unhealthy (201–300), Hazardous (301+).

Sources cited in the dashboard footer: airnow.gov, AQICN, IQAir, EPA Safe Drinking Water Information System.

---

## Key Learnings

**1. Claude Artifacts are real applications, not snippets.** I expected a code dump and got a working product — charts that actually render, filters that actually filter, a score ring that actually animates. The mental model shift from "AI as writer" to "AI as builder" is the headline takeaway.

**2. Specification quality directly drives output quality.** The prompt was dense — five roles, exact metric list, exact chart list, exact category colors. The output mapped 1:1 to the spec. Vague prompts produce vague dashboards; structured prompts produce structured ones.

**3. A single HTML file beats a framework when shareability matters.** No npm, no build, no deploy. One file you can email, drop in a GitHub repo, or open offline. For LinkedIn-shareable demos this format wins.

**4. Cross-domain analysis is where AI dashboards shine.** Air quality and water quality usually live in different reports. Joining them into one Environmental Health Score — and surfacing the mismatch as the "most surprising observation" — is the kind of insight that's tedious to build manually but trivial to ask Claude for.

**5. Dark themes + Chart.js + CSS conic-gradient = premium UI on zero budget.** No design system needed. The score ring, gradient cards, and color-coded bars all come from primitives.

**6. Personalization without backend.** Every recommendation (mask use, HEPA purifier, hair oiling, RO + UV) recalculates the moment you click a different city card. No API, no user account — just reactive client-side logic Claude wrote correctly on the first pass.

---

## What surprised me most

The dashboard updates instantly on every filter change — and Claude wired up the entire reactivity layer (city click → report card scroll → chart redraw → recommendation refresh) without being told to. I expected to spend time wiring callbacks; instead I spent time admiring them.

Also surprising: the data story itself. Seattle, Honolulu, and Boston deliver the cleanest air, but California's Central Valley (Bakersfield, Fresno, Visalia) tops the polluted list — agricultural dust and trapped inversions, not wildfire alone. And the southwest desert cities (Phoenix, Las Vegas) have moderate air but very hard water, meaning hair and skin issues show up before lung issues do. That framing changed how I think about "pollution."

---

## Tech Stack

- HTML5 + CSS3 (no frameworks)
- Chart.js 4.4 (via CDN) — bar, line, doughnut, horizontal bar
- Vanilla JS — filter state, reactive renders, conic-gradient score ring
- Dark theme with multi-radial-gradient background
- Mobile responsive (single-column on <720px)

---

## Files in this folder

- `index.html` — the dashboard (open in any browser)
- `day8.md` — this file
- `screenshots/` — dashboard screenshots

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
