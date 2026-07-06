# Day 37 — Task Compass

**Project:** `task-compass.html` — a management simulation that teaches how work flows through real organizations
**Challenge:** #60DayClaudeChallenge — Day 37
**Type:** Single-file vanilla HTML/CSS/JavaScript. No frameworks, no CDN, no images, no APIs. Works fully offline.

---

## What I built

A three-stage organizational simulation. The player enters a workplace, receives realistic tickets, and makes decisions about **ownership** (Stage 1), **routing** (Stage 2), and **cross-functional collaboration** (Stage 3). The point isn't to test job titles — it's to teach how work actually moves through orgs.

Instead of building for one workplace, I built four fully playable environments — **Tech Company**, **Startup**, **Hospital / Healthcare**, and **Media / Creative Agency** — each with its own role palette (6–8 roles) and its own scenario library (9 scenarios per workplace × 4 workplaces = 36 realistic tasks). Every playthrough starts with the workplace picker; you can replay the same workplace or hop to a different one to compare how the same three stages play differently in different orgs.

---

## The three stages

### Stage 1 · Who Owns This? (3 scenarios)

A single work ticket appears — priority, origin, and body. Below it, all the workplace's role cards are laid out. Drag one role into the "Primary Owner" slot. On submit, the reveal shows:

- The correct primary owner and why they own it.
- Which roles typically assist — with their names badged in each role's department color.
- The reasoning behind the assist list (not just "who else" but *why* they matter).

There's no "correct/wrong" flash. Every answer gets an explanation of the reasoning behind the intended answer.

### Stage 2 · Task Routing (3 scenarios)

A more complex ticket appears. The player builds a workflow chain by dragging role cards into a horizontal lane in the order they'd move the work. Some tasks need three steps. Others need five. The player decides length.

On submit, the reveal shows the "common workflow" pattern with animated flow arrows and a paragraph on why this order is standard. Scoring rewards first-step and last-step matches (who intakes, who closes) as well as overall sequence overlap.

### Stage 3 · Collaboration Challenge (3 scenarios)

Larger situations — "customer satisfaction dropped 15 points," "runway is 6 months," "a public news story runs about a patient safety incident." The player picks one primary owner **and up to 3 supporting teams**. On submit, the reveal names the primary + supporting roles, gives full reasoning, and shows a possible communication flow.

The design lesson here is explicit: complex problems don't have one owner. They have a primary + a coalition.

---

## The four workplaces

Each is a fully self-contained environment with its own role palette, its own vocabulary, and its own scenarios.

### 💻 Tech Company
Roles: Frontend Dev, Backend Dev, QA Engineer, Product Manager, UX Designer, Customer Support, Engineering Manager, Data Analyst.
Sample scenarios: iPhone-only payment failures (Stage 1), enterprise-scale sales inquiry routing (Stage 2), sudden CSAT drop (Stage 3).

### 🚀 Startup
Roles: Founder / CEO, Product Lead, Full-stack Engineer, Designer, Growth Lead, Operations.
Sample scenarios: investor follow-up on financials (Stage 1), scope change from client (Stage 2), can't hit product-market fit (Stage 3).

### 🏥 Hospital / Healthcare
Roles: ER Nurse, Attending, Charge Nurse, Case Manager, Ops Administrator, Billing/Coding, IT/EHR Support, Social Worker.
Sample scenarios: deteriorating ER patient (Stage 1), insurance denial routing (Stage 2), patient safety incident news story (Stage 3).

### 🎬 Media / Creative Agency
Roles: Account Lead, Strategist, Copywriter, Art Director, Producer, Video Editor, Project Manager.
Sample scenarios: brand book mismatch complaint (Stage 1), scope change mid-project (Stage 2), campaign underperformance (Stage 3).

Every scenario in every workplace ships with the same schema: task text, priority, origin, correct primary owner/sequence/team, reasoning, and (for Stage 3) a possible communication flow.

---

## Scoring — understanding, not points

Instead of a raw score, the app tracks four dimensions of **understanding**:

- **Ownership** — did you identify the primary owner correctly?
- **Delegation** — did you match the first and last handoff in a workflow?
- **Collaboration** — did you pull in the right supporting teams?
- **Workflow Thinking** — did you match the common sequence?

