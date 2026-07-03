# Day 34 — Marketing Detective

**Project:** `marketing-detective.html` — a detective-game marketing analyzer
**Challenge:** #60DayClaudeChallenge — Day 34
**Type:** Single-file vanilla HTML/CSS/JavaScript application. No frameworks, no CDN, no images, no APIs.

---

## What I built

A marketing case-solving detective game. Every playthrough drops a fictional marketing case on your desk — company, industry, objective, budget mix, campaign metrics, customer comments, and three anonymized clues from an "analyst's notebook." Your job is to read the evidence, look at the corkboard, and identify the one primary marketing mistake that explains everything.

The whole thing is styled like a detective's office: kraft-paper case folder with a red UNSOLVED stamp, a wooden corkboard with sticky notes pinned by push pins, IM Fell English serif for headlines, Courier for the interface. Five color themes with Claude Orange as the default.

Thirteen fictional cases in the library, spanning D2C, B2B SaaS, local services, wellness, media, industrial supply, and agency services. Each case ships with all 13 required fields: company name, industry, campaign objective, target audience, marketing channels, budget allocation, campaign metrics (reach, CTR, engagement, conversions, sales), customer comments, social media performance, one primary marketing mistake, three supporting clues, correct explanation, and suggested improvements.

---

## The six-stage detective flow

**1. Case Assignment.** A folder lands on the desk. Kraft paper background, red UNSOLVED stamp rotated -8 degrees, case number, company name in serif, industry in Courier. The folder's meta-grid shows the objective, audience, channels, and budget mix as small pill tags.

**2. Investigation Board.** A wooden corkboard fills the screen. Eight sticky notes are pinned to it — campaign metrics, social performance, three analyst clues, and three customer comments. Each note is a different pastel color, tilted at a slightly different angle, with a red push pin at the top. Every note is draggable (HTML5 drag-and-drop) so users can physically rearrange evidence the way an analyst would.

**3. Solve the Case.** Four multiple-choice buttons. Each option is written as a real hypothesis (positioning, channel, funnel, budget, etc.) with a category tag and a short justification. Only one is the primary mistake — the other three are real issues in the case but not the root cause. That's the pedagogy: teach the user to distinguish between contributing factors and the primary lever.

**4. Case Closed.** The verdict stamps down. Correct answers get "CASE CLOSED" in green with a confetti burst. Wrong answers get "SOLVED (WITH A NOTE)" — still validating, but honest that the pick wasn't quite the root cause.

**5. Learning Report.** The teaching payload. The primary mistake is named at the top. The user's verdict is graded. A full expert explanation walks through the reasoning that connects the evidence to the mistake. Three suggested improvements give the user something operational to take away. The original budget allocation is visualized as an animated bar chart so the user can see where the money actually went.

**6. Take a New Case.** One click reshuffles to a different case. The stepper progress bar resets. No two playthroughs are the same.

---

## The pedagogy — "distinguish root cause from contributing factors"

The reason every case has four multiple-choice options — not two — is deliberate. Each incorrect option is a plausible marketing issue that shows up in the evidence. Bad follow-up. Weak content. Budget too small. Wrong channel. In real cases, all of these often coexist.

The lesson is that only one of them is the primary lever — the one that, if fixed, moves the outcome. Everything else is contributing factors. A junior marketer sees all four issues and wants to fix all four. A senior marketer identifies the one that unblocks the others.

Every case in the library is structured around that distinction. Meridian Music School has real customer comments about ad creative AND about follow-up AND about audience. All three are real. Only follow-up is the root cause — because 79% of leads never got contacted, and no amount of ad-creative improvement would have converted them.

That's the transferable skill this game teaches: separate the noise from the primary lever.

---

## Why the detective aesthetic

A marketing dashboard is not memorable. A detective board is. When the same content is framed as evidence to be examined instead of numbers to be reviewed, users engage differently — they read the customer comments as testimony, treat the metrics as forensic data, and take the "solve" moment seriously in a way they wouldn't take a "submit answer" button.

The corkboard is the single most important design choice. It signals "look at all of this together" instead of "process each field individually." That framing is what turns marketing analysis from a checklist into a pattern-matching exercise.

---

## Tech notes

