# Day 49 — Personal AI Playbook

**Project:** `personal-ai-playbook.html` — a workshop for building reusable prompt systems, with a block-based Prompt Builder and a Loop Builder that wraps any prompt into a self-improving loop
**Challenge:** #60DayClaudeChallenge — Day 49
**Persona:** Blended (analyst + student + job seeker + builder)
**Type:** Single-file HTML/CSS/JavaScript. No frameworks, no CDN. All state in localStorage.

---

## What I built

Most "AI prompt tools" are storage systems — a bigger folder for your best prompts. This one is a workshop. The unit of value isn't a prompt; it's a labelled block (Role, Objective, Context, Constraints, Reasoning strategy, Output format, Tone, Examples, Quality checks). Nine blocks combine into thousands of prompt variations. A separate Loop Builder wraps any prompt into an autonomous self-improving loop with a goal, evaluator, stop conditions, and safety rules.

Twelve real workflows seed the Dashboard so the empty state teaches by example — resume bullet tailorer, single-file HTML app spec drafter, decision framework runner, LinkedIn Day-N post drafter, and a self-improving loop variant of the bullet tailorer among them.

---

## The four-question intro

Per the spec, I asked four MCQs before generating:

1. **Persona focus** → Blended (analyst + student + job seeker + builder)
2. **Workflow categories** → All four (Building, Analysis, Job search, Content)
3. **Depth level** → "See according to you" — chose power-user default with beginner onboarding
4. **Seed workflows** → Yes, 8-12 real workflows from actual daily use

---

## The architectural decision: blocks, not prompts

The spec explicitly asked for "reusable AI workflow systems that match the user's needs" and "modular building blocks that can be combined into thousands of prompt variations." The choice was between two shapes:

**Shape A — a prompt library.** Every workflow is a stored string. To make a new one, you copy and edit. Storage grows linearly with variety. Reusability lives in copy-paste discipline.

**Shape B — a block system.** Every workflow is an ordered list of block instances. Each block has a type (Role, Objective, etc.), a preset dropdown, and free-text override. To make a new one, you assemble. Storage grows linearly with unique blocks, not variety.

Shape B wins because it teaches. Every block carries a short in-UI explanation shown in two places — inside the picker before you add it, and inside the assembled block after you add it. You never have to guess what "Reasoning strategy" means or why it's there. The tool becomes a prompt-engineering tutor as a side effect of being a prompt-engineering tool.

---

## The nine building blocks

Each block has a visible explanation, a dropdown of hand-written presets, and a free-text override.

- **👤 Role** — Who the model should act as. Anchors expertise level and vocabulary.
- **🎯 Objective** — What you want, in one sentence. Prompts fail more often from unclear objectives than from anything else.
- **📋 Context** — What you know that the model doesn't. Audience, current state, real names.
- **🚧 Constraints** — Hard rules the output must obey. Negative instructions are underrated.
- **🧠 Reasoning strategy** — How the model should think before answering. Step-by-step, first-principles, working-backwards, considering alternatives.
- **📐 Output format** — The exact shape of the response. Eliminates a class of "asked for X, got Y" failures.
- **🎨 Tone** — The voice. Easier to specify with a Style reference than with adjectives.
- **💡 Examples** — Few-shot examples of what "good" looks like. One vivid example often beats a paragraph of rule-writing.
- **✅ Quality checks** — Self-checks before the model finishes. Catches vague claims, invented facts, unnecessary caveats.

Assembled blocks render as a live prompt preview on the right, with block count / character count / estimated token count. One-click copy to clipboard.

---

## The Loop Builder

Any prompt can become a self-improving loop. Paste a base prompt at the top. Add five loop blocks:

- **🎯 Loop goal** — What the loop optimizes for. Must be measurable.
- **📊 Evaluation criteria** — How each iteration is scored. Model self-scores 1-100 against a rubric.
- **🔧 Improvement strategy** — How to revise between iterations. Different from Reasoning strategy — this is about improving on the previous attempt.
- **🛑 Stop conditions** — When to end the loop. Must include a hard iteration cap. Best practice: use threshold + plateau + cap simultaneously.
- **🛡️ Safety rules** — Hard aborts regardless of score. Catches runaway loops and drift from the original task.

The Loop Builder renders a complete wrapper prompt — base prompt + loop configuration + protocol — that instructs the model to iterate itself. The output is a single string you paste into any chat. The model runs the loop; you paste the winner.

This design carries over the lesson from Day 46 (autonomous agent studio with real stop conditions) and Day 47 (markdown responses beat JSON to avoid parse errors) — the loop wrapper explicitly asks the model to emit `SCORE: N/100` and `FINAL: [output]` markers as plain markdown, no JSON to break on.

