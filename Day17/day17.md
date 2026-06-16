# Day 17 — AI Vehicle Cost & Fuel Analysis Dashboard

**Project:** CSV → interactive HTML dashboard with KPIs, SVG charts, E85 paradox analysis
**Challenge:** #60DayClaudeChallenge — Day 17
**Subject Vehicle:** Tesla Model 3 · EV · 2 years old · 1200 km/month · Dallas, TX
**Dataset:** 52 vehicles, 5 fuel types (India fleet sample, INR pricing)

> **Note on currency:** The provided dataset is from an India fleet sample (INR pricing, Indian fuel types including E85 Flex-Fuel and E20 Petrol). I'm based in Dallas, TX — the methodology and dashboard layout apply globally, but the absolute INR numbers reflect Indian market conditions. The relative comparisons (EV vs Petrol vs E85) are the meaningful insight.

---

## Files

- `vehicle-dashboard.html` — the interactive HTML dashboard (single file, no CDN, pure SVG)
- `day17.md` — this writeup
- `screenshots/` — dashboard screenshots

---

## Dataset Snapshot

| Fuel Type | n | Cost/km (₹) | CO₂ g/km | Maint/km (₹) | Refuel (min) | Mileage |
|---|---|---|---|---|---|---|
| **Electric (EV)** | 11 | **1.75** ⬇ | 91 | **0.21** ⬇ | 45 ⬆ | 6.6 km/kWh |
| CNG | 10 | 3.34 | 125 | 0.63 | 8 | 22.9 km/kg |
| Diesel | 9 | 4.62 | 177 ⬆ | 0.89 | 5 | 20.9 km/L |
| Petrol (E20) | 11 | 6.21 | 172 | 0.41 | 5 | 17.1 km/L |
| **E85 (Flex-Fuel)** | 11 | **6.37** ⬆ | **69** ⬇ | 0.43 | 5 | 13.0 km/L |

---

## Dashboard Sections (per prompt spec)

1. **Header** — Tesla Model 3 · EV · Age 2y · 1200 km/mo
2. **5 KPI Cards** — Your cost/km, E85 cost/km, E85 premium vs petrol, break-even price, your monthly cost
3. **SVG Bar Chart** — Cost per km across all 5 fuels (color-coded, hover tooltips)
4. **SVG Doughnut** — CO₂ per km share with legend (hover tooltips)
5. **SVG Line Chart** — Cost per km vs vehicle age (0–12y), vertical line marks YOUR age = 2y
6. **SVG Animated Gauge** — E85 Score (5.7/10) with verdict sentence
7. **5 Fuel Cards** — Your EV highlighted with glow; each card has 2 pros, 2 cons, "best-for" tag

All SVG. No external CDN. Dark navy `#0a0f1e` background with glassmorphism. Color coding: EV=purple, Petrol=blue, Diesel=grey, CNG=green, E85=amber.

---

## Key Findings from the Data

### 🔥 The E85 Paradox (most surprising finding)

E85 is **18% cheaper at the pump** than Petrol (₹82 vs ₹100 per liter), but **2.6% more expensive per kilometer driven** (₹6.37 vs ₹6.21).

**Why?** E85's mileage is 13 km/L vs Petrol's 17.1 km/L — you burn 31% more fuel per km, completely offsetting the cheaper pump price. The **break-even E85 price is ₹76.02/L** — current pump is ₹82, meaning E85 is still ₹6 over breakeven economically.

**The trap:** Buyers compare pump prices, not cost per km. Without this dashboard, an E85 driver would feel like they're saving money while actually spending more.

### ⚡ EV cost economics are dominant

EV costs **₹1.75/km** vs Petrol **₹6.21/km** — a **3.5× cost difference**. At 1200 km/month:
- EV monthly: ₹2,100
- Petrol monthly: ₹7,452
- **Annual savings vs Petrol: ₹64,224** (~$770 USD equivalent)

### 🌍 E85 wins on CO₂, EV wins on cost, Diesel loses on both

E85's 69 g CO₂/km is the **cleanest liquid fuel** in the dataset. EV's 91 g/km is grid-mix-dependent (could be lower with renewables). Diesel at 177 g/km is the **dirtiest** and the most expensive to maintain past 6 years (+50% maintenance cost jump).

### 📈 Age effect on cost/km

Cost per km creeps up with age across every fuel:
- **EV:** Almost flat (1.71 → 1.82) — the most predictable long-term TCO
- **CNG:** +13% from New to Aged
- **Petrol:** +8% from Mid to Aged
- **Diesel:** +22% from Mid to Old (steepest) — diesel ages poorly

The line chart shows EV as the flattest line — a stability signal for long-term ownership.

---

## How Visualization Helped Understand the Data

1. **The bar chart** made the EV cost advantage immediately obvious — it's not a 20% gap, it's a **3.5× gap**. A table would've shown the numbers but not the visceral asymmetry.

2. **The doughnut chart** revealed E85's hidden environmental advantage. Looking at the table, E85 is "just another fuel." Looking at the doughnut, E85's slice is visibly the smallest — instant pattern recognition for "the green fuel."

3. **The line chart with the vertical "YOU" marker** turned an abstract age-cost relationship into a personal projection. As a 2-year-old EV, the dashboard literally points to where your costs sit *today* and shows the flat trajectory ahead.

4. **The gauge** condensed a 4-criterion scoring framework (cost/CO₂/refuel/maint) into one number with one verdict sentence. Easier to share, easier to remember.

5. **The fuel cards with the highlight glow** made the comparison personal: "you're here, here's what the others would mean."

---

## Key Learnings

**1. Visualization isn't decoration — it's compression.** Five fuel types × 7 metrics × 4 age buckets = 140 data points. The dashboard reduces that to ~6 visual elements while preserving every comparison. Compression is the actual value of dashboards.

**2. The most useful insight is usually counterintuitive.** "E85 is cheaper at the pump but more expensive per km" is the insight a buyer will share at a dinner party. A dashboard's job is to surface those counterintuitive moments, not just summarize averages.

**3. Personalize the reference point.** Adding "YOUR vehicle = 2y, EV" anchors every chart to a user-specific viewpoint. Without that anchor, dashboards feel generic and forgettable.

**4. CSV → dashboard is a 1-prompt workflow now.** What used to be a 3-day BI project (clean data, build calculations, design viz, ship) is now a single prompt with clear specs. The bottleneck moves from execution to specification.

**5. Constraints inside the prompt produce better designs.** "Dark navy, glassmorphism, pure SVG, no CDN, specific color per fuel, 5 KPI cards, 6 chart sections, mobile responsive" — every constraint forced cleaner choices. Loose prompts produce loose designs.

**6. Methodology travels; data doesn't always.** The CSV was India-context (INR, E85, CNG common). Living in Dallas, my actual fuel prices and choices differ. But the *workflow* — pull CSV, compute by group, visualize comparisons, anchor to user — applies to any market with any dataset.

---

## What surprised me most

The E85 paradox. I went in assuming "cheaper pump price = cheaper to drive." The dashboard disproved that in 30 seconds. That's the kind of insight that protects buyers from making decisions based on the wrong reference number. Most fuel buyers in flex-fuel markets are probably making the same mistake.

This is the entire job of a business analyst — finding the reference number people *should* be using instead of the one they *are* using.

---

## Tech Stack

- Python (for CSV preprocessing + metric verification)
- Pure HTML + inline CSS + vanilla JS (single file, no build step)
- Pure SVG charts (no Chart.js, no D3, no CDN dependency)
- Glassmorphism + dark theme
- Mobile responsive (375px → 1440px)
- Hover tooltips via vanilla JS

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
