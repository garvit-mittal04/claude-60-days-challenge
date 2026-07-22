# Day 52 — System Design: The Technical Blueprint

**Project:** Kickoff — analysis brief tool for analysts and ops pros
**Challenge:** #60DayClaudeChallenge — Day 52 · Capstone Day 2 of 10
**Repo pushed:** [`garvit-mittal04/kickoff` — commit 2ec394a](https://github.com/garvit-mittal04/kickoff/commit/2ec394af73dfb5d86bdaa113d26ab6beef038a3e)
**Deliverables:** 5 markdown design docs
**Ship target:** Day 60

---

## What today was

Day 2 of the capstone. Still no production code — that starts tomorrow. Today converted yesterday's PRD + Implementation Blueprint into a complete technical spec that makes tomorrow's build unambiguous.

The Kickoff GitHub repository was created, the project folder structure was scaffolded, and five design documents were written and committed. Everything a fresh AI conversation would need to continue building on Day 53 now exists in `docs/`.

---

## The five design documents

### 1. `ARCHITECTURE.md`

Final tech stack (Cloudflare Workers + Cloudflare KV + Cloudflare Pages + Anthropic Messages API), Mermaid component diagram, sequence diagram for end-to-end brief creation, request lifecycle detail per endpoint, AI interaction detail for both hand-tuned prompts (decomposition + brief generation), external services and cost breakdown, deviations flagged from the PRD (two minor refinements — server-side session storage and 8-char slugs).

### 2. `SCHEMA.md`

Three Cloudflare KV namespaces: `KICKOFF_BRIEFS` (permanent brief storage), `KICKOFF_SESSIONS` (1-hour interview state), `KICKOFF_RATELIMIT` (SHA-256-hashed IP counters). Full TypeScript value shapes. Explicit validation against every one of the 8 user stories from yesterday's PRD.

### 3. `API.md`

Five endpoints fully specified: `POST /api/interview`, `POST /api/brief`, `POST /api/save`, `GET /api/brief/:slug`, `GET /api/health`. Each with request/response JSON examples, validation rules, error codes and messages, CORS policy, rate limiting algorithm (10 writes per IP per hour, unlimited reads). Standard error format across all endpoints.

### 4. `UI-WIREFRAMES.md`

Four screens (Landing, Interview modal, Brief view + share, Permalink viewer), full user flow Mermaid diagram, low-fidelity ASCII wireframes for each screen, navigation transitions, empty and error states, accessibility notes, responsive design breakpoints. Explicit non-goals — no dashboard, no settings, no history list.

### 5. `PROJECT-STRUCTURE.md`

Complete folder layout with responsibility per folder. Where future code will live, mapped to which capstone day builds it. Why the structure is the smallest that works (no `src/`, no `components/`, no premature abstraction). What's explicitly NOT in the structure and why.

---

## The architectural stance

Three decisions worth calling out.

**No database, only KV.** Every access pattern is a single-key lookup or write. Cloudflare KV covers it. No Postgres, no Supabase, no schema migrations. Removes an entire deployment layer and its failure modes.

**No auth in v1.0.** No accounts, no signup, no login. IP-based rate limiting is the only gate. The trust model is "public permalink, treat like a Pastebin." This kills weeks of scope and makes distribution frictionless.

**Zero build step for the frontend.** `public/index.html` is a real HTML file. Open it in a browser locally, works. No webpack, no Vite, no bundler. Same architecture as the 50 prior single-file HTML builds in this challenge.

---

## Day 3 readiness check

- **Blueprint realistic?** ✅ Day 53 (backend proxy build) is the highest-risk milestone but the doc breaks it into 6 clearly-timed sub-tasks. Achievable in one focused ~5-hour session.
- **Scope creep?** ✅ None. Every doc explicitly reaffirms the PRD §9.2 non-goals. `docs/v2-parking-lot.md` exists specifically to catch and defer feature requests during beta.
- **Ready for Day 53?** ✅ Yes. The API contract is fully specified. Both Anthropic prompts are architecturally described in ARCHITECTURE.md §5, with canonical text going into `design/prompts.md` next.
- **Blueprint update needed?** ❌ No. The two refinements in ARCHITECTURE.md §7 (server-side session storage, 8-char slugs) are compatible improvements — they don't change any Day 53-60 milestone. Original Blueprint stays as-is.

---

## What I learned today

### 1. A spec is a scope-defense mechanism

The temptation on Day 2 was to sketch the tech decisions and start coding. Writing every endpoint's request/response shape, every KV value, every screen's error state took roughly 4 hours. That 4 hours will save 4 days of "wait, how does this work again?" moments across Days 53-60. Specs are how future-you gets guided by past-you.

### 2. Naming what's NOT there is half the doc

Every design document has a "Not in v1.0" section. Together those sections list the whole product roadmap for v2 and beyond. Writing them out is uncomfortable — it feels like admitting weakness. But every non-goal is a decision that saves a week later.

### 3. The stack chose itself

Once you know the pain (analyst scoping), the audience (analysts + ops pros), the trust model (public permalinks, no PII), and the launch constraint (10 days, real users), Cloudflare Workers + KV + Pages becomes the obvious answer. There's no debate to have. The decision compresses if the constraints are precise enough.

---

## Narrative continuity

- **Day 51** — Product discovery + PRD + Implementation Blueprint + Pitch Deck
- **Day 52 — System design + 5 technical documents committed to kickoff repo**
- **Day 53** — Backend proxy build (Cloudflare Workers)
- **Day 54** — Landing page + Interview UI (static shell)
- **Day 55** — Interview logic + live brief generation
- **Day 56** — Persistence + permalinks
- **Day 57** — Landing polish + example briefs + analytics
- **Day 58** — Beta testing + edge cases
- **Day 59** — Launch prep + content
- **Day 60** — LAUNCH

Eight build days after today. Every one has an objective, features, files, and a checklist in the Implementation Blueprint.

---

## Tags

`#60DayClaudeChallenge` `#SystemDesign` `#Capstone` `#BuildInPublic`