- **Single HTML file.** ~50 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no images, no external assets, no APIs.
- **Five color themes** — Claude Orange (default), Noir, Forest Case, Ruby, Warm Sepia. Theme swap flips a `data-theme` attribute; all colors resolve via CSS custom properties, so no re-render is needed.
- **HTML5 drag-and-drop** on every sticky note. Users can physically rearrange evidence — the drop logic reflows the corkboard grid.
- **Editorial typography** — IM Fell English serif for headlines, Courier for the interface. Special Elite as the stylistic fallback body font when available.
- **CSS keyframe confetti** on correct verdicts. Pure CSS animation, no library, 100 particles.
- **Six-stage state machine.** Every stage renders from a single `goto(next)` navigator that updates the top progress bar and re-renders the stage.
- **Randomized case pool.** 13 fictional cases; every "Take a New Case" reshuffles without repeats.
- **Weighted scoring by choice.** The correct choice is baked into every case; the report grades whether the user's pick was the root cause.
- **Personalized reveal** — the report calls out the user's specific verdict versus the actual primary mistake, so the reveal is different for every user.

---

## The 13 fictional cases

Each one models a real marketing failure pattern:

| # | Company | Industry | Root cause taught |
|---|---|---|---|
| 1 | Lumen Brew Coffee | D2C coffee subscription | Acquisition/product promise mismatch |
| 2 | Vertex Analytics | B2B SaaS | No differentiation vs incumbents |
| 3 | Riverwood Wellness | Wellness retreats | Broken site conversion path |
| 4 | Solstice Auto Detailing | Local services | Weak local SEO / Google presence |
| 5 | BluePine Craft Kits | D2C kids subscription | "Gift" positioning kills Q1–Q3 |
| 6 | Nordic Notes | Paid newsletter | Free tier too generous |
| 7 | Halberd Industrial | B2B distribution | Customer experience, not marketing |
| 8 | Meridian Music School | Consumer education | Sales follow-up broken |
| 9 | Prairie & Pine Home | D2C furniture | No repeat purchase (CAC > LTV) |
| 10 | Fenwick Fitness App | Fitness app | No activation flow |
| 11 | Cascade Cybersecurity | B2B cybersecurity | Positioning vs named incumbents |
| 12 | Verdant Meal Kit | D2C food | Ads over-promise, product under-delivers |
| 13 | Sable Digital Agency | B2B agency | Portfolio anchors pricing tier |

Every case is fictional but every root cause is a pattern I've seen in real teardowns. The point isn't to test recognition of these specific companies — it's to train the pattern.

---

## What this exercise demonstrates

### 1. Teaching frameworks generalize; content doesn't

Same architecture as Day 32 (marketing strategist) and Day 33 (media literacy): teach-before-decide, guided discovery, personalized reveals. Only the domain changed. The lesson: educational simulators generalize across domains as long as the teaching architecture is stable and the content is well-chosen.

### 2. Multiple-choice with plausible distractors is a better teacher than binary right/wrong

The four-choice format — where every wrong option is a real issue but not the root cause — is what separates this from a quiz. It forces the user to think in causal chains: "which of these, if I fixed it, would unlock the others?"

### 3. Aesthetic isn't decoration — it's framing

The corkboard, sticky notes, and case folder aren't decoration. They tell the user, before any content loads, how to read the material. "Look at everything together. This is evidence. Something connects it." A dashboard sends the opposite signal. Framing is doing pedagogical work.

### 4. Named companies (even fictional) beat generic scenarios

"Lumen Brew Coffee" is more memorable than "a hypothetical coffee subscription company." Named cases create narrative memory. The lesson sticks to the story, and the story sticks to the pattern.

---

## Narrative continuity

- **Day 22–24** — startup validation, MVP blueprint, business strategy
- **Day 26–28** — healthcare workflow simulators
- **Day 29–31** — supply chain crisis simulators
- **Day 32** — marketing strategist simulator
- **Day 33** — media literacy analyzer
- **Day 34 — marketing detective**

Marketing arc: strategy → analysis. Day 32 asked the user to make marketing decisions. Day 34 asks the user to diagnose marketing decisions someone else made. The two together teach the same skill from two angles — one prospective, one retrospective.

---

## Tags

`#60DayClaudeChallenge`