Each dimension normalizes to a 0–100 score at the end. The dashboard renders both an animated bar chart AND a hand-drawn SVG radar chart with four axes. The radar makes it visual which of the four dimensions you leaned on hardest and which you underinvested in.

---

## Final reflection

At the end, the app generates four personalized paragraphs based on your dimension scores:

1. **What you understood well** — computed from your strongest dimension.
2. **Where you tended to over-assign responsibility** — triggered if your Ownership was high but your Collaboration was low (a classic "everything has one owner" bias).
3. **Where you underestimated collaboration** — triggered if Collaboration scored under 60 (a common pattern for individual contributors playing this for the first time).
4. **One insight about how organizations actually work** — a closing paragraph on why "workflow thinking" is the real transferable skill.

No labels. No categorization of the user. Just observation of the specific patterns their session showed.

---

## Game feel

- **Task ticket aesthetic** — every scenario appears as an amber-toned ticket with rounded chevron cutouts on the sides, like a workshop request slip. Priority + origin metadata sits below the body.
- **Role cards as collectible items** — each role card has its department color as a bottom stripe, an emoji icon, and a distinct hover lift. Roles from the same department share the same color, so the player learns the org's color palette quickly.
- **Snap animations** on drop.
- **Toast notifications** after every submission.
- **Confetti burst** when the final understanding average passes 70.
- **Progress stepper** across all 5 stages (Welcome + 3 stages + Dashboard).
- **Glass-panel dark mode** with animated gradient backgrounds on the accent glow.
- **Radar chart** rendered inline in SVG with no library — the polygon fills with a linear gradient from sky-blue to purple.

---

## Tech notes

- **Single HTML file.** ~65 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no external assets, no APIs.
- **Native HTML5 drag-and-drop** with **click-to-move fallback** — every draggable role also responds to click and Enter/Space keydown, so the game works on touch devices and via keyboard.
- **Four full workplaces** — Tech, Startup, Hospital, Media Agency. Total 36 scenarios (9 per workplace × 4). Roles reused across stages within a workplace.
- **Reusable role-card renderer** — every stage renders roles through the same `roleCardHtml()` function.
- **Reusable drop-zone pattern** — every stage uses the same `.dropzone.hover` CSS + drag/drop event scaffold.
- **State machine** — single `goto(next)` navigator; every stage renders from an isolated function.
- **`prefers-reduced-motion`** respected — all animations disable when that OS setting is on.
- **SVG radar chart** rendered inline with computed polygon geometry.
- **Confetti** as pure CSS keyframes, no library.

---

## What this exercise demonstrates

### 1. Real orgs teach through pattern, not diagram

Reading an org chart tells you who reports to whom. Playing a task ticket teaches you which decisions actually get made in which chairs. Same underlying information, radically different learning. This app is a bet that simulating one *task at a time* is more useful than presenting a hierarchy diagram.

### 2. Four workplaces × same architecture = the actual lesson

The most useful insight isn't which owner is correct in the Tech scenarios — it's noticing that the *same three-stage pedagogy* (ownership → routing → collaboration) generalizes to Hospitals, Agencies, Startups, and every other org shape. If you understand the pattern in one workplace, you can transfer it to any workplace you enter.

### 3. Collaboration is the higher-order skill

Every stage builds toward Stage 3. Stage 1 teaches the vocabulary. Stage 2 teaches sequence. Stage 3 teaches the meta-lesson: complex problems have a primary owner *and* a coalition. Users who play Stage 1 well but underperform on Stage 3 are the exact audience the reflection is written for.

### 4. Simulation beats quiz for org design lessons

A multiple-choice quiz measures whether you know that a QA Engineer files bugs. A drag-and-drop simulation measures whether you'd actually route work through them. That's the difference between recognition and internalization.

---

## Narrative continuity

- **Day 32** — marketing strategist simulator
- **Day 33** — media literacy analyzer
- **Day 34** — marketing detective
- **Day 35** — prompt puzzle
- **Day 36** — cognitive pattern explorer
- **Day 37 — task compass**

Meta-skill arc continues. Day 36 turned self-reflection into a game. Day 37 turns organizational thinking into a game. Same "teach → observe → commit → reveal" spine every time.

---

## Tags

`#60DayClaudeChallenge` `#OrganizationalDesign` `#WorkflowThinking`
