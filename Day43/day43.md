# Day 43 — AI Workflow Architect · Data Analysis (Question to Dashboard)

**Project:** `ai-workflow-architect.html` — a complete end-to-end workflow from stakeholder question to delivered dashboard
**Challenge:** #60DayClaudeChallenge — Day 43
**Workflow selected:** Data Analysis · Question to Dashboard
**Structure:** Claude auto-designed the full 7-stage workflow
**Type:** Single-file vanilla HTML/CSS/JavaScript. No frameworks, no CDN, no images, no APIs. Fully offline. localStorage persistence.

---

## What I built

A working analyst's end-to-end workflow, tuned to how modern analysts actually work with AI in the loop. Seven progressive stages from a fuzzy stakeholder question to a delivered dashboard or memo. Each stage ships with: objective, tasks, best AI tools (with real names, tiers, and alternatives), copy-paste-ready prompt examples, best practices, common mistakes, expected outputs, time estimates, and efficiency tips.

Not a checklist. Not a diagram. A complete mapped workflow — the kind an analyst could actually work from during a real project.

Progress, notes, and bookmarks persist per stage in `localStorage`. Print-ready CSS turns the whole thing into a handout you could give a stakeholder or a new hire.

---

## The two-question intro (per task spec)

Before generating anything, I asked two MCQ-format questions:

1. **Which end-to-end workflow should the Workflow Architect map?** — options included Job Application, Data Analysis, LinkedIn Personal Brand Growth, and Product Marketing Campaign. User chose **Data Analysis (Question to Dashboard)**.
2. **Should Claude auto-structure the workflow, or customize sections?** — user chose **auto-structure**.

---

## The seven stages

Each stage is a fully-authored module with the full section list from the task spec.

**01 · Clarify the Question — from vague ask to precise hypothesis.** The single most-skipped stage and the one that determines the whole analysis's success. Includes prompts for reframing stakeholder asks into candidate hypotheses.

**02 · Design the Analysis — methodology before queries.** Plan the entire analysis on paper before writing SQL. Data sources, methodology, join keys, metric definitions, expected output shape. Prompts for pressure-testing the plan.

**03 · Extract & Clean Data — the most time-consuming stage.** SQL, row-count validation, null handling, outlier decisions. The stage where projects most commonly go silently wrong. Includes SQL debugging prompts and data cleaning workflows.

**04 · Analyze & Model — where the answer lives.** Segmentation, statistical testing, quantifying uncertainty. Prompts for picking the right statistical test and sanity-checking a regression.

**05 · Synthesize Insights — from numbers to a narrative.** The stage that separates good analysts from great ones. Prompts for sharpening findings, ranking by impact, and stress-testing counter-arguments.

**06 · Build the Deliverable — dashboard, memo, or deck.** Pick the format that matches how the answer will be consumed. Prompts for designing dashboards and structuring executive memos.

**07 · Present & Iterate — the analysis doesn't end with the deck.** Rehearsing hard questions, capturing follow-ups, writing the summary email. Prompts for rehearsing tough questions and post-mortems.

Every stage has a time estimate — the whole workflow ranges from 1 to 2 weeks of analyst effort for a non-trivial question.

---

## The AI stack

Every stage lists real AI tools with tier (Free/Paid), the specific reason it's recommended, and alternatives. The tools list spans:

- **Thinking partner** — Claude (primary), ChatGPT, Gemini, Perplexity
- **Notebook + code** — Cursor, Hex, Deepnote, Claude Code, GitHub Copilot
- **Analysis** — Julius AI, Chat2CSV, Python + pandas + statsmodels, scikit-learn, DoWhy/EconML
- **Data quality** — Great Expectations, dbt tests, DuckDB for local exploration
- **Dashboards** — Tableau + Power BI Copilot, Metabase, Superset, Gamma, Tome, Claude Artifacts
- **Presentation prep** — Loom, Slack Canvas, Fathom/Otter

Total 28+ tools referenced. Every recommendation has a why and an alternative.

---

## Prompt library

18 copy-paste-ready prompts spread across the seven stages, each engineered for the specific step of the workflow:

