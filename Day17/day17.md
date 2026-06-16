# Day 17 — AI Vehicle Cost & Fuel Analysis Dashboard

**Project:** CSV → interactive HTML dashboard with KPIs, SVG charts, E85 paradox analysis
**Challenge:** #60DayClaudeChallenge — Day 17
**Subject Vehicle:** Tesla Model 3 · EV · 2 years old · 1,000 mi/month · Dallas, TX
**Dataset:** 52 vehicles, 6 fuel types (US-context, USD pricing, miles)

---

## Files

- `vehicle-dashboard.html` — the interactive HTML dashboard (single file, no CDN, pure SVG)
- `day17_us_fuel_dataset.csv` — the 52-vehicle US dataset
- `day17.md` — this writeup
- `screenshots/` — dashboard screenshots

---

## Dataset Snapshot

| Fuel Type | n | Cost/mi ($) | CO₂ g/mi | Maint/mi ($) | Refuel (min) | Avg MPG |
|---|---|---|---|---|---|---|
| **Electric (EV)** | 10 | **0.044** ⬇ | **135** ⬇ | **0.06** ⬇ | 30 ⬆ | 110 mpge |
| Hybrid | 6 | 0.080 | 203 | 0.08 | 5 | 48 |
| Regular Gasoline | 11 | 0.133 | 338 | 0.09 | 5 | 28 |
| **E85 Flex-Fuel** | 10 | **0.139** | 323 | 0.11 | 5 | 22 |
| Diesel | 8 | 0.139 | 355 | 0.14 | 5 | 32 |
| Premium Gasoline | 7 | 0.174 ⬆ | 378 ⬆ | 0.13 | 5 | 26 |

---

## Dashboard Sections (per prompt spec)

1. **Header** — Tesla Model 3 · EV · Age 2y · 1,000 mi/mo
2. **5 KPI Cards** — Your cost/mi, E85 cost/mi, E85 premium vs Reg Gas, break-even price, your monthly cost
3. **SVG Bar Chart** — Cost per mile across all 6 fuels (color-coded, hover tooltips)
4. **SVG Doughnut** — CO₂ per mile share with legend (hover tooltips)
5. **SVG Line Chart** — Cost per mile vs vehicle age (0–12y), vertical line marks YOUR age = 2y
6. **SVG Animated Gauge** — E85 Score (4.2/10) with verdict sentence
7. **6 Fuel Cards** — Your EV highlighted with glow; each card has 2 pros, 2 cons, "best-for" tag

All SVG. No external CDN. Dark navy `#0a0f1e` background with glassmorphism. Color coding: EV=purple, Hybrid=green, Reg Gas=blue, Premium=cyan, Diesel=grey, E85=amber.

---

## Key Findings from the US Data

### 🔥 The E85 Paradox (the headline insight)

E85 is **20% cheaper at the pump** than regular gasoline ($2.80 vs $3.50 per gallon), but **4.3% more expensive per mile driven** ($0.139 vs $0.133).

**Why?** E85 averages 22 mpg vs regular gas at 28 mpg — you burn 27% more fuel per mile, completely offsetting the cheaper pump price. The **break-even E85 price is $2.75/gallon** — current pump is $2.80, meaning E85 is still $0.05/gal over its breakeven economically.

**The trap:** Buyers compare pump prices, not cost per mile. Without this dashboard, an E85 driver would feel like they're saving 20% while actually spending more.

This paradox holds across both US and India markets — only the magnitude changes. The mechanism is the same: ethanol has ~33% lower energy density than gasoline, so mileage degradation wipes out pump savings.

### ⚡ EV cost dominance

EV costs **$0.044/mile** vs Regular Gas **$0.133/mile** — a **3× cost difference**. At 1,000 mi/month:
- EV monthly: $44
- Regular Gas monthly: $133
- **Annual savings vs Regular Gas: $1,068**
- Annual savings vs Premium Gas: $1,560

### 🌍 EV wins CO₂ in US too — but for a different reason

E85 reduces CO₂ via partial renewability (15% emissions reduction vs gasoline). EV wins outright at 135 g/mi vs Reg Gas 338 g/mi — but this depends on the **US grid mix**. In Texas (~40% renewable in 2026), the EV number drops further. In a coal-heavy grid state it's higher.