---

## The twelve seeded workflows

**Building & prompt engineering:**
1. Single-file HTML app spec drafter
2. Prompt roast + rewrite

**Analysis & decisions:**
3. SQL query explainer + optimizer
4. Decision framework runner (Reversibility × Impact × Time × Confidence)

**Job search & career:**
5. Resume bullet tailorer (3 variants matched to a target JD)
6. Cover letter drafter (tight 3-paragraph)
7. Interview STAR-story builder
8. LinkedIn networking DM (3-line format)
9. Resume bullet — self-improving loop (LOOP)

**Content & thought leadership:**
10. LinkedIn Day-N build post drafter
11. Build-in-public writeup
12. Product teardown analysis

Each seed uses 4-7 blocks. Opening any seed loads it into the Prompt Builder (or Loop Builder) so you can inspect exactly how it's assembled, then duplicate and modify freely.

---

## First-time viewer discoverability

Per the spec's requirement that a cold viewer understand the tool within seconds:

- **Persistent explainer banner** on the Dashboard by default. States what the tool does and describes the three main views (Dashboard, Prompt Builder, Loop Builder). Dismissible via user action only.
- **Permanent "? What is this?" button** in the top bar re-opens the full onboarding at any time.
- **Four-step onboarding** on first load — explains the block philosophy, walks through the Prompt Builder, walks through the Loop Builder, and points at the seeded workflows as tutorial-by-example.
- **Every block carries its explanation** both in the picker (before add) and inside the assembled block (after add). No guessing what "Reasoning strategy" is or why it matters.
- **Self-descriptive nav labels** — "Dashboard", "Prompt Builder", "Loop Builder", not invented jargon like "Deck" or "Hub".
- **Section subtitles describe purpose** — "Your saved workflows across four categories" not "Your AI operating system, at a glance".

---

## Tech notes

- **Single HTML file.** ~84 KB. Vanilla CSS + JavaScript. No frameworks, no CDN.
- **localStorage persistence** — workflows, favorites, theme choice, onboarding state, explainer dismissal.
- **CRUD** on workflows — create, edit, duplicate, delete, favorite. Search by name / description / block content. Filter by category via chips + sidebar.
- **Import / export JSON** — full backup / restore. Reset to seeds also available.
- **Dark mode default + light mode toggle** via CSS custom properties.
- **Keyboard shortcuts** — ⌘K / Ctrl+K to focus search, ⌘N / Ctrl+N to create new workflow, `?` to open onboarding, Escape to close any modal.
- **Responsive** — sidebar collapses on narrow screens, builder columns stack.
- **`prefers-reduced-motion`** respected.
- **Toast notifications** for save / copy / delete confirmations.

---

## What this exercise demonstrates

### 1. Teach the model of the prompt, not the prompts themselves

If someone leaves this tool remembering only "prompts have these nine parts", that's more valuable than any specific prompt they took away. Labelling the blocks turns every use into a micro-lesson in prompt engineering.

### 2. Reusability compounds when the unit is small

A prompt library of 200 prompts is 200 prompts. A block library of 9 blocks with 6-8 presets each is thousands of combinations. The right level of abstraction moves the growth curve from linear to combinatorial.

### 3. First-run friction is a UX problem, not a documentation problem

The persistent explainer, the "? What is this?" button, and the block-level in-context explanations solve the same problem three ways — because the spec explicitly named "someone opening this file cold, possibly just a screenshot" as the target. That constraint shaped the entire interface.

### 4. Loops are prompts too

The Loop Builder isn't a runtime — it doesn't execute the loop. It produces a single loop-wrapper prompt you paste into any chat. The model becomes the loop runtime. This is the cheapest possible way to ship self-improving behavior — no backend, no orchestration, no API keys required.

---

## Narrative continuity

- **Day 40** — AI Assistant Builder (single-agent per assistant)
- **Day 46** — Autonomous Agent Studio (sequential 6-agent loop with real stop conditions)
- **Day 47** — Content Intelligence Studio (parallel 6-reviewer multimodal audit)
- **Day 48** — Compare & Decide Builder (weighted-criteria decision tool)
- **Day 49 — Personal AI Playbook (block-based prompt system + loop wrapper)**

The multi-agent arc ended at Day 47. Days 48-49 pivot to systems for the user's own daily AI work — Day 48 was "how to decide between models", Day 49 is "how to structure the prompts you send them". Both replace the storage metaphor (a stash of prompts / a fixed ranking) with a mechanic that generates fresh answers as inputs change.

---

## Tags

`#60DayClaudeChallenge` `#PromptEngineering` `#AITools` `#BuildInPublic`