- **Clarify:** reframe a vague ask, pressure-test scope
- **Design:** pressure-test the plan, pick a methodology
- **Extract:** write and explain SQL, debug a wrong result, clean nulls and outliers
- **Analyze:** interpret a distribution, pick a statistical test, sanity-check a regression
- **Synthesize:** sharpen a finding, find the counter-argument, rank by impact
- **Build:** design a dashboard, structure a memo, redesign a busy dashboard
- **Present:** rehearse hard questions, structure a follow-up email, post-mortem the delivery

Plus 3 general-purpose prompts in the resources section for ongoing practice (analysis critique, data quality check, metric definition).

Every prompt has a title and is copy-clickable.

---

## Interactive elements

- **Stage strip at the top** — 7 clickable chips showing every stage, with visual states for active / complete / not-started. Click any chip to jump to that stage; completed stages get a green checkmark.
- **Progress bar + stage counter** in the header — updates live as stages are marked complete.
- **Bookmark button per stage** — bookmarked stages count in the header HUD.
- **Notes per stage** — 80px+ textarea with auto-save (400ms debounce). "✓ Saved" indicator on every keystroke.
- **Prompt copy buttons** — every prompt has a one-click copy button.
- **Reset button** — clears all progress, notes, and bookmarks with confirmation.
- **Dark mode default + light mode toggle** — CSS custom properties on `<body>`.
- **Print-ready CSS** — header, tabs, and buttons hide in print view. Turns the workflow into a hand-off document.

---

## The Resources section

Ships with all the sections the task spec required:

- **Workflow Summary** — every stage's objective and time estimate on one page as a scannable overview.
- **Recommended AI Stack** — organized by function (Thinking Partner, Code + SQL, Analysis, Deliverables).
- **Learning Resources** — books (Storytelling with Data, The Art of Statistics), documentation (Mode's SQL tutorial, Hex Templates, dbt Learn).
- **Communities** — Slack groups (Locally Optimistic), newsletters (benn.substack.com, Cassie Kozyrkov), Reddit threads.
- **Search Keywords** — specific phrases to Google when stuck.
- **Additional AI Prompts** — three general-purpose prompts for ongoing practice.
- **Future Automation Opportunities** — where to invest in tooling to move up the value chain (dbt metric layer, self-serve exploration, anomaly detection).

Every one is real and useful — no placeholder resources.

---

## Tech notes

- **Single HTML file.** ~90 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no external assets, no APIs.
- **localStorage persistence** — every stage completion, note, and bookmark saves immediately. Reload restores state.
- **State machine** — single `goto(next)` navigator; every screen renders from an isolated function.
- **Auto-save on notes** with 400ms debounce and visible save confirmation.
- **`prefers-reduced-motion`** respected.
- **Print styles** — the workflow prints as a legitimate document without the header/nav chrome.
- **Dark mode default + light mode toggle** via CSS custom properties on `<body>`.

---

## What this exercise demonstrates

### 1. A workflow is not a checklist

Most "guides" for a workflow are a checklist of tasks. This is a mapped process — each stage has a specific input, output, tool stack, and failure modes. The difference is that a user could actually work from this during a real project.

### 2. Naming failure modes is where teaching happens

Every stage has a "Common Mistakes" section. The mistake most likely to sink Stage 3 (silent LEFT JOIN row explosions) is different from the mistake most likely to sink Stage 5 (burying the answer in slide 12). Naming them explicitly is more useful than any positive advice.

### 3. AI tools change the workflow shape, not the workflow steps

The stages don't disappear when you use AI — they just get faster. Clarifying the question, choosing a methodology, and defending the finding are still the analyst's job. AI is the co-pilot, not the pilot. The tool recommendations in each stage reflect this: AI accelerates the mechanical work; the judgment work is still yours.

### 4. The archive is the artifact

An analyst can work through the workflow, take notes at each stage, mark stages complete, and end up with a fully documented project record — kept in the browser's localStorage without an account. That archive is more valuable than any single deliverable.

---

## Narrative continuity

- **Day 37** — task compass
- **Day 40** — AI assistant builder
- **Day 41** — interactive learning studio (SQL window functions)
- **Day 42** — personal financial command center
- **Day 43 — AI workflow architect (data analysis)**

Meta-arc continues. Day 41 taught the SQL window function skill. Day 43 puts that skill in the context of the whole analyst workflow. Day 42 was operational; Day 43 is procedural. Together they give an analyst a real toolkit for the daily job.

---

## Tags

`#60DayClaudeChallenge` `#DataAnalytics` `#AIWorkflow` `#Productivity`
