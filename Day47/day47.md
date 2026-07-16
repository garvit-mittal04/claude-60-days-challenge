# Day 47 — Content Intelligence Studio

**Project:** `content-intelligence-studio.html` — a live multi-reviewer content audit that analyzes text OR images via Claude Sonnet 4.5
**Challenge:** #60DayClaudeChallenge — Day 47
**Content type selected:** Blog article / long-form (with runtime platform switching)
**Type:** Single-file HTML/CSS/JavaScript. No frameworks, no CDN. Client-side API calls (bring your own key).

---

## What I built

A commercial-grade content audit tool that takes your text or image and runs it through **six specialized AI reviewers in a single multi-stage pipeline**. Every score, critique, rewrite, and recommendation is a live API call. No hardcoded scoring, no canned feedback, no rule-based logic — the whole review is Claude, six ways.

The first five reviewers run **in parallel** for speed. The Final Synthesizer then reads all five outputs and produces the executive summary, rewritten version, before/after comparison, and further prompts.

---

## The four-question intro

Per the spec, I asked four MCQ-format questions before generating:

1. **Content type** → Blog article / long-form
2. **Platform** → All (user picks per-session via header dropdown)
3. **Primary goal** → All (user picks per-session)
4. **Criticality** → All (user picks per-session)

Because the user picked "all" for three of the four, I built the app with **runtime selectors** for platform, goal, and criticality. One tool, all combinations.

---

## The six reviewers

**🎯 Content Strategist** — Analyzes structural strength: thesis clarity, hook, argument arc, CTA effectiveness. Returns markdown with sections for overall read, strongest element, structural weakness, missed opportunity, and specific rewrite direction.

**✏️ Line Editor** — Sentence-level critique. Tightness, weak verbs, redundancy, corporate speak, buzzwords, passive voice. Quotes the user's own words back at them when calling out issues. Ships three specific "Before → After" rewrites.

**🧠 Reader Journey Analyst** — Reader experience over writer intent. Attention curve, first-3-seconds hook test, cognitive load, retention fix.

**🔍 SEO & Discovery Specialist** — Findability + shareability. Missing search anchors, three alternative titles, share hook.

**📢 Platform Specialist** — Platform-specific optimization. Format adjustments, CTA recommendation, publishing checklist. Adapts everything to whatever platform the user selects in the header dropdown.

**🎁 Final Synthesizer** — Runs LAST, sees all five prior reports. Produces executive summary, five-line health report, top-3 highest-impact improvements, predicted performance (labeled as AI ESTIMATE across three scenarios), full rewritten version, before-vs-after table, and 5 further prompts for continued optimization.

Each reviewer has its own hand-written system prompt tuned to its specific role.

---

## The critical design choice: markdown, not JSON

The prompt explicitly said: *"Do not expect json format anywhere in order to avoid errors like 'expected `{` or `(`'"*.

So the entire pipeline returns **free-form markdown**. Each reviewer's system prompt asks for a `**SCORE: N/100**` line at the top (extracted with a tolerant regex), followed by structured markdown sections. The rest is rendered as-is by an inline markdown renderer.

This eliminates the class of failures that plagued Day 46 — no `JSON.parse()` on model output, no schema mismatches, no `SyntaxError: Unexpected token`. The tradeoff is that structured data (like the score) has to be recovered via regex, which is less precise. In practice the score line is easy to detect and the rest of the value is in the rendered markdown anyway.

---

## Live dashboard

Real-time updates as the pipeline runs:

- **Overall content score** — big gradient number appears once all reviewers finish
- **Reviewer status grid** — one card per reviewer with icon, name, pending/running/done status, and score when available. Spinner while running.
- **Activity log** — auto-scrolling feed with timestamp, reviewer name (color-coded), and status messages
- **Reviewer reports** — full markdown critique from each reviewer as it completes
- **Execution stats** — reviewers completed, word count, duration, model, platform, overall score

