# Day 36 — Cognitive Pattern Explorer

**Project:** `cognitive-pattern-explorer.html` — a psychology-inspired self-reflection experience
**Challenge:** #60DayClaudeChallenge — Day 36
**Type:** Single-file vanilla HTML/CSS/JavaScript. No frameworks, no CDN, no images, no APIs. Works fully offline.

---

## What I built

An interactive self-reflection experience that surfaces the tendencies people naturally reach for when they think, decide, and prioritize — without ever diagnosing anything. Three chapters of scenarios, priority-card ranking, and a decision timeline. A reflection journal at the end names your leading thinking style, shows the full percentage breakdown across five tendencies, and gives one gentle experiment to try next time.

The whole experience uses reflective language ("you often…", "this suggests…") — never clinical labels. There are no right answers. The user chooses between **Calm Mode** (everyday framing, no time pressure) and **Stress Mode** (urgency, tighter framing) — and can replay in the opposite mode to see how their tendencies shift under pressure.

---

## The five thinking tendencies

Every choice, priority, and timeline step maps to weighted contributions across five tendencies:

- **Analytical Thinker** — reaches for structure, frameworks, step-by-step reasoning.
- **Emotional Intuitive** — reads the emotional signal in a situation first.
- **Overthinking Loop Style** — turns decisions over from many angles.
- **Action-First Decision Maker** — favors motion over deliberation.
- **Balanced Reflective Thinker** — moves comfortably between structure and feeling.

None are ranked "better." The reflection just names which one the session most surfaced.

---

## The five-stage flow

### 1. Start Screen — Calm Mode or Stress Mode

Two large mode cards. Calm frames scenarios as everyday moments ("You have an open Saturday…"). Stress frames the same scenarios under time and social pressure ("It's Friday 6pm and three people want plans"). Same underlying tendencies, different context — so the user can compare their own patterns across modes.

### 2. Chapter 1 · Discover Your Thinking Style

Five everyday scenarios with 4–5 options each. Every option is weighted toward one or two tendencies. Choosing "Open my notes app and map out the day" weights Analytical + Balanced. Choosing "Text one person and see what they feel like" weights Emotional + Balanced. Users don't see the weights — they just pick what feels most true.

### 3. Chapter 2 · Choose Your Priorities

Eight priority cards (Speed, Clarity, Empathy, Certainty, Impact, Balance, Growth, Efficiency) — drag your top 5 into rank order. Position matters: #1 counts most, #5 counts least. Ranked list weights different tendencies. Someone who ranks Speed → Impact → Growth → Balance → Empathy trends action-forward. Someone who ranks Clarity → Certainty → Balance → Efficiency → Impact trends analytical.

### 4. Chapter 3 · Map Your Thinking

Six thinking steps (Feel through it, List pros and cons, Do research, Ask trusted people, Try a small version, Question the framing). Drag them into the order you actually take when facing a big decision. The first three positions carry the most weight — because that first move says the most.

### 5. Reflection Journal

- **Hero** — your leading tendency in this session, with a reflective sentence about it.
- **Tendency Breakdown** — animated bar chart of all five tendencies with percentages that always sum to 100.
- **What Suggested This** — an explanation, personalized to your leading and secondary tendencies, of what your specific answers pointed at.
- **A Gentle Experiment for Next Time** — one small practice tuned to your leading tendency (e.g., analytical: "name the feeling before you open the spreadsheet").
- **Compared to Your Previous Session** — if the user replays in the opposite mode, this tile compares the two runs and calls out whether the leading tendency shifted or held.

---

## Calm mode vs. Stress mode

The same scenario appears in two framings. Example — the "big decision" scenario:

**Calm.** *"You're weighing a big decision (a job change, a move, a big buy). What does your first draft of the decision look like?"*

**Stress.** *"You have to give an answer on a big decision by end of day. Your first move is:"*

The options and their tendency weights are identical — only the framing changes. This lets the user notice their own pattern shifts under time pressure without any framing feedback loop distorting the result. The background color also subtly cools when Stress mode is on, so the environment matches the framing.

---

## Design principles

### 1. Reflective language, never diagnostic

Every insight starts with "You often…" or "This suggests…" — never "You are a…" Every result reminds the user this is a snapshot of one session, not a label. The final tile explicitly says *"Your tendencies shift with context, sleep, and stakes. The value here is noticing — nothing more."*

