# Day 46 — Autonomous Agent Studio

**Project:** `autonomous-agent-studio.html` — a real multi-agent orchestration pipeline that runs live against the Claude API
**Challenge:** #60DayClaudeChallenge — Day 46
**Workflow selected:** Interview Answer Coach (from a candidate's rough answer to interview-ready)
**Success criteria:** Composite 0-100 quality score across 5 STAR dimensions
**Stopping conditions:** All three per spec — plateau (Δ<3 for 2 rounds) → threshold (≥88) → hard cap (8 rounds)
**Agent lineup:** Full 6-agent auto-design (Planner · Executor · Evaluator · Critic · Improver · Final Reviewer)
**Type:** Single-file HTML/CSS/JavaScript. No frameworks, no CDN. Client-side API calls (bring your own key).

---

## What I built

A working multi-agent system that turns a candidate's rough interview answer into an interview-ready one — through a real orchestration loop against Claude Sonnet 4.5. Every score, every critique, every rewrite is a live API call. Every stop-condition decision is computed from actual model output, not templates.

The number of rounds is not fixed in advance. The loop runs as long as it needs to, and the exact stop reason is surfaced when it fires.

---

## The four-question intro

Per the spec, I asked four MCQ-format questions before generating:

1. **Which fully-narrow workflow?** → Interview Answer Coach
2. **Primary success criteria?** → Composite quality score 0-100 across 5 STAR dimensions (I chose per user's "see according to you")
3. **Stopping conditions?** → All three (plateau + threshold + cap) in the spec-required order (I chose per user's "see according to you")
4. **Auto-design vs customize?** → Auto-design (full 6-agent lineup)

---

## The six agents

Each has its own hand-written system prompt tuned to its role.

**🗺️ Planner** — Reads the rough answer, plans the STAR structure, flags missing information. Returns JSON.

**⚙️ Executor** — Produces the first polished draft from the Planner's plan. Constrained never to invent metrics not in the rough answer.

**📊 Evaluator** — Scores the current draft 0-100 across 5 dimensions (Situation, Task, Action, Result, Polish — 20 each). Returns JSON with the total, breakdown, weakest dimension, and one-line summary.

**🔍 Critic** — Sharp 2-3 sentence critique focused specifically on the Evaluator's weakest dimension. Not restating the score — targeting the exact fix.

**✨ Improver** — Rewrites the draft to address the Critic's feedback, preserving strengths, never inventing facts.

**🎯 Final Reviewer** — Once a stop condition fires, packages the winning draft into a readiness report (final answer, verdict, key strengths, verbal-opening pitch).

---

## The non-negotiable: it's a real loop

The `startLoop()` function implements a genuine `while` loop with a live API call every pass. Not a fixed sequence. Not a hardcoded round count.

```
Setup pass (once):
  Planner (JSON plan) → Executor (first draft)

Loop pass (repeats):
  Evaluator (score JSON) → Critic (critique text)
  → check stop conditions in order:
      1. Plateau (Δ < 3 for 2 straight rounds)
      2. Threshold (score >= 88)
      3. Hard cap (8 rounds — safety fallback, not intended ending)
  → if none fire: Improver (rewrite) → back to Evaluator

Exit pass (once):
  Final Reviewer (readiness report JSON)
```

**State threading is real.** Each Improver call receives the prior round's Evaluator JSON + Critic critique. Each Evaluator call receives the current draft + the original question. History is a running array of `{round, score, breakdown, critique, draft, delta}`.

**Stop check is real.** All three conditions are computed from live scores, in the spec-required order. Whichever fires first wins. The exact stop reason is displayed in a color-coded banner at the end.

**No canned strings.** Every score, every critique, every draft in the UI is the literal text from that round's API response.

---

## Dashboard

Six live views update as the loop runs:

- **Workflow diagram** — SVG showing the loop as a real cycle. Planner → Executor at the top. Evaluator → Critic → Improver → back-arrow to Evaluator forms the loop. Final Reviewer sits on a separate branch that only lights up when a stop condition fires. Active agent glows with a pulse animation.
- **Round indicator** — Reads as open-ended: *"Round 3 — checking stop condition…"* Not "Round 3 of 5."
- **Score progression chart** — SVG line chart plotting the Evaluator score round-over-round with the target threshold marked as a dashed line.
- **Activity log** — Auto-updating feed with timestamp, agent name, message, color-coded per agent.
- **Round-by-round history** — Cards showing each round's total score + delta + STAR dimension breakdown + Critic's critique + the specific draft that was scored.
- **Stop banner** — Color-coded final banner naming which of the three conditions fired.

---

## Post-run report

Once the loop ends, the final section shows:

- **Final answer** — the interview-ready draft, verbatim from the Final Reviewer.
- **Readiness verdict** — one of *ready / almost / needs_more*.
- **Key strengths** — 2-3 items from the Final Reviewer.
- **Verbal opening pitch** — how to introduce the answer in 5 seconds.
- **Agent performance** — API call count per agent.
- **Execution stats** — rounds, total calls, duration, final score, stop reason, retry count.
- **Architecture overview** — a written explanation of the loop, precisely.
- **Extension ideas** — multi-question batch, streaming responses, divergent improvers, cross-session memory, human-in-the-loop.
- **Further prompts** — four follow-up prompts to build more advanced autonomous systems.

---

## Tech notes

- **Single HTML file.** ~65 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no images.
- **Client-side Claude API calls** using `anthropic-dangerous-direct-browser-access: true`. Same pattern as Day 40. User brings their own key.
- **Retry logic** — every API call has 2 retries with exponential backoff. Errors log to the activity feed and count into the retry stat.
- **JSON parsing** — tolerant of preamble/postamble via regex fallback.
- **Sample data pre-loaded** — a realistic behavioral question + rough answer so the loop demos immediately once a key is added.
- **`prefers-reduced-motion`** respected. Pulse animations disable.
- **localStorage persistence** — API key and input form save across reloads.
- **Print-friendly report** at the end.

---

## What this exercise demonstrates

### 1. The interesting problem in multi-agent systems is the stop check, not the agents

The five agents (Planner, Executor, Evaluator, Critic, Improver) are individually simple. The system becomes real when the loop runs an unknown number of times, driven by the actual score trajectory rather than a preset count. The stop check is where multi-agent design earns its complexity.

### 2. State threading is what makes a "loop" different from "a sequence"

Any tutorial can chain N prompts. What makes this a real loop is that every Improver call sees the specific critique from the specific round that just ran, and every Evaluator call sees the specific draft that came out of that Improver call. The history array isn't just for display — it's the ground truth the whole system references.

### 3. Naming the exact stop reason is the difference between an autonomous system and a demo

Every autonomous system needs to end. "It just stopped" is not an acceptable answer. The three-condition check (plateau → threshold → cap) in a defined order, with the winning condition surfaced explicitly, is what makes the system usable — you can look at any run and know exactly why it stopped.

### 4. The Final Reviewer being off the main loop is a real architectural choice

Every previous day's app treated the "final rendering" as part of the main path. This one puts the Final Reviewer on a separate branch — it only runs once a stop condition has fired. Diagrammatically it's a small choice; architecturally it's what turns the pipeline into a proper decision tree.

---

## Narrative continuity

- **Day 40** — AI Assistant Builder (Claude API + BYOK pattern)
- **Day 41** — Interactive Learning Studio (progressive gating)
- **Day 42** — Personal Financial Command Center
- **Day 43** — AI Workflow Architect
- **Day 44** — LinkedIn Roast & Rebuild
- **Day 45** — Decision Strategist
- **Day 46 — Autonomous Agent Studio**

Meta-skill arc caps out here. Day 40 taught single-agent prompt design. Day 46 is that same skill scaled to multi-agent orchestration with a real loop. If the challenge has one build that demonstrates "AI as a system" rather than "AI as a text-generator," this is it.

---

## Tags

`#60DayClaudeChallenge` `#AIAgents` `#Orchestration` `#ClaudeAPI`
