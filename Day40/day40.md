# Day 40 — AI Assistant Builder

**Project:** `ai-assistant-builder.html` — a four-in-one AI assistant hub that calls Claude directly from the browser
**Challenge:** #60DayClaudeChallenge — Day 40 · Two-thirds through the challenge
**Type:** Single-file HTML/CSS/vanilla JavaScript. No frameworks, no external libraries. Calls `https://api.anthropic.com/v1/messages` directly with your API key.

---

## What I built

A single-page application that ships four purpose-built AI assistants in one interface. Each assistant has a hand-crafted system prompt tuned to its domain, structured markdown output, and its own input/context labels. Users bring their own Claude API key (stored only in browser localStorage), pick an assistant, paste their input, and get a report back within seconds — no server, no proxy, no data collection.

The whole app is ~55 KB of a single HTML file, plus one live network call: `POST /v1/messages`.

---

## The four assistants

Each is a Claude Sonnet 4.5 call with a purpose-built system prompt. Users toggle between them with a single click.

**📄 Resume & Career Coach.** Paste your resume (and optionally a target JD). Get back a match score, top 3 strengths, top 3 gaps, 3–5 specific rewrite suggestions (old bullet → new bullet + why), missing keywords in a table, and next steps.

**📊 Business Case Analyzer.** Paste a scenario or memo. Get back a Situation restatement, a SWOT table, a positioning read, a recommendation with defense, and a "what I would want to know" section for the missing context.

**📈 Data Analyst Assistant.** Ask a data question. Get back a commented SQL query (or an analysis plan for open-ended questions), every schema assumption named explicitly, an interpretation of what the result would tell you, and follow-up questions worth asking.

**🎯 Interview Prep Coach.** Paste a STAR answer for evaluation or ask for a mock question. If evaluating: a per-STAR breakdown, a 1–5 rating with justification, the single most important edit, and a rewritten version. If generating: the question, what it's actually testing, hooks a strong answer would hit, and the common trap.

---

## The system prompt architecture

Every system prompt in this app follows the same four-part structure:

**1. Role.** Anchors the model in a specific senior-practitioner voice. "You are a senior data analyst who has shipped analysis at product companies like Airbnb, Stripe, and Notion" produces different output than "You are a data analyst." The specificity of the role narrows the model's variance dramatically.

**2. Constraints.** Names the things the model must NOT do. Fabricate metrics. Invent experience. Give generic praise. Recommend illegal or clearly self-destructive actions. Constraints do more work in a good system prompt than instructions do — they define the guardrails within which the model can be creative.

**3. Output format.** Explicit markdown structure with named sections. Every assistant returns the same shape of report, so downstream UI treatment is predictable and users know what they're getting before they click Run.

**4. Edge cases.** Named failure modes ("if the input is one sentence…", "if they didn't give a schema…", "if the answer is contradictory…") so the model has a specific "what to do" for each. This is where the difference between a demo-quality assistant and a production one lives.

---

## Client-side API calls (the interesting engineering decision)

The Claude API defaults to blocking browser-origin calls to prevent API keys from leaking through client-side code. The way to opt in — and this is documented on Anthropic's side — is to pass the header `anthropic-dangerous-direct-browser-access: true`. That header is exactly what it sounds like: a promise from the developer that they understand what direct browser calls imply.

For this app, that trade-off is worth it. The user brings their own key. The key is stored only in `localStorage` on their device. Nothing ever routes through a server I control. There is no server at all. The privacy model is identical to Day 39's PDF tool: what's structurally impossible to leak can never leak.

The alternative — proxying calls through a Cloudflare Worker or similar — would give me stronger key-hiding but at the cost of trusting me with the key. For a personal, single-user tool, the client-side pattern is the right one.

---

## The documentation panel

Per the task spec, there's a collapsible "How this was built" panel at the bottom of every assistant's view. It shows:

- **The actual system prompt** for the currently selected assistant, verbatim, in a scrollable code block. Users can copy it, study it, adapt it.
- **The four-part system prompt architecture** (Role / Constraints / Output format / Edge cases) with explanations.
- **UI design decisions** — why tabs vs. dropdown, why free-text vs. form fields, why client-side calls.
- **Future extensions** — tool use, memory, multi-step workflows chaining assistants, streaming responses, prompt versioning.

The panel is closed by default so it doesn't crowd the primary interface, but one click reveals the whole design rationale. This is the difference between an app and a teaching tool.

---

## Tech notes

- **Single HTML file.** ~55 KB. Vanilla CSS + JavaScript. No frameworks, no CDN libraries, no build step.
- **Client-side API calls** via `fetch` to `https://api.anthropic.com/v1/messages` with `anthropic-dangerous-direct-browser-access: true`.
- **Model:** `claude-sonnet-4-5` — current highest-quality model for this class of task.
- **Custom inline Markdown renderer** — 60 lines of vanilla JS handling headers, lists, code blocks, tables, blockquotes, bold, italic, inline code. Chose this over CDN Marked because the response format is well-controlled and a minimal renderer is enough.
- **localStorage** for the API key (`aab.apiKey`) — nothing else persisted.
- **Loading state** with animated spinner and pulsing "Analyzing input…" text.
- **Error state** renders API errors inline in the output panel with the full server message.
- **Empty state** invites the user to fill in the input and click Run.
- **Dark mode default** + light mode toggle via CSS custom properties.
- **`prefers-reduced-motion`** respected.
- **Responsive layout** — two-column above 900px, stacked below.
- **Keyboard-friendly** — Enter on the API key input saves it, standard form focus flows work.
- **Copy-to-clipboard** on completed responses.

---

## What this exercise demonstrates

### 1. The system prompt is the product

Four assistants share the same UI, the same API, the same model. The only thing that differs between them is the system prompt. That's where the entire product experience lives. When users say "this AI feels different from that AI," what they're actually noticing is different system prompts.

### 2. Constraints do more work than instructions

The Resume assistant's system prompt says "Never invent metrics, experience, or credentials the resume doesn't already contain." That single constraint prevents 90% of the failure mode where AI resume tools hallucinate impressive-sounding fake achievements. A positive instruction ("be accurate") wouldn't work — models need to be told what NOT to do, not just what to do.

### 3. Explicit output format eliminates variance

Every assistant tells the model exactly which markdown sections to return, in which order. Users get consistent, scannable reports every time — not one time a bulleted list and next time a paragraph. That predictability is what makes the app feel professional instead of feeling like a chat with a language model.

### 4. Client-side is the honest architecture for personal AI tools

If the user brings their own key, there's no reason for a server to be in the middle. The additional infrastructure would only exist to justify itself. The client-side architecture is simpler, faster, cheaper, and more private.

---

## Narrative continuity

- **Day 35** — prompt puzzle
- **Day 36** — cognitive pattern explorer
- **Day 37** — task compass
- **Day 38** — typing speed studio
- **Day 39** — PDF splitter & merger
- **Day 40 — AI assistant builder**

Two-thirds through the challenge. Day 35 taught prompting as a skill. Day 40 uses that skill to build purpose-built assistants. The arc closes: prompt engineering isn't just about writing individual prompts — it's about designing repeatable assistants where the prompt IS the product.

---

## Tags

`#60DayClaudeChallenge` `#AI` `#ClaudeAPI` `#PromptEngineering`