### 2. Calm modern aesthetic

Dark base with soft sage green as the default accent — one of the calmest palettes available. Five color themes: Sage, Lavender, Sky, Peach, Claude Orange. Rounded 14-16px corners. Cormorant Garamond serif for headings (calm, editorial), system sans for body. Ambient background glow that gently drifts.

### 3. Accessibility built-in

- Full keyboard nav — every card is tabbable, Enter/Space acts as click.
- Arrow-key reorder for the timeline in Chapter 3 and the ranking in Chapter 2.
- `prefers-reduced-motion` respected — all animations disable when the user has that OS setting.
- Progress bar has `role="progressbar"`.
- Live region on the stage for screen readers.
- Focus rings visible on all interactive elements.

### 4. Touch-friendly drag-and-drop

Every draggable card can also be **tapped** to move — the click handler on each priority card moves it between pool and rank list. This makes the whole experience work on phones where HTML5 drag-and-drop is unreliable.

---

## Scoring architecture

Weights come from three sources with different multipliers:

- **Chapter 1 scenarios** — each chosen option contributes its raw weight (1–2 points per tendency).
- **Chapter 2 ranking** — each priority card is multiplied by its position (rank 1 = 3×, rank 2 = 2.5×, rank 3 = 2×, rank 4 = 1.5×, rank 5 = 1×).
- **Chapter 3 timeline** — each step is multiplied by its position (position 1 = 3×, position 2 = 2×, position 3 = 1.5×, etc.).

Totals are converted to percentages and rebalanced to sum to exactly 100. Percentages animate from 0 to their target values on the report render.

---

## Tech notes

- **Single HTML file.** ~55 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no external assets, no APIs.
- **Five color themes** — Sage (default), Lavender, Sky, Peach, Claude Orange. All swap via `data-theme` attribute.
- **HTML5 drag-and-drop** for Chapters 2 and 3, plus a click-to-move fallback that works on touch devices and via keyboard.
- **`prefers-reduced-motion` media query** — disables all animations for users with that OS setting.
- **Ambient CSS glow** that drifts on a slow 18s cycle (disabled under reduced motion).
- **State machine** with a single `goto(next)` navigator; every stage renders from an isolated function.
- **Reusable JS objects** for tendencies, scenarios, priorities, and timeline steps — trivial to add more.
- **Session-persistent** compare-across-modes — replaying in opposite mode shows a compared insight.

---

## What this exercise demonstrates

### 1. Reflection is a design pattern, not a diagnostic pattern

Most personality tools try to *classify* users. This one tries to *invite noticing*. Every result is followed by a reminder that tendencies shift with context. That framing choice is what makes the experience feel calm instead of judgmental. Language does most of the work — "you often" is very different from "you are."

### 2. Same content, two framings, teaches self-awareness

The Calm vs. Stress mode split is the single most useful design choice. A user who plays both discovers something they couldn't get from either alone — how their pattern *shifts* under pressure. That comparison is the actual teaching mechanism. Neither mode alone would surface it.

### 3. Weighted scoring across multiple channels is more accurate than a single channel

If the experience only used the multiple-choice scenarios, users would recognize the pattern and game their answers. By combining scenarios + priority ranking + timeline ordering, the composite is harder to game — and more honest as a result. Weighted position on Chapter 2 and 3 also captures preference intensity, not just category.

### 4. Accessibility isn't optional for a reflection tool

A tool that asks users to reflect on their own thinking must respect users who navigate differently. Full keyboard nav, arrow-key reorder, click-to-move drag alternatives, and `prefers-reduced-motion` support aren't polish — they're the difference between a tool that works for everyone and a tool that quietly excludes people.

---

## Narrative continuity

- **Day 32** — marketing strategist simulator
- **Day 33** — media literacy analyzer
- **Day 34** — marketing detective
- **Day 35** — prompt puzzle
- **Day 36 — cognitive pattern explorer**

Meta-skill arc continues. Day 35 taught prompt engineering as a skill. Day 36 turns the same "teach → observe → commit → reveal" pedagogy inward — using it to explore how you naturally think.

---

## Tags

`#60DayClaudeChallenge` `#PsychologyDesign` `#SelfReflection`
