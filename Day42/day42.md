# Day 42 — Personal Financial Command Center

**Project:** `personal-financial-command-center.html` — a full-featured personal finance dashboard
**Challenge:** #60DayClaudeChallenge — Day 42
**Profile selected:** Salaried Professional · Balancing savings, debt, and investments
**Structure:** Claude auto-designed the full module set
**Type:** Single-file vanilla HTML/CSS/JavaScript. No frameworks, no CDN, no images, no APIs. Works fully offline. localStorage persistence.

---

## What I built

A commercial-grade personal finance dashboard designed for a salaried professional balancing three simultaneous priorities: emergency savings, debt payoff, and long-term investing. Twelve tabbed modules, a live-computed financial health score, an SVG portfolio allocation chart, a cash flow bar chart, a debt-payoff strategy comparator (avalanche vs. snowball), a compound-growth what-if simulator, and rule-based AI recommendations.

The whole thing runs in one 90 KB HTML file. Every data change writes to `localStorage` immediately — the dashboard survives reloads. Nothing leaves the browser.

---

## The three-question intro (per task spec)

Before generating anything, I asked three MCQ-format questions:

1. **Who is the dashboard for?** — user chose **Salaried Professional** (W-2 income, mid-career).
2. **Which financial goal to prioritize?** — user chose **Balance all three** (savings + debt + investments).
3. **Auto-design or customize modules?** — user chose **auto-design**.

That combination locked in the full module set weighted equally across the three priorities.

---

## The twelve modules

Every module is a distinct tab. State flows between them — enter your income in one tab and every other tab updates its computed metrics immediately.

**📊 Overview.** The centerpiece. Displays the composite financial health score (0-100) with a four-dimension breakdown (savings rate, emergency runway, debt-to-income, net worth). Below that, six live stat tiles for cash flow, and a categorical bar chart of every monthly outflow.

**💵 Income.** Annual salary, bonus, side income, effective tax rate — with computed monthly gross, monthly net, and estimated tax withheld.

**🧾 Expenses.** Eight expense categories (housing, food, transportation, utilities, healthcare, entertainment, shopping, other) with editable monthly amounts. A category breakdown shows what percentage of monthly total each category consumes.

**📋 Budget.** The classic 50/30/20 rule visualization. Splits your actual spending into Needs / Wants / Savings buckets and compares each to its target ratio. Callouts explain when housing is heavy or when savings are underfunded.

**🏦 Savings.** Emergency fund, high-yield savings, checking balances. Computes emergency runway in months of essentials, shows progress toward the 6-month target, and explains where to keep emergency money (high-yield savings, not stocks).

**💳 Debt.** Full debt list with balances, interest rates, and minimum payments. Computes weighted average rate, monthly interest cost, and debt-to-income ratio. Below that, a side-by-side comparison of Avalanche (highest-rate first, mathematically optimal) vs. Snowball (smallest-balance first, psychologically motivating) with the exact payoff order for each strategy given your actual debts.

**📈 Investments.** 401(k), IRA, brokerage balances plus monthly contribution rate. Computes projected retirement balance at 65 (assuming 7% real return). Below, an SVG pie chart of asset allocation (stocks / bonds / cash / crypto) with editable percentages and a "110 minus your age" allocation rule callout.

**🔁 Subscriptions.** The silent budget leak module. Every subscription lists monthly and annual cost. The stat tile at the top shows "= Invested @ 7% in 10 years" — turning annual subscription cost into forgone investment gain to sharpen the trade-off.

**🎯 Goals.** Named goals with target amount and deadline. For each, computes progress percent, remaining amount, and monthly savings needed to hit the deadline. Includes house down payment, wedding, vacation, and emergency fund as defaults.

**🔮 What-If.** Three interactive sliders: extra debt payment per month, extra investment per month, annual raise. Each recomputes downstream effects — extra debt payment's interest savings, extra investment's compound future value, raise's savings-rate lift.

**💡 Insights.** Rule-based AI recommendations computed from the current dashboard state. Ranked by impact — high-rate debt beats subscription review, which beats general savings rate advice. If everything looks healthy, it names the specific next-level move (Roth IRA max, tax-loss harvesting, house down payment).

**📚 Resources.** Books, communities, checklist items, and copy-paste AI prompts for continuing to learn.

---

## The financial health score

Composite 0-100 score computed from five dimensions:

