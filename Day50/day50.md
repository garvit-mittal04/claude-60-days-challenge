# Day 50 — Defend Your Experience

**Project:** `defend-your-experience.html` — an interview simulator that extracts every claim from your resume / bio / project doc and grills you on each one until you've earned the right to say it
**Challenge:** #60DayClaudeChallenge — Day 50
**Sharpness default:** Hostile grill (Recommended for interview prep)
**Type:** Single-file HTML/CSS/JavaScript. No frameworks, no CDN, no API key required.

---

## What I built

A tool that flips the resume-improvement premise. Every other resume tool wants to make what's on your page better. This one assumes the page is fine and asks the harder question: **can you actually defend any of it?**

Paste any experience — a resume, a LinkedIn About section, a project writeup, a research abstract. The tool extracts every specific claim (metric, verb, outcome, tool) and treats each as something that has to survive one-on-one questioning. Then it asks. And asks again. And scores each answer.

The final Defense Report shows which claims survive under pressure and which will collapse the first time a real interviewer asks a follow-up.

---

## The three-question intro

Per the spec, three MCQs before generating:

1. **Experience type** → Universal (works for resumes, projects, research, bios interchangeably)
2. **Skeptic sharpness** → Hostile grill (Recommended for interview prep)
3. **Seed content** → Yes, a fictional analyst resume for cold viewers

Visual style question was skipped — my memory already indicates the recurring aesthetic (dark editorial SaaS, serif headlines, purple/cyan accents), so the tool inherits it directly.

---

## The architectural inversion

Most resume tools have this shape:

```
Your resume → tool suggests improvements → better resume
```

This one has this shape:

```
Your resume → tool extracts claims → tool asks
"can you defend this?" → your answer scored → weak claims flagged
```

The insight is that a resume improvement isn't measured by what appears on the page. It's measured by whether the version of you talking about the page survives contact with a sharp interviewer. Bullets get tightened all the time. What almost never gets rehearsed is the answer to "walk me through that 22% churn reduction — how did you measure it and what was your specific role?"

That question is where most resumes collapse. The tool exists to make that question unavoidable during prep, so it stops being fatal during interviews.

---

## How claim extraction works

The tool parses pasted text line-by-line and flags any line as a claim if it:

- Starts with a strong action verb (`led`, `built`, `analyzed`, `automated`, `owned`, `migrated`, `mentored`, and 30+ others)
- Contains a specific metric (`%`, `$`, `hours`, `users`, `x` multipliers, seconds/ms)
- Contains a proper-noun tech stack (`SQL`, `Snowflake`, `Looker`, `dbt`, `AWS`, etc.)

Skips headers, contact info, pure skill lists, and standalone title lines. Extracts up to 20 claims per session (a realistic interview volume — anything longer stops teaching).

For each claim, the tool computes an initial strength score (10-95) based on:

- Whether it has a specific metric (+25)
- Whether it starts with a strong verb (+15)
- Whether it names a causal chain (+10)
- Whether its length lands between 10-30 words (+10)
- Whether it contains vague filler words like "optimized", "leveraged", "spearheaded" (-15)

The initial score orders the queue — weakest claims first, so the interview builds momentum toward the sharpest bullets.

---

## The 8-category challenge library

40+ hand-written challenge templates across 8 categories, matched to claim content:

- **METRIC PROBE** — "You wrote '22% reduction'. What was the baseline? How did you measure it? Would your former manager use the same number?"
- **OWNERSHIP PROBE** — "You wrote 'led' or 'owned'. Who else was in the room, and what did they do? Name them."
- **METHOD PROBE** — "Walk me through week one. What was the first thing you built? What did you consider and reject?"
- **IMPACT PROBE** — "What would have happened if you'd done nothing? Give me the counterfactual."
- **SCOPE PROBE** — "How many users, how many dollars, how many weeks? Real numbers."
- **VAGUENESS PROBE** — "'Optimized' — from what to what? Give me numbers or replace the word."
- **FAILURE PROBE** — "What's the worst thing that happened during this project? Something you'd rather not tell me."
- **TIMELINE PROBE** — "How long did this actually take? Not the success-story version — the real timeline including detours."

Templates are personalized via token substitution — `{metric}`, `{verb}`, `{vague_word}` — so the same template produces different questions on different claims. The hostile sharpness mode asks 3 challenges per claim from 3 different categories. Friendly asks 1. Balanced asks 2.

---

## Answer scoring — two modes

If the tool is loaded inside Anthropic's Claude artifact environment, it uses live model calls for both question generation AND answer scoring (`window.claude.complete()`). The environment badge in the header flips green.