The dashboard updates progressively — you see the Strategist's report appear before the Line Editor's, and so on. Parallel execution means all five come in within a few seconds of each other.

---

## Text OR image input

The input has two tabs: **Paste Text** and **Upload Image**. When image mode is selected, drag-and-drop or click-to-upload accepts PNG/JPG/WebP up to ~4 MB. The image is base64-encoded and sent to Claude as a proper multimodal content block:

```json
{
  "type": "image",
  "source": { "type": "base64", "media_type": "image/png", "data": "..." }
}
```

Claude's vision then analyzes the actual visual — thumbnails get roasted for click-worthiness, screenshots of posts get analyzed for hook density, banners get judged on hierarchy.

---

## The full pipeline flow

```
1. User inputs text or uploads image + picks platform/goal/criticality
2. Analysis kicks off
3. Reviewers 1-5 fire in PARALLEL (Strategist, Editor, Journey, SEO, Platform)
4. Each returns markdown with SCORE line at top
5. Once all five complete, Synthesizer receives all outputs as context
6. Synthesizer produces executive report + rewritten version + performance prediction + before/after + further prompts
7. Overall score = average of all 6 reviewer scores
```

Parallel execution for stages 1-5 means the whole analysis completes in ~15-25 seconds instead of ~60 if run sequentially.

---

## Tech notes

- **Single HTML file.** ~55 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no images.
- **Client-side Claude API calls** using `anthropic-dangerous-direct-browser-access: true`.
- **Multimodal input** — text + image, both sent as a proper `content` array with `image` and `text` blocks.
- **Retry logic** — 2 retries with exponential backoff on transient failures.
- **Tolerant score extraction** — regex looks for `**SCORE: N**` or `SCORE: N`, falls back to first number in first 80 chars.
- **Inline markdown renderer** — headers, bold/italic, code, lists, tables, blockquotes, hr, code fences. Safe HTML escaping.
- **localStorage persistence** — API key and input form save across reloads.
- **`prefers-reduced-motion`** respected.
- **Runtime settings switching** — change platform, goal, or criticality anytime and re-run.

---

## What this exercise demonstrates

### 1. Parallel > sequential for independent reviewers

Days 40 and 46 ran agents sequentially. This one runs 5 in parallel because none of them need the others' outputs. Result: 3-4x faster. The Synthesizer waits for the parallel batch, then runs alone. This is the correct topology for a "read the work from N angles" pipeline.

### 2. Markdown responses beat JSON responses when the payload is text-heavy

Day 46 had one reviewer (the Evaluator) return JSON. When it fired malformed output — even once — the loop stalled or scored 0. This day's pipeline has zero JSON parsing. Score extraction happens on a plain string with a regex. Result: no schema errors, no fallback code paths.

### 3. Runtime settings are more useful than baked-in choices

The user picked "all" for three of the four interview questions. The right response wasn't to pick defaults for them — it was to expose the choices as runtime selectors in the header so a single session could re-analyze the same content under different platform / goal / criticality settings.

### 4. Multimodal review is a specific unlock

Being able to hand Claude a screenshot of a LinkedIn post or a YouTube thumbnail and ask "how does this land visually" is a genuinely different tool than a text-only reviewer. The Platform Specialist and Journey Analyst prompts both explicitly consider visual elements when an image is present.

---

## Narrative continuity

- **Day 40** — AI Assistant Builder (single-agent per assistant)
- **Day 44** — LinkedIn Roast & Rebuild (rule-based + Claude hybrid)
- **Day 46** — Autonomous Agent Studio (sequential 6-agent loop with real stop conditions)
- **Day 47 — Content Intelligence Studio (parallel 6-reviewer multimodal audit)**

Multi-agent arc continues. Day 46 showed sequential orchestration with dynamic stop conditions. Day 47 shows parallel orchestration with a final synthesis step. Together they cover the two most common multi-agent topologies in production AI systems.

---

## Tags

`#60DayClaudeChallenge` `#ContentStrategy` `#AI` `#Multimodal`
