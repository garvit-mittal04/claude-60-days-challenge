# Day 41 — Interactive Learning Studio · SQL Window Functions

**Project:** `interactive-learning-studio.html` — a full-length interactive tutorial that teaches one specific topic end-to-end
**Challenge:** #60DayClaudeChallenge — Day 41
**Topic selected:** SQL Window Functions
**Structure:** Claude decided — all sections auto-structured
**Type:** Single-file vanilla HTML/CSS/JavaScript. No frameworks, no CDN, no images, no APIs. Works fully offline.

---

## What I built

A complete interactive tutorial on SQL window functions — the single most powerful SQL feature that separates analysts from beginners. The tutorial teaches the topic *completely* rather than presenting a summary. Four progressive modules, four quizzes, a final practical challenge, a cheat sheet, and a curated resources section, all in one HTML file (~85 KB).

The whole thing is built around one design principle: teach a real topic well enough that a beginner leaves able to write ranking queries, moving averages, and running totals in production.

---

## The two-question intro (per task spec)

Before generating anything, I asked the user two MCQ-format questions:

1. **What topic should the Interactive Learning Studio teach?** — with four candidate topics (SQL Window Functions, A/B Testing, Python OOP, Marketing Positioning). User chose **SQL Window Functions**.
2. **Should Claude auto-structure the tutorial or would you like to customize sections?** — user chose auto-structure.

The topic locked in as SQL Window Functions with the full section list from the spec.

---

## The four progressive modules

**Module 1 · The Basics — Why window functions exist.** Introduces the OVER clause. Explains the fundamental difference between GROUP BY (collapses rows) and window functions (preserve rows and add columns). Includes a "spreadsheet with a helper column" analogy, a side-by-side GROUP BY vs. window comparison, and an interactive "predict the output" exercise on a single-row partition case.

**Module 2 · Ranking Functions — ROW_NUMBER, RANK, DENSE_RANK.** The three ranking flavors with the exact tie-handling difference. The classic "top N per group" pattern is taught in full — including why `LIMIT 2` doesn't work and why filtering `WHERE rn <= 2` requires a CTE. Includes the "salary tie" example that makes RANK vs. DENSE_RANK visually clear.

**Module 3 · LEAD, LAG, and Value Windows.** Comparing rows to their neighbors. LAG(1) for previous row, LEAD(1) for next row, FIRST_VALUE for the partition's opening row. Includes the DAU/MoM growth-rate query — the single most common LAG use case in analyst work. Handles the "first LAG is always NULL" gotcha explicitly.

**Module 4 · Frames and Running Calculations — the deepest module.** Frame clauses (`ROWS BETWEEN … AND …`), running totals (`UNBOUNDED PRECEDING AND CURRENT ROW`), moving averages (`N PRECEDING AND CURRENT ROW`), and the ROWS vs. RANGE distinction. This module also explains the famous LAST_VALUE gotcha that most tutorials skip — that the default frame stops at CURRENT ROW, so LAST_VALUE without an explicit frame returns the current row's value.

Every module has: detailed explanations, real SQL code blocks with syntax highlighting, worked examples on realistic tables (employees, orders, daily_sales), a hand-drawn SVG diagram, a common misconceptions section, a "try it yourself first" practical exercise with hidden solution, and a Key Takeaways summary.

---

## The four quizzes

Each module unlocks a 4-question quiz. Questions are engineered to reveal specific misconceptions rather than test memorization:

- **Q1 asks about empty OVER()** — a classic "wait, that's valid?" moment.
- **Q2's ranking question** uses the exact salary-tie example from the module — testing whether the user internalized RANK vs. DENSE_RANK.
- **Q3 asks about NULL** — every LAG/LEAD tutorial glosses over the boundary NULLs.
- **Q4 tests off-by-one on moving averages** — "7-day = 6 preceding, not 7."

Every question has a full paragraph explanation on submit. Correct answers earn +10 XP each. A perfect 4/4 earns a +50 XP bonus and triggers a confetti burst.

The module UNLOCKS the next module only after the quiz is submitted — encouraging users to actually work through the exercises before moving on.

---

## The final practical challenge

One realistic analyst task that requires combining ranking, LAG, and a frame clause in a single query:

> For each customer, return the date and amount of their SECOND order, the days since their first order, and their cumulative spend up to and including that second order. Customers with fewer than 2 orders should be excluded.

The reveal shows the full CTE-based query, a step-by-step reasoning trace, and the expected output. Solving this well means the user understands all four modules together — it's the graduation exam.