Otherwise, it falls back to a rules-based scorer that grades answers 0-100 on six signals:

- Word count (30+ words: +15; 60+: +10; 120+: +5)
- Contains specific numbers (+15)
- Contains proper nouns / names (+10)
- Uses first-person ("I", "my") (+5)
- Uses causal language ("because", "resulted in", "drove") (+12)
- Names alternatives ("considered", "instead of", "trade-off") (+10)
- Acknowledges failure or regret (+8)
- Contains vague filler words (-12)

A live pattern-score preview updates under the answer box as the user types, so they can see their score climbing in real time. Encourages specificity through immediate feedback.

---

## The Defense Report

Final visualization has three layers:

**Layer 1 — the number.** A donut chart with the overall Defense Score (average across all answered questions). Colored green ≥75, amber 55-74, red <55.

**Layer 2 — the verdict.** One-sentence read of the whole defense ("Strong defense" / "Middling — a sharp interviewer would follow up on the weak spots" / "Your resume is stronger than your defense" / "Rehearsal required").

**Layer 3 — the buckets.** Three sections — Fix these first, Sharpen these, Ship these as-is — each with the full Q&A trail per claim, the score, and a rewrite hint targeting the specific weakness category.

Exportable as Markdown (for a rehearsal doc) or JSON (for reuse).

---

## Design decisions worth naming

### 1. Not a runtime

The tool doesn't hit Anthropic's API from GitHub Pages. When loaded there, it uses the rules-based skeptic — which was designed to be genuinely strong on its own, not a placeholder waiting for real AI. 40+ templates hand-written by someone (me) who's spent 50 days thinking about prompt engineering probably outperforms a naive LLM call on this specific task.

### 2. Dropping the API key ask

The spec explicitly said no API key, no backend. Past days used BYOK to get around this. Day 50 doesn't. The rules-based challenge engine has to carry the experience alone for the public deploy, and it does.

### 3. The sample resume is fictional

"Jordan Chen" is invented. Every metric is invented. This is intentional — it lets first-time viewers click "Try demo" and see the interview run without pasting anything personal. The claims are realistic enough to teach the mechanic without any privacy exposure.

### 4. Session state persists

localStorage saves your last session so if you close the tab mid-interview, you can resume. Answers scored, weak claims flagged — everything survives a reload.

---

## Tech notes

- **Single HTML file.** ~60 KB. Vanilla CSS + JavaScript. No frameworks, no CDN.
- **Drag-drop or paste** input. `.txt`, `.md`, or paste any text.
- **Live pattern scoring** as user types — immediate feedback on answer strength.
- **Environment detection** — badge in header flips between "live claude" and "pattern skeptic" depending on availability.
- **Dark mode default + light mode toggle** via CSS custom properties.
- **Keyboard shortcut** — ⌘Enter to submit answer.
- **`prefers-reduced-motion`** respected.
- **Fully responsive** — sidebar collapses on narrow screens.
- **Toast notifications** for score feedback per answer.

---

## What this exercise demonstrates

### 1. The output is a rehearsal, not a document

The Markdown export isn't a résumé. It's a rehearsal script — the exact questions you got, the answers you gave, the scores. You take that into an actual mock interview and drill the low-scoring claims out loud until they land.

### 2. The unit of value is the question, not the answer

Every prior resume tool ships you polished bullets. This one ships you the questions those bullets provoke. The interviewer's brain is now on your side of the table, which is the harder engineering problem.

### 3. Hostile beats friendly for prep

The default sharpness is "Hostile grill" for the same reason boxers spar with faster opponents — the real interviewer will feel easier than your practice tool. Friendly and Balanced modes exist but Hostile is where the actual value lives.

---

## Narrative continuity

- **Day 44** — LinkedIn Roast & Rebuild (external voice critiquing your profile)
- **Day 45** — Decision Strategist (analyze one high-stakes decision)
- **Day 48** — Compare & Decide Builder (weighted-criteria decision tool)
- **Day 49** — Personal AI Playbook (reusable prompt systems)
- **Day 50 — Defend Your Experience (adversarial interview simulator)**

Day 50 caps the personal-tooling arc. Days 48-49 were about how to think and how to ask. Day 50 is about how to answer. Together they cover the three moments where a professional either survives scrutiny or doesn't — deciding, prompting, defending.

Ten builds left.

---

## Tags

`#60DayClaudeChallenge` `#InterviewPrep` `#AI` `#BuildInPublic`
