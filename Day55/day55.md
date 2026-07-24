# Day 55 — Live AI Wired In

**Project:** Kickoff
**Challenge:** #60DayClaudeChallenge — Day 55 · Capstone Day 5 of 10
**Live URL:** https://kickoff-5r0.pages.dev
**Worker URL:** https://kickoff-api.garvitm534.workers.dev
**Kickoff repo:** [garvit-mittal04/kickoff](https://github.com/garvit-mittal04/kickoff)
**Ship target:** Day 60

---

## What today was

The AI wired in. Yesterday the interview modal cycled through 5 hardcoded questions and generated a canned brief. Today the interview asks real AI-generated clarifying questions and produces a real AI-generated brief from the analyst's actual answers.

End-to-end, hands-on-keyboard, live at `kickoff-5r0.pages.dev`. Zero API keys required. Zero cost to users.

---

## The pivot: Anthropic → Cloudflare Workers AI

The original PRD (Day 51) specified Anthropic Messages API with `ANTHROPIC_API_KEY` as a Worker secret. That would have cost real money per request — every one of the 50+ target briefs by Day 60 would burn credits. Not viable for a "no signup, no API key, zero friction" v1.0.

The pivot: **Cloudflare Workers AI** with `@cf/meta/llama-3.2-3b-instruct`. Free tier. 10,000 neurons/day. Native to the CF Workers runtime — literally one line in `wrangler.toml`:

```toml
[ai]
binding = "AI"
```

Then in the Worker: `env.AI.run(MODEL, { messages })`. Zero auth flow. Zero cross-service complexity. Zero cost.

The tradeoff is model size. Llama 3.2 3B is much smaller than Claude Sonnet 4.5. Reasoning quality is objectively lower. But for scoping tasks — decomposing an exec question into sub-questions and hypotheses — 3B is enough. Early testing today produced briefs that reference specific Salesforce tables (`program_participant`, `program_coordinator`), rank hypotheses correctly, and estimate reasonable effort. Not perfect, but good enough for v1.0 launch on Day 60.

If quality complaints show up in beta (Day 58), the fix is a one-line change to swap to a bigger free model or add a paid tier post-launch. Model is decoupled — never a rebuild.

---

## What actually shipped

### Backend (deployed to `https://kickoff-api.garvitm534.workers.dev`)

- `wrangler.toml` — added `[ai]` binding, updated compatibility date
- `src/index.ts` — router with 4 routes: `GET /`, `GET /api/health`, `POST /api/interview`, `POST /api/brief`
- `src/endpoints/interview.ts` — adaptive Q&A. Sends full transcript to Llama with the decomposition prompt. Returns next clarifier or `isReady: true` signal
- `src/endpoints/brief.ts` — final brief generation. Sends transcript + brief-generation prompt. Returns markdown
- `src/lib/ai.ts` — wrapper around `env.AI.run()`. Model swap is one constant change
- `src/lib/prompts.ts` — canonical decomposition + brief-generation prompts, tuned for Llama 3.2 3B
- `src/lib/validation.ts` — request body validation (originalQuestion length, transcript shape)

Deployed with `npx wrangler deploy`. 10.94 KiB uploaded. Live in ~4 seconds.

### Frontend (deployed to Cloudflare Pages)

- `public/index.html` — full rewrite of the interview flow
- Removed all `MOCK_QUESTIONS` and `setTimeout` mocks
- Added `API_BASE` constant pointing at production Worker
- Interview flow now:
  1. First submit = original exec question. Stored, no clarifier shown yet.
  2. `fetchNextClarifier()` POSTs to `/api/interview` with full transcript.
  3. Response either has a clarifier (loop back) or `isReady: true` (go generate brief).
  4. `generateBrief()` POSTs to `/api/brief` with same transcript. Returns markdown.
  5. Inline markdown renderer converts to HTML (H2, H3, ul, ol, bold, italic).
- Error handling: network failures, malformed responses, timeouts all surface a red banner in the modal
- Loading states: submit button becomes "Sending…" and disabled during fetch
- Progress bar animates through the clarifier count

Deployed with `npx wrangler pages deploy public --project-name=kickoff`.

---

## The debug story

Three errors hit during Day 55 build. Worth naming:

**Error 1 — `workers.dev subdomain not registered`.** Wrangler needs a `.workers.dev` subdomain for remote AI in dev mode. Fixed by registering `garvitm534.workers.dev` on the Cloudflare dashboard. One-time.

**Error 2 — `1031: model not supported`.** Initial model choice `@cf/meta/llama-3.3-70b-instruct-fp8-fast` requires the paid Workers AI tier. Fell back to `@cf/meta/llama-3.1-8b-instruct` which then hit —

**Error 3 — `5028: model deprecated`.** Llama 3.1 8B was deprecated on 2026-05-30. Fell back to `@cf/meta/llama-3.2-3b-instruct` which works. Newer, smaller, currently on the free tier.

Debug time: ~15 minutes total. The pattern: expose the error message via the response body, retry, adjust. Guessing model names blind doesn't scale — the dashboard's model catalog is the truth.

---

## What I learned today

### 1. Free-tier constraints improve the product

The Anthropic → Workers AI pivot wasn't a downgrade. It made the product BETTER for the target user: real analysts who won't set up an API key just to try a scoping tool. Cost pressure forced the decision that makes the tool actually usable at scale by strangers. Every "free tier only" constraint I hit in the next 5 days will probably do the same.

### 2. Same-platform binding beats cross-service auth

Workers AI is native to Cloudflare Workers. No API key, no OAuth, no bearer tokens — just `env.AI.run()`. That eliminates an entire class of failure modes (expired keys, rotated secrets, wrong region). When your compute and your AI live in the same runtime, integration is a keyword, not a project.

### 3. Ship the graceful fallback FIRST

The first version of `interview.ts` had `try { callAI() } catch { returnFallback() }` from the start. When the AI failed silently on the first curl test, the tool didn't crash — it just used a canned question and returned 200 OK. That let me diagnose calmly instead of debugging a 500 error under pressure. The debug pivot (temporarily surfacing the error) came second, and it was safer because the fallback still worked in production the whole time.

---

## Day 55 status

- ✅ Workers AI binding added, `@cf/meta/llama-3.2-3b-instruct` selected as free-tier model
- ✅ Backend endpoints `/api/interview` + `/api/brief` deployed to production
- ✅ Frontend wired to real API, redeployed to Cloudflare Pages
- ✅ End-to-end live test — real 5-clarifier interview → real generated brief
- 🔜 KV namespaces + real permalink persistence (Day 56)

---

## Narrative continuity

- **Day 51** — Product discovery + PRD + Blueprint + Pitch Deck
- **Day 52** — System design (5 technical docs)
- **Day 53** — Project setup + Hello World Worker
- **Day 54** — Full frontend UI as static shell + deployed
- **Day 55 — Live AI wired in end to end**
- **Day 56** — Real permalinks (KV storage for briefs)
- **Day 57** — Landing polish + example briefs + analytics
- **Day 58** — Beta testing
- **Day 59** — Launch prep
- **Day 60** — LAUNCH

Five build days until launch. The core product now works.

---

## Tags

`#60DayClaudeChallenge` `#Capstone` `#CloudflareWorkersAI` `#BuildInPublic`