---

## The final resources & continue-learning section

Per the spec, the tutorial ends with a full learning-continuation panel:

- **Cheat Sheet** — 10 window function patterns as copy-paste code blocks
- **Summary Notes** — the "four things to remember" digest
- **Books** — SQL Cookbook, Ace the Data Science Interview, SQL Antipatterns
- **Documentation** — Postgres, MySQL 8, BigQuery window docs
- **Practice Platforms** — DataLemur, StrataScratch, LeetCode Database, SQLZoo
- **Communities** — r/SQL, Stack Overflow, Modern Data Stack Slack
- **Research papers** — SQL:2003 spec, engine implementation papers
- **Search keywords** — the exact phrases to google when stuck
- **AI prompts for further learning** — 5 copy-paste prompts to keep drilling with Claude

---

## Interactive elements

- **Syntax-highlighted SQL code blocks** with click-to-copy buttons. Custom vanilla JS highlighter — no external library.
- **Live data tables** with color-coded partition rows (each department a different tint) so the reader visually sees what "partition" means.
- **Reveal-on-click solution panels** using native `<details>` elements — every practical exercise lets the user try first, reveal the answer second.
- **Three hand-drawn SVG diagrams** — the "row + window function = new column" analogy, the "LEAD/LAG paging" diagram, and the "moving average spotlight" diagram.
- **Progress bar + tab navigation** — completed modules get a green checkmark, locked modules are dimmed and click-blocked.
- **XP counter + badge counter** in the header — every quiz answer adds to the total, every module completion adds a badge.
- **Confetti animation** on perfect quiz scores and on final challenge completion.
- **Dark mode default + light mode toggle** with CSS custom properties.
- **Print-ready CSS** — the header, tabs, and buttons hide in print view so users can print the tutorial as a summary handout.

---

## Tech notes

- **Single HTML file.** ~85 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no external assets, no APIs.
- **Custom SQL syntax highlighter** — 30-line regex-based highlighter for keywords, functions, strings, numbers, comments. No highlight.js dependency.
- **State machine** with locked/unlocked module tracking. Quiz completion is the gate that unlocks the next module.
- **All content is topic-specific.** Every example, analogy, exercise, and quiz question was generated specifically for SQL window functions — nothing is a placeholder.
- **Reward system.** XP counter (10 per correct answer, 50 bonus for perfect quiz, 100 for final challenge) and 4 badges (one per module completion).
- **`prefers-reduced-motion`** respected — all animations disable when the OS setting is on.
- **Print styles** — the tutorial is a legitimate printable reference. Header, tabs, and buttons hide; cards flow into a clean printable document.

---

## What this exercise demonstrates

### 1. Teaching a topic completely is a different problem from summarizing it

The task spec was explicit: "teach the topic completely rather than a learning roadmap." Most AI-generated tutorials give you a table of contents and a two-paragraph summary. This one takes 45 minutes to work through — and by the end the user can actually write a running total or a rank-per-group query in production. That difference is the whole point.

### 2. Progressive gating is what turns "content" into "learning"

Locking Module 2 behind Quiz 1 forces the user to answer questions before moving on. Without that gate, users skim to the cheat sheet. With it, they engage with the content. The gating is a design choice that shows up as one line of state, but it's what makes the tutorial actually teach.

### 3. Every misconception deserves its own callout

Most SQL tutorials mention that LAG(1) returns NULL on the first row and move on. This tutorial dedicates a full misconception callout to it — because that's exactly the bug that shows up in production a week later. Naming failure modes explicitly is what separates a tutorial from documentation.

### 4. The final challenge is where the tutorial earns its title

The last practical challenge combines ranking, LAG, FIRST_VALUE, and a running-total frame in one query. A user who can solve it can defensibly claim to know window functions. Everything before it was preparation for that one query.

---

## Narrative continuity

- **Day 35** — prompt puzzle
- **Day 36** — cognitive pattern explorer
- **Day 37** — task compass
- **Day 38** — typing speed studio
- **Day 39** — PDF splitter & merger
- **Day 40** — AI assistant builder
- **Day 41 — interactive learning studio (SQL window functions)**

Meta-skill arc continues. Day 40 taught how to design assistants; Day 41 turns the same "teach → observe → commit → reveal" pedagogy toward SQL — the actual language every analyst job requires. Fits the MSBA / analyst track directly.

---

## Tags

`#60DayClaudeChallenge` `#SQL` `#DataAnalytics` `#WindowFunctions`
