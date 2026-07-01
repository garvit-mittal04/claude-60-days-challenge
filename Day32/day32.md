# Day 32 — Think Like a Marketing Strategist

**Project:** `think-like-marketing-strategist.html` — beginner-friendly marketing strategy simulator with reusable Claude prompts embedded at every step
**Challenge:** #60DayClaudeChallenge — Day 32
**Type:** React single-file application (via CDN + Babel) with three modes: Business, Personal Brand, Random Client.

---

## What I built

A single self-contained HTML app that walks the user through the actual thinking sequence a real marketing strategist follows — **audience → platforms → content pillars → 30-day roadmap → unexpected event → growth report**. Each stage teaches *what the concept means* and *why it matters* in plain English before the user picks anything, then hands them a reusable Claude prompt calibrated to their actual inputs.

**Three modes** the user picks at the start:

- 🏢 **Use My Own Business** — user provides business name, industry, audience, and 30-day goal
- 🙋 **Build My Personal Brand** — same shape but the "product" is the user's name, expertise, and story
- 🎲 **A New Client Has Arrived** — a randomly generated business drops in with industry, audience, budget, competitors, and stated marketing challenge

Every downstream stage adapts to the mode. Personal brand mode swaps "competitors" for "people in your space you admire," weights LinkedIn / X / YouTube / Newsletters more heavily, and includes content pillars like Thought Leadership, Personal Story, Behind the Scenes, and Audience Education.

---

## The stages

### 1. Audience Step
Explainer: understanding the audience is not demographics — it's understanding what they believe, what they want, and the phrases they use to describe their problem. Prompt card: get Claude to write a detailed Ideal Customer Profile using the user's actual context.

### 2. Platform Step
Explainer: the best content in the wrong place beats mediocre content in the right place, but not by much. User picks 3 platforms from a mode-specific list (personal brand list weights LinkedIn, X, YouTube, Newsletter, Podcast; business list includes Facebook, Pinterest, TikTok). Each option shows a "Best for" tag and a "why it fits" reason.

### 3. Content Pillars Step
Explainer: three pillars is the sweet spot. Beginners pick more, positioning stays fuzzy, audience can't remember what you're about. User picks exactly 3 from 6-8 options each tied to a specific goal.

### 4. 30-Day Roadmap Step
Weekly goals (not daily posts). Personal brand Week 1 focuses on defining POV and optimizing bio/profile — per the task spec. Business Week 1 focuses on foundation and audience research. All four weeks have the strategist rhythm: define → ship → distribute → measure.

### 5. Unexpected Event Step
A randomized marketing event appears. For business mode: viral post, competitor copying positioning, PR crisis. For personal brand: viral post, podcast invite, public disagreement, someone copying content, sudden follower spike. User picks a response, sees the consequence in real terms, and gets a Claude prompt to draft the actual public response.

### 6. Growth Report
Four scores (Audience Understanding, Platform Strategy, Content Strategy, Growth Potential), best decision + biggest mistake computed from actual choices, and three marketing lessons customized to mode (personal brand lessons reference authenticity, consistency, niche clarity; business lessons reference positioning, distribution, metrics). Final prompt card turns the 30-day report into a 90-day plan.

---

## The "How to ask Claude" prompt cards

**Eight prompt cards embedded across the flow.** Each one:

- Explains WHY this specific prompt shape works
- Shows the ready-to-copy prompt calibrated to the user's actual context
- Includes a Copy button (uses `navigator.clipboard.writeText`)
- Uses code-block styling so it looks like a snippet, not marketing copy

The prompt cards are the actual pedagogical hook. The user learns marketing strategy *and* prompt engineering at the same time. By the end of one playthrough, they've collected 8 reusable prompts they can save and reuse across future campaigns — audience research, platform comparison, pillar generation, roadmap breakdown, event response, 90-day planning, and two more.

---

## Tech notes

- **Single HTML file.** ~60 KB. React + Babel via CDN. No frameworks, no Tailwind, no APIs, no images, no external assets.
- **Reusable React components with useState.** `Stepper`, `Welcome`, `ContextInput`, `RandomClientCard`, `AudienceStep`, `PlatformStep`, `PillarStep`, `RoadmapStep`, `EventStep`, `GrowthReport`, plus the reusable `PromptCard`.
- **State machine.** A single App component holds `stage`, `context`, `platforms`, `pillars`, `event`. Every stage transition preserves everything.
- **Mode-adaptive libraries.** Two separate `PLATFORM_LIBRARY` and `PILLAR_LIBRARY` objects — one keyed to `business`, one to `personal`. The right list surfaces based on mode automatically.
- **Randomization.** New business + new event on every playthrough via the Replay button.
- **Copy-to-clipboard** on prompt cards with visible confirmation.
- **Dark modern UI.** Purple + cyan brand accents, gradient card headers, animated hover states, progress bar at the top.

---

## Key learnings

**1. Teach-before-decide is the foundational pattern across the challenge.**
Days 28, 30, and 32 all use the same pattern — beginner-friendly explainer before every decision. That structure isn't a coincidence. It's what separates a decision game from an actual educational tool.

**2. Prompt cards are the multiplier.**
An educational tool that only teaches *concepts* is a lesson. An educational tool that also teaches *how to ask AI about those concepts* is a workflow. That second layer is what makes users come back — because they've learned a reusable skill, not just consumed a lesson.

**3. Personal brand is a genuinely different pattern from business marketing.**
It's not a subset. The Week 1 focus is different (POV vs. audience research). The pillars are different (Personal Story exists in one, doesn't exist in the other). The platforms weight differently. The lessons at the end reference completely different frameworks (authenticity vs. positioning). Building both meant designing two parallel content libraries — the effort was worth it because personal brand builders are a huge subset of the audience for this kind of tool.

**4. Randomization + a Replay button changes the ROI of the tool.**
The user who runs the simulator once gets one strategy walk-through. The user who runs it three times — across a business, their own personal brand, and a randomized client — has internalized the pattern in a way that transfers. Replayability is a teaching device, not just entertainment.

---

## What surprised me most

The random client mode ("A New Client Has Arrived") turned out to be more teaching-rich than either of the other two. Because the business is *not the user's*, they can be honest about it without ego. They see the audience clearly, they diagnose the challenge without defensiveness, and they pick platforms without the "I already use Instagram" bias.

Building a strategy for a fictional client is the fastest way to internalize how a strategist thinks — because you don't have skin in the game emotionally. That's the whole point of case-study teaching in MBA programs, and the random-client mode is that exercise in browser form.

---

## Narrative continuity

This week the challenge shifted from healthcare and supply chain operations (Days 26–31) into marketing strategy — a completely different domain. But the underlying pedagogical pattern is the same:

- Concept before decision
- Trade-off after choice
- Personalized feedback in the report
- Replayable with randomized inputs

The specific vocabulary changes across domains. The teaching architecture doesn't. That's the actual meta-lesson of the 60-day challenge — how to design educational simulators that generalize across any domain by holding the pedagogical scaffolding constant while swapping the content.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