### 📈 Age effect on cost/mile (US data)

Cost per mile creeps up with age across every fuel:
- **EV:** Almost flat ($0.040 → $0.049, +22% over 10 years) — but only because efficiency degrades slowly
- **Hybrid:** +13% from New to Aged
- **Regular Gas:** +11% from New to Aged
- **E85:** **+21% from New to Old** — steepest decline due to FFV engine deposits
- **Diesel:** +23% from Mid to Old — diesel ages worst
- **Premium:** +18% over 10 years

The line chart shows EV as the flattest line — best long-term TCO predictability.

### 🎯 The Tesla Model 3 case

At 1,000 mi/month and your age 2y EV, your total annual fuel cost is **~$528**. If you'd bought a regular gas sedan, it would be **~$1,596**. The **$1,068/year savings** pays back the EV premium (~$8,000-12,000 typical) in roughly **8-11 years** — assuming flat gas prices, which is generous to gas.

---

## How Visualization Helped Understand the Data

1. **The bar chart** made the EV cost advantage immediately obvious — it's not a 20% gap, it's a **3× gap**. A table would've shown the numbers but not the visceral asymmetry.

2. **The doughnut chart** revealed how close E85 and Regular Gas sit on CO₂ (323 vs 338 g/mi) — most people assume E85 is "much greener." The data says only ~4% greener per mile.

3. **The line chart with the "YOU · 2y" vertical marker** turned an abstract age-cost relationship into a personal projection. As a 2-year-old EV, the dashboard literally points to where your costs sit *today* and shows the flat trajectory ahead.

4. **The gauge** condensed a 4-criterion scoring framework (cost/CO₂/refuel/maint) into one number (4.2/10) with one verdict. Easier to share, easier to remember.

5. **The fuel cards with the highlight glow** made the comparison personal: "you're here, here's what the others would mean."

---

## Key Learnings

**1. Visualization isn't decoration — it's compression.** Six fuel types × 7 metrics × 4 age buckets = 168 data points. The dashboard reduces that to ~6 visual elements while preserving every comparison. Compression is the actual value of dashboards.

**2. The most useful insight is usually counterintuitive.** "E85 is cheaper at the pump but more expensive per mile" is the insight a US car buyer would share at a Texas barbecue. A dashboard's job is to surface those counterintuitive moments, not just summarize averages.

**3. Personalize the reference point.** Adding "YOUR vehicle = 2y, EV" anchors every chart to a user-specific viewpoint. Without that anchor, dashboards feel generic and forgettable.

**4. CSV → dashboard is a 1-prompt workflow now.** What used to be a 3-day BI project (clean data, build calculations, design viz, ship) is now a single prompt with clear specs. The bottleneck moves from execution to specification.

**5. Constraints inside the prompt produce better designs.** "Dark navy, glassmorphism, pure SVG, no CDN, specific color per fuel, 5 KPI cards, 6 chart sections, mobile responsive" — every constraint forced cleaner choices. Loose prompts produce loose designs.

**6. The paradox transcends geography.** The E85 economics work the same way in India (with INR/km) and the US (with USD/mile). The mechanism — ethanol's lower energy density — doesn't care about currency. Good dashboards expose mechanism, not just numbers.

---

## What surprised me most

The E85 paradox. I went in assuming "cheaper pump price = cheaper to drive." The dashboard disproved that in 30 seconds. That's the kind of insight that protects buyers from making decisions based on the wrong reference number.

Most flex-fuel buyers in Texas, Iowa, and Minnesota — the three biggest E85 markets — are probably making this exact mistake. They look at the $0.70/gal discount at the pump and feel like they're saving. The data says they're not.

This is the entire job of a business analyst — finding the reference number people *should* be using instead of the one they *are* using.

---

## Tech Stack

- Python (for CSV generation + metric verification)
- Pure HTML + inline CSS + vanilla JS (single file, no build step)
- Pure SVG charts (no Chart.js, no D3, no CDN dependency)
- Glassmorphism + dark theme
- Mobile responsive (375px → 1440px)
- Hover tooltips via vanilla JS

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
