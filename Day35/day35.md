# Day 35 — Prompt Puzzle

**Project:** `prompt-puzzle.html` — an interactive game that teaches AI prompting through play
**Challenge:** #60DayClaudeChallenge — Day 35
**Domain selected:** Product & Business Strategy
**Difficulty:** Intermediate
**Type:** Single-file vanilla HTML/CSS/JavaScript application. No frameworks, no CDN, no images, no APIs.

---

## What I built

A three-challenge prompting game that teaches you to write prompts that actually produce useful output — instead of vague ones. The domain is Product & Business Strategy at intermediate difficulty. Six real scenarios (competitive analysis, PRD, market sizing, positioning, GTM plan, executive summary) rotate through three challenge types on every playthrough.

The whole thing runs in a single HTML file with a modern puzzle-game UI: glassy dark cards, five color themes, live HUD with score/time/hints, floating toast notifications, drag-and-drop, animated score bars, Prompt DNA visualization, and confetti on high scores.

---

## The two-question intro (per the task spec)

Before generating any of the app content, I asked the user exactly two questions:

1. **Which domain would you like to practice prompting for?** — with major domain options (Data Analytics, Marketing, Product & Business Strategy, Software Engineering).
2. **Choose your difficulty.** — Beginner, Intermediate, Advanced.

Only after both answers are received does the app generate. The user chose **Product & Business Strategy · Intermediate**.

---

## The three challenges

### Challenge 1 · Build the Prompt

Six blocks appear in a tray. Five are correct (Role, Context, Task, Constraints, Format) — three are distractors ("Please be creative," "Add emojis," "Make it sound impressive"). The user drags the right blocks into the prompt slot on the right, leaving the distractors behind.

Scoring rewards accuracy (percentage of correct placements), penalizes wrong placements (dragging distractors into the slot), and awards a +15 Optimization Bonus for a perfect zero-error solve. Hints highlight one correct block at a time — but cost a small hint penalty at the end.

### Challenge 2 · Clean the Prompt

A "messy" prompt appears with ~10 chunks — some are useful, some are noise ("Please try your very best on this," "Really really think hard about it," etc.). The user clicks every chunk they want to remove. The scorer checks correct removes vs. wrong removes vs. missed noise, colors the result inline, and shows a per-chunk breakdown.

This challenge teaches editorial judgment about prompts — the skill of looking at a prompt and cutting the parts that don't earn their space.

### Challenge 3 · Choose the Best Prompt

Three prompts appear for the same task. One is **Weak** (too vague — "tell me about notion vs competitors"). One is **Optimized** (structured role + context + constraints + format). One is **Over-Engineered** (piled with 20+ constraints, meta-instructions, and framework name-drops).

The user picks. The reveal shows why the optimized wins, what would go wrong with each of the other two, and shows both the weak model output and the optimized model output side by side. This is the challenge that most directly teaches the "more constraints ≠ better prompt" lesson.

---

## The scenarios (all Product & Business Strategy)

Six scenarios ship in the library. Every scenario has all nine required fields per the spec: desired output, correct blocks, distractor blocks, weak prompt, optimized prompt, over-engineered prompt, weak AI output, optimized AI output, and a prompt principle.

| # | Scenario | Principle taught |
|---|---|---|
| 1 | Competitive analysis of Notion in project management | Constraints beat vague requests |
| 2 | PRD for a new SSO feature | Explicit sections force structure |
| 3 | Market sizing for an AI analytics tool | Ask for reasoning, not just answers |
| 4 | Positioning statement using For/Who/Unlike | A great template beats creativity |
| 5 | 90-day GTM plan for an API product | Weekly milestones turn wishes into specs |
| 6 | Executive summary of a strategy memo | Front-load the decision for exec audiences |

Every playthrough randomly selects 3 of the 6, one per challenge, so no two runs are identical.

---

## Live scoring

Six live scoring dimensions per the spec:

- **Accuracy** — percentage of correct block placements or choices per challenge.
- **Time** — session timer running from Challenge 1 start. Bonus for finishing under 3 minutes.
- **Moves** — total drag events and clicks. Tracked for post-session analysis.
- **Wrong Placements** — every distractor moved to the slot, every useful chunk removed.
- **Hints Used** — every hint costs a small penalty (3 per hint at the end).
- **Optimization Bonus** — +15 for a perfect zero-error solve within a challenge.

Composite Prompt Score is the average of the three challenge scores plus time bonus minus hint penalty, capped at 100.

---

## Prompt Performance Report

The final report includes everything the spec required plus a few extra micro-interactions:

- **Prompt Score** — big number, animated count-up from 0 to final.
- **Rating** — five bands: Elite (92+), Sharp (80+), Solid (65+), Developing (45+), Just Starting.
- **Rank** — five tiers: S+, A, B, C, D.
- **Prompt DNA visualization** — four vertical strands showing Clarity, Specificity, Constraints, Format. Each height computed from a weighted blend of the three challenge scores.
- **Challenge Breakdown** — animated bar chart showing each challenge score.
- **Session Metrics** — time, moves, wrong placements, hints used.
- **Personalized Feedback** — computed from the user's DNA — names the strongest and weakest dimension and gives concrete next-step advice based on both.
- **Next Milestone** — different at each score band.
- **Final Optimized Prompt** — the fully-formed optimized prompt from Challenge 3 so the user can copy it and reuse.

---

## Tech notes

- **Single HTML file.** ~55 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no APIs.
- **Five color themes** — Claude Orange (default), Electric, Neon, Rose, Gold. Theme swap is a `data-theme` attribute flip; every color resolves via CSS custom properties.
- **Live HUD** — score, time, hints — always visible in the header.
- **Drag-and-drop** via HTML5 DnD for Challenge 1. Every block is `draggable="true"` with `dragstart`/`dragend`/`drop` handlers.
- **Click-to-remove** for Challenge 2. Each phrase is a `<span class="chunk">` with a toggle handler.
- **Multiple-choice** for Challenge 3 with animated result reveal.
- **Toast notifications** — floating messages after each challenge score.
- **Confetti** on Prompt Score ≥ 80 — pure CSS keyframe animation, no library.
- **Score animation** — count-up from 0 to final score on report render.
- **DNA bars animate** from 0 to their computed heights on report render.
- **Randomized scenarios** — every "Replay" picks 3 fresh scenarios from the pool.
- **Reusable JS objects** — every scenario is a plain object with the same schema so more can be added trivially.

---

## What this exercise demonstrates

### 1. Prompting is a skill that teaches better through play than through tutorials

A tutorial tells you "include role, context, task, constraints, and format." A game makes you physically drag those blocks into place — and drop them wrong the first time, and get penalized, and remember it next time. The tactile encoding is the actual teaching mechanism.

### 2. Optimized ≠ maximized

The most valuable single lesson in the whole game is the difference between the Optimized prompt and the Over-Engineered prompt. Beginners assume more constraints = better prompt. The over-engineered version has more constraints but produces worse output because every extra constraint dilutes the ones that actually matter. Recognizing that takes practice — this is what Challenge 3 trains.

### 3. Prompt DNA is a better feedback surface than a raw score

Reporting "you scored 78" tells the user nothing about what to work on. Reporting "Clarity 88, Specificity 74, Constraints 61, Format 82" tells them precisely which dimension to invest in on their next prompt. The DNA visualization is the report's most useful output.

### 4. Same architecture, different domain

The teach-before-decide → observe → commit → reveal spine used in Days 32 (marketing strategy), 33 (media literacy), and 34 (marketing detective) generalizes to prompting. The lesson keeps repeating: educational simulators generalize across domains as long as the teaching architecture stays stable and the content is well-chosen.

---

## Narrative continuity

- **Day 32** — marketing strategist simulator
- **Day 33** — media literacy analyzer
- **Day 34** — marketing detective
- **Day 35 — prompt puzzle**

Marketing arc pauses. Meta-skill arc begins. The next few days will focus on the skills that underlie every other day of this challenge — prompting, critical reading, structured thinking.

---

## Tags

`#60DayClaudeChallenge` `#PromptEngineering` `#AI`
