# Day 44 — LinkedIn Roast & Rebuild

**Project:** `linkedin-roast-rebuild.html` — a single-file LinkedIn optimizer that roasts your profile, rebuilds every section, and gives you a 7-day activation plan
**Challenge:** #60DayClaudeChallenge — Day 44
**Engine:** Dual — Rule-based (offline, instant) + optional Claude API (bring your own key)
**Type:** Single-file vanilla HTML/CSS/JavaScript. No frameworks, no CDN, no images.

---

## What I built

An interactive tool that takes your current LinkedIn profile — headline, About, most recent experience, top skills, and target goal — and returns a five-part output: a Roast (scored 5 sections + overall out of 100), a Rebuild (three headline options, complete About rewrite, before/after Experience bullets, skills add/remove/pin), a Before/After Scorecard, a 7-Day LinkedIn Activation Plan (with drafted posts and connection messages), and a Shareable Summary Card designed to be screenshotted and posted.

Two engines. Rule-based mode runs entirely offline against 20+ pattern checks — buzzword detection, weak verbs, missing numbers, first-two-lines hook analysis, soft-skill dilution. Instant results, no API key. Optional Claude API mode calls Sonnet 4.5 with a structured JSON schema for richer, more specific output. Toggle in the header.

Sample profile pre-loaded so the tool demonstrates its output shape on first load.

---

## The single-question intro (per task spec)

The task spec asked to gather 5 fields from the user before generating. Rather than interviewing the user in chat, I built those 5 fields into the app's input form — so any visitor can drop in their profile and get an instant read. Fields: current headline, About section, most recent role (title + company + bullets), top skills, target goal.

Additionally I asked one clarifying question up-front: **should the analysis be rule-based, Claude API, or both?** — user chose **both**, so the app ships with a header toggle.

---

## Part 1 · The Roast

Every section gets a 1-10 score and a recruiter's real 3-second reaction. Every problem is:

- **❌ The specific issue** (quoting the user's own words back at them)
- **🧠 Why it hurts** (how recruiters interpret it)
- **🔍 The invisible cost** (what opportunities they're losing)

**Section detection logic:**

- **Headline** — length check, 40+ buzzword blacklist, "who/what" specificity check, aspiring/student framing detection.
- **About Hook (first 2 lines)** — the ONE place buzzwords hurt most because it's all a scroller sees before "see more." Special detection for one-long-sentence-with-no-period hooks.
- **About Full** — number density, corporate speak, CTA presence, first-person voice check.
- **Experience** — weak-verb detection ("responsible for," "assisted with," "helped"), number presence, strong-verb starters, bullet count.
- **Skills** — count vs LinkedIn's 50-limit, soft-skill dilution, target-goal-specific hard skill gaps.

Every score change is defensible: the reason is quoted, the fix is named.

---

## Part 2 · The Rebuild

**Headline — three options.** Keyword-optimized (for recruiter search), value-proposition (for client attraction), authority-style (for thought leadership). Each with a strategy paragraph explaining when to use it.

**About section — complete rewrite.** Structured per the spec: hook (line 1-2 before "see more"), story (line 3-5 with tools + context), proof (line 6-8 with specific projects and numbers), CTA (last line). Every rebuild lists the embedded LinkedIn SEO keywords.

**Experience — before/after per bullet.** Each rewrite ships with a strategy paragraph explaining what changed (weak verb → strong verb, added number, reframed clerical work as process improvement) and why.

**Skills — add/remove/pin.** Ten skills to add in priority order for the target goal, generic soft skills to remove, top 3 to pin. Skills recommendations shift based on the target goal (job vs clients vs thought leadership vs network).

---

## Part 3 · Before/After Scorecard

Compact grid showing each section's before/after score with delta, plus an overall row highlighted with a gradient border. Estimated after-scores are calibrated to what applying every rebuild recommendation would produce.

---

## Part 4 · 7-Day Activation Plan

Actionable, day-by-day. Not just profile edits.

- **Day 1** — Apply every rebuild with an 8-item checklist.
- **Day 2** — Publish Post #1 ("I just rebuilt my LinkedIn"), full draft included, under 1300 characters, with a comment hook.
- **Day 3** — 10 strategic connection requests with a personalized template under 300 characters.
- **Day 4** — Comment on 5 posts using the Value Comment formula (Agree + Insight + Question).
- **Day 5** — Publish Post #2 (lessons-learned), full draft included.
- **Day 6** — Engage with every comment + 5 more connections.
- **Day 7** — Review numbers with target benchmarks and adjustment guidance.

Every drafted post and connection message has a copy button.

---

## Part 5 · Shareable Summary Card

A gradient card designed to be screenshotted and posted on LinkedIn directly. Shows: Before/After score, top 3 mistakes, the #1 thing that made the biggest difference. Ready as-is.

---

## Dual engine architecture

**Rule-based (default).** 20+ pattern checks in vanilla JS. No network. Works entirely offline. Instant. Ships with the sample profile pre-loaded so the tool demos immediately.

**Claude API (optional).** Header toggle switches to Claude Sonnet 4.5. User pastes their Anthropic API key (localStorage only, never leaves the browser). System prompt is a full LinkedIn Optimization Consultant persona with a strict JSON schema for the response. Same 5-part output structure, but richer and more specific to the user's actual profile.

Both engines return the same interface shape, so the UI code is identical for both.

---

## Tech notes

- **Single HTML file.** ~90 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no external assets.
- **Client-side API calls** for the Claude engine via `anthropic-dangerous-direct-browser-access: true` header. Same pattern as Day 40.
- **localStorage persistence** — all input fields save on every keystroke. The API key saves separately. Reload restores state.
- **Rule engine** — 5 analyzer functions, each with its own scoring heuristics. 40+ buzzword blacklist. Weak verb / strong verb dictionaries. Number regex.
- **Templates for rebuilds** — headline options, About rewrite, per-bullet strategies. Generated based on the user's target goal and detected role.
- **Copy-to-clipboard** on every drafted post, connection message, and rebuild option.
- **Dark mode default + light mode toggle** via CSS custom properties.
- **Print-ready CSS** — full report prints as a legitimate handout.
- **`prefers-reduced-motion`** respected.

---

## What this exercise demonstrates

### 1. A "roast" is a strong pedagogical device because it forces specificity

Generic advice ("improve your headline") is invisible. Specific criticism ("You wrote 'Passionate MBA candidate.' Recruiters read that as 'no real projects yet'") is unmissable. The tool's power is in quoting the user's own words back at them.

### 2. Rule-based + LLM together is more useful than either alone

Rule-based catches the objective, quantifiable problems (buzzword count, missing numbers, weak verbs) that don't require judgment. Claude handles the nuanced synthesis (which of 3 headline options fits your specific situation). Neither alone gives you what both together do.

### 3. Templated rebuilds beat generic advice

The tool doesn't say "make your bullets stronger." It shows the exact rewrite: "Responsible for supporting operations" → "Owned intake for the operations team's ad-hoc data requests — averaged ~15 requests/week, cut turnaround from 3 days to 4 hours." A recruiter can compare before/after in 2 seconds. That's what makes the rebuild credible.

### 4. The 7-day plan is the actual value

A rewritten profile that never gets seen doesn't get you a job. The activation plan is what turns the rewrite into interviews. Post drafts included so the user doesn't have to write them.

---

## Narrative continuity

- **Day 40** — AI Assistant Builder (Claude API + BYOK pattern)
- **Day 41** — Interactive Learning Studio (progressive gating)
- **Day 42** — Personal Financial Command Center (live composite score)
- **Day 43** — AI Workflow Architect (7-stage workflow map)
- **Day 44 — LinkedIn Roast & Rebuild**

Personal-productivity arc. Day 42 optimized your money. Day 43 optimized your analytical workflow. Day 44 optimizes the thing that decides whether either of those matter — your professional visibility.

---

## Tags

`#60DayClaudeChallenge` `#LinkedIn` `#PersonalBranding` `#JobSearch`