- **Savings rate** — 30 points. 20%+ = full, sliding scale down.
- **Emergency runway** — 25 points. 6+ months = full, sliding down to 0 for less than 1 month.
- **Debt-to-income** — 20 points. Under 10% = full, sliding down. Over 36% (lender risk threshold) drops fast.
- **Investment velocity** — 15 points. 15% of gross going to investments = full.
- **Net worth** — 10 points. Positive net worth = 6 points minimum, higher for exceeding 1 year's gross.

The score displays with a color-coded band (Excellent / Strong / Fair / Building / Needs Work) and a four-tile breakdown so the user can see exactly which lever moves the score most.

---

## The what-if simulator

Three sliders let the user explore trade-offs:

- **Extra debt payment.** Slider from $0 to $1000/mo. Shows approximate 3-year interest saved and estimated payoff acceleration on the credit card.
- **Extra investment.** Slider from $0 to $1500/mo. Compounds forward at 7% until age 65 using the exact FV formula: `PMT × [((1+r)^n - 1) / r]`.
- **Annual raise.** Slider from $0 to $30,000. Computes net monthly increase, shows new savings rate if 100% of the raise is saved, and warns about lifestyle inflation (60-80% of raises get absorbed by lifestyle if not deliberately saved).

Every slider triggers live recalculation — no submit button.

---

## The AI recommendations engine

The Insights tab uses a rule-based engine (not an LLM call) to prioritize advice by impact-to-effort ratio. The priority stack:

1. Emergency fund below 3 months → highest priority (bad callout).
2. High-rate debt (>15%) → second priority (bad callout).
3. Savings rate below 10% → third priority (warn).
4. Excessive subscriptions (>$100/mo) → warn.
5. Debt-to-income above 36% → warn.
6. Missing likely 401(k) match → warn.
7. Overweight crypto (>15%) → warn.
8. Everything healthy → positive reinforcement + next-level suggestions (tax-loss harvesting, Roth IRA max).

Every recommendation names a specific dollar impact so the user knows exactly what fixing it is worth. Not "save more" — "target $9,500 more into your high-yield savings account."

---

## Tech notes

- **Single HTML file.** ~90 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no external assets, no APIs.
- **localStorage persistence** — every input write triggers a save. The dashboard survives reloads without an account.
- **Live metric recalculation** — every stat, chart, and callout re-derives from the underlying data object on every input change. Never a stale number on screen.
- **SVG pie chart** for asset allocation with computed slice geometry and inline value labels.
- **SVG bar chart** for cash flow with 11 categorized bars, each with its own color.
- **Dark mode default + light mode toggle** via CSS custom properties on `<body>`.
- **Print-ready CSS** — the header, tabs, and buttons hide in print view. The dashboard becomes a legitimate printable financial report.
- **`prefers-reduced-motion`** respected.
- **Reset button** in the header restores the default salaried-professional data for demo purposes.

---

## What this exercise demonstrates

### 1. A financial dashboard is a state-of-your-life dashboard

Personal finance software (Mint, YNAB, Monarch) exists because seeing your numbers in one place changes behavior more than any advice article. The health score is a single number that summarizes "how am I doing," which is the question most people can't answer without one.

### 2. The 50/30/20 rule is a communication device, not a math rule

The rule is mathematically arbitrary but pedagogically brilliant. It gives non-experts three clean buckets to think in. This dashboard visualizes your actual ratios against the target and calls out overshoots — that visualization is more useful than the underlying dollar amounts.

### 3. Compound growth math is worth showing, not stating

Telling someone "extra $200/month becomes $250K in 30 years" is abstract. Showing a slider that lets them move from $0 to $500 and see the number change from $0 to $600K in real time is visceral. The what-if simulator is the module people play with longest.

### 4. Recommendations must name a dollar amount

Generic advice ("save more, spend less") is invisible. Specific advice ("your credit card at 22% APR is costing $58/month in interest — put every extra dollar here first") is actionable. Every insight in the Insights tab names a dollar figure.

---

## Narrative continuity

- **Day 36** — cognitive pattern explorer
- **Day 37** — task compass
- **Day 38** — typing speed studio
- **Day 39** — PDF splitter & merger
- **Day 40** — AI assistant builder
- **Day 41** — interactive learning studio (SQL window functions)
- **Day 42 — personal financial command center**

Meta-skill arc pauses for a genuinely useful daily tool. Day 41 was educational, Day 42 is operational — an app you could actually run every month to manage your finances. 18 more days to go.

---

## Tags

`#60DayClaudeChallenge` `#PersonalFinance` `#WebApp` `#Privacy`
