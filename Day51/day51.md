# Day 51 — Capstone Kickoff: Product Discovery & Sprint Planning

**Project:** Kickoff — an analysis brief tool for analysts and ops pros
**Challenge:** #60DayClaudeChallenge — Day 51 · Capstone Day 1 of 10
**Deliverables:** PRD, Implementation Blueprint (Days 52-60), Pitch Deck
**Ship target:** Day 60

---

## What today was

Not a build day. A sprint-planning day.

Day 51 begins the 10-day capstone that closes the challenge. The task was to interview my way to a real project, define its scope with precision, then produce three professional deliverables — a PRD, an implementation blueprint for the remaining nine days, and a pitch deck — before writing a single line of code.

For 50 days, I've been building one single-file HTML app per day and posting the writeup. This capstone is different. It's one project across ten days, with a shipped v1.0 at the end that real people can use.

---

## The project: Kickoff

**Tagline:** The analysis brief you write before you write any SQL.

**Who it's for:** Analysts and operations pros at nonprofits and mid-market companies.

**The pain it solves:** Executives ask questions that are not yet answerable — "why is retention down?" is not one data question but ten data questions in a trench coat. The analyst who receives that ask usually either (a) guesses wrong and spends four hours on the wrong analysis, (b) asks a clarifying question in Slack and waits six hours, or (c) answers literally and ships shallow work.

**What Kickoff does:** Paste the vague exec question. Get 4-6 adaptive clarifying questions from Claude. Answer them in seconds each. Receive a structured brief — decomposed sub-questions, ranked hypotheses, data sources per sub-question, analysis approach, executive summary template, definition-of-done. Every brief gets a shareable permalink you can send to the exec for scope alignment *before* real work starts.

---

## The interview that produced the scope

Rather than picking a project and defending it, I let a structured interview surface the strongest realistic option. Seven MCQ-shaped questions across:

1. **Capstone purpose** → Real product with actual users
2. **Target audience** → Analysts + ops pros at nonprofits + mid-market companies
3. **Core pain** → Translating exec questions into analysis plans
4. **Stack ambition** → Single-file HTML + real backend proxy (Cloudflare Workers + Anthropic API)
5. **Output shape** → Shareable permalink per brief
6. **Success criteria** → 50+ briefs created, 10+ shared, 5+ named testimonials by Day 60
7. **Product name** → Kickoff

Each answer gated the next question. Each question was calibrated to remove one specific scope ambiguity. By question seven, the project shape was unambiguous enough to write a PRD against.

---

## The three deliverables produced

### 1. Product Requirements Document (PRD)

A professional 14-section PRD covering: problem statement, target personas (Priya the senior ops analyst; Marcus the program director asking the question), product vision and positioning, complete functional and non-functional requirements, success metrics, explicit non-goals, technical architecture, assumptions and dependencies, risks and mitigations, distribution and growth strategy, and an appendix with a full example brief.

The PRD is the alignment doc — the single thing anyone joining the project (an advisor, a mentor, future me) can read to understand exactly what's being built and why.

### 2. Implementation Blueprint · Days 52-60

The largest deliverable. A per-day plan for the remaining nine days with, for each day: objective, what I'll learn, features to build, step-by-step implementation plan, files and folders, APIs / libraries / services to integrate, testing tasks, common issues and debugging tips, end-of-day checklist, expected project state, and handoff notes for the next day.

The point of the blueprint is that if I hand any single day's section to a fresh AI conversation, it should have enough context to help me continue the build with zero re-planning. The blueprint is the compression of Day 51 into a permanent scaffold.

### 3. Project Pitch Deck

A 12-slide dark-editorial pitch deck: problem, cost of the problem, target user persona, solution, key features, technical approach, success metrics, future scope (v2 / v3), vision, and a closing timeline slide showing the nine build-days ahead. Built to be shown to a mentor, an advisor, an investor down the line, or as a standalone artifact for the portfolio.

---

## What v1.0 explicitly excludes

Scope creep kills 10-day capstones. Everything on this list is a good idea. Everything on this list is v2 or later.

- User accounts / signup / login
- Brief editing after creation
- Comments or collaboration on shared briefs
- Templates library
- Data source connectors (Salesforce, Snowflake, etc.)
- Slack app / bot / slash command
- Team / workspace features
- Any paid tier or monetization
- Mobile-optimized UI

---

## The 10-day path

- **Day 52** — Design, prompts, and domain
- **Day 53** — Backend proxy + deploy
- **Day 54** — Landing page + interview UI (static shell)
- **Day 55** — Interview logic + live brief generation
- **Day 56** — Persistence + permalinks
- **Day 57** — Landing polish, example briefs, analytics
- **Day 58** — Beta testing + edge cases + bug bash
- **Day 59** — Launch prep + content
- **Day 60** — Launch day: ship + monitor + wrap

Each day is calibrated for roughly 4-6 hours of focused work, leaving room for job search and life.

---

## What I learned today

### 1. Interview-first is a real discipline

I've drafted product ideas alone before. Every time, I optimized for the idea I liked most. When I interviewed myself instead — via well-structured MCQs that each removed one specific scope ambiguity — I arrived at a project that was less "cool" but more realistic and more differentiated. The interview format forced me to answer questions I would have skipped alone.

### 2. Distribution is a scope decision

Two of the seven questions were about distribution, not product. The target audience (analysts + ops pros) was picked before the pain point. This inverted my usual instinct (pick the cool problem, then figure out who wants it). Deciding audience first meant every subsequent product decision had to survive the "will *that* audience actually use this?" test.

### 3. Excluding features is more work than including them

The v1.0 exclusion list is longer than the inclusion list. Writing it down explicitly, in a PRD anyone can point at, is the mechanism that will save the next nine days from scope creep. Not the intention. The written artifact.

---

## Narrative continuity

- **Days 1-50** — 50 single-file HTML builds shipped daily
- **Day 51 — Capstone kickoff: product discovery + PRD + blueprint + pitch deck**
- **Days 52-60** — Building and shipping Kickoff v1.0

The capstone is the first time in this challenge the tool has to survive contact with real users who did not ask for it. Every prior build was a demo. This one has to work.

---

## Tags

`#60DayClaudeChallenge` `#ProductDiscovery` `#Capstone` `#BuildInPublic`
