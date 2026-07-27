# Day 58 — Release Readiness: Senior QA + Security Audit

**Project:** Kickoff
**Challenge:** #60DayClaudeChallenge — Day 58 · Capstone Day 8 of 10
**Live URL:** https://kickoff-5r0.pages.dev
**Kickoff repo:** [garvit-mittal04/kickoff](https://github.com/garvit-mittal04/kickoff)
**Ship target:** Day 60 (two days)

---

## What today was

Release-readiness audit. Applied a Senior QA Engineer, Senior Software Engineer, Security Reviewer, and Performance Engineer lens across the full codebase from Days 51-57. Found real issues. Fixed the critical + high-severity ones. Left the medium ones on a v2 backlog.

Two build days until launch. Today made the product actually safe to launch.

---

## The audit findings

### 🔴 CRITICAL (fixed)

**BUG-1 — slugHint allowed seed brief overwrite.** `POST /api/save` accepted `slugHint: "retention"` without a collision check and would happily overwrite the seeded retention example brief. Attack vector: anyone could destroy `/b/retention`, `/b/revenue`, or `/b/launch` with a single curl request.

**Fix:** save.ts now checks for existing slugs when a slugHint is provided. Returns `409 Conflict` with error code `slug_taken` if the slug already exists. Also added a reserved-slug list (`admin`, `api`, `health`, etc.) to block claim of infrastructure paths.

**BUG-2 — No structural validation on generated briefs.** If Llama returned a short refusal or malformed output, we'd save 60 chars of nothing as a "brief" and give the user a broken permalink.

**Fix:** brief.ts now rejects responses under 300 characters (likely refusals). Additionally, save.ts requires briefMarkdown to be at least 200 chars AND contain at least one `##` header. Both layers of validation.

### 🟡 HIGH (fixed)

**UX-1 — Raw HTTP codes leaked into user-facing errors.** Users saw "Server returned 429" instead of "You've made a lot of requests, wait a minute." Bad first impression, especially for a launch.

**Fix:** Added an `ERROR_MESSAGES` mapping in the frontend for 400 / 404 / 409 / 429 / 500 / 502 / 503 / 504 / timeout / aborted / offline. Every network error now surfaces a human-readable message.

**UX-2 — No timeout on fetches.** If the AI hung 60+ seconds, the user had no way to cancel. The tab would just spin.

**Fix:** `fetchWithTimeout()` wrapper with `AbortController`. Default 30s ceiling. 45s for the brief-generation call (the biggest AI call). Storing the active `AbortController` in state so the user can cancel mid-request.

**UX-3 — No cancel button during "Generating…"** Users were locked in once they submitted the last clarifier.

**Fix:** Cancel button added to the generating view. Also wired Escape key to cancel. Cancelling shows "You cancelled. You can retry any time." instead of a scary error.

**UX-4 — Markdown renderer didn't handle links.** Briefs often reference URLs to internal docs, dashboards, or Confluence pages. Rendered as plain text with no click affordance.

**Fix:** Extended the `inline()` markdown renderer with `[text](url)` support. Links open in a new tab with `rel="noopener nofollow"` for security.

### 🟢 MEDIUM (deferred to v2 or post-launch)

- Rate limiting on `/api/save` at the Worker layer — Cloudflare's built-in DDoS handles the top of the funnel; will add per-IP rate limits if abuse shows up post-launch
- Automated tests (Vitest for Worker, Playwright for UI) — manual testing is enough for v1.0
- Prompt injection defenses on user input — low risk since briefs are public anyway
- Analytics enable (Cloudflare Web Analytics) — nice-to-have, will add during Day 59 or after launch

### 🟢 SMALLER POLISH SHIPPED

- `maxlength="5000"` on the answer textarea (soft cap before backend rejects)
- Client-side session save cap (10 briefs per browser via localStorage) — prevents accidental double-saves
- `copyToClipboardSafe()` with fallback for non-secure contexts and older browsers
- Reserved slug list blocks `admin`, `api`, `health`, etc. from being claimed
- Better spacing on the kbd hint (`⌘+Enter to submit`) on narrow viewports

---

## Verification

Both critical fixes verified via curl:

**Test 1 — length validation:**
```
POST /api/save {"slugHint":"retention", "briefMarkdown":"<187 chars>"}
→ 400 "briefMarkdown too short — briefs must be at least 200 characters."
```

**Test 2 — slug collision protection:**
```
POST /api/save {"slugHint":"retention", "briefMarkdown":"<300+ chars with ##>"}
→ 409 "That slug already exists. Omit slugHint to get an auto-generated one."
```

Both layers fired correctly. The seed briefs `/b/retention`, `/b/revenue`, `/b/launch` are now protected.

Frontend hardening verified in browser: cancel button visible during "Generating…", friendly error messages when triggering 4xx/5xx responses, markdown links render as clickable elements.

---

## Files changed

**Backend (`api/`):**
- `src/endpoints/save.ts` — slug collision check + reserved list + structural validation
- `src/endpoints/brief.ts` — reject too-short AI responses (< 300 chars)

**Frontend (`public/`):**
- `index.html` — error mapping, fetchWithTimeout, cancel button, markdown link support, session save cap, safer clipboard, header updated to "day 58 of 60"

Deployed both:
- Worker: `348588b7` — 15.79 KiB upload
- Pages: 1 file uploaded, live

---

## What I learned today

### 1. The bugs you find during a real audit are almost never the ones you expected

Going in, I assumed the main risk was AI-side stuff — prompt injection, weird outputs, hallucinations. The real bug was in the save endpoint: a code path (slugHint provided) skipped the collision check that the other code path (auto-generate slug) enforced. A subtle asymmetry between two branches of the same function. Boring, dangerous, easy to miss without a deliberate audit.

### 2. Small validation layers compound

The 187-char test brief hit the min-length check before the slug-collision check ever ran. Two hardening layers, one attack blocked at each. You almost never need one perfect defense — you need three okay defenses that reject different failure modes.

### 3. Error messages are product decisions, not tech decisions

Changing "Server returned 429" to "You've made a lot of requests — wait a minute and try again" took 30 seconds. Impact on the user experience is disproportionate to the effort. A launch-ready product's error messages are as considered as its happy path.

---

## Day 58 status

- ✅ Full senior-engineer audit performed across Days 51-57 code
- ✅ 2 critical bugs fixed and verified in production
- ✅ 4 high-severity UX issues fixed
- ✅ 5+ smaller polish items shipped
- ✅ Worker + Pages redeployed
- ✅ Curl-verified: length validation returns 400, slug collision returns 409
- ✅ Browser-verified: cancel button, error messages, markdown links work

---

## Narrative continuity

- **Day 51** — Product discovery + PRD + Blueprint + Pitch Deck
- **Day 52** — System design (5 technical docs)
- **Day 53** — Project setup + Hello World Worker
- **Day 54** — Full frontend UI shell + deployed
- **Day 55** — Live AI wired in
- **Day 56** — MVP complete: real permalinks + KV
- **Day 57** — Product refinement + UX pass
- **Day 58 — Release readiness + senior QA audit**
- **Day 59** — Launch prep (LinkedIn post, Reddit posts, outreach templates)
- **Day 60** — LAUNCH

Two build days to launch. The product is now stable enough to ship.

---

## Tags

`#60DayClaudeChallenge` `#Capstone` `#QAEngineering` `#BuildInPublic`
