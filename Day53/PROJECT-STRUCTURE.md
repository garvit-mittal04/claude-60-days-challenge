# Kickoff — Project Folder Structure

**Version:** 1.1 · Day 53 (updated to reflect real code that landed today)
**Repo:** `garvit-mittal04/kickoff`
**Change log:** v1.0 (Day 52) → v1.1 (Day 53): `api/` populated with actual Worker code. `public/` populated with placeholder landing. Small structural refinements.

---

## 1. Top-level structure

```
kickoff/
├── README.md                    # Project overview + quickstart
├── LICENSE                      # MIT
├── .gitignore                   # Node + Wrangler + editor + secrets
│
├── public/                      # Static frontend (Cloudflare Pages target)
│   └── index.html               # ✅ Day 53: placeholder "coming Day 60" landing
│                                # 🔜 Day 54: full landing + interview modal
│
├── api/                         # Cloudflare Workers backend
│   ├── package.json             # ✅ Day 53: created
│   ├── package-lock.json        # ✅ Day 53: dependency lockfile
│   ├── node_modules/            # ✅ Day 53: (gitignored) installed dev deps
│   ├── wrangler.toml            # ✅ Day 53: Worker config (KV bindings pending)
│   ├── tsconfig.json            # ✅ Day 53: TypeScript config for Workers
│   └── src/
│       ├── index.ts             # ✅ Day 53: entry point with /api/health + /
│       ├── endpoints/           # 🔜 Day 55: interview.ts, brief.ts, save.ts, retrieve.ts
│       ├── lib/                 # 🔜 Day 55: anthropic.ts, ratelimit.ts, validation.ts, prompts.ts
│       └── types.ts             # 🔜 Day 55: shared TypeScript types
│
├── design/                      # Design decisions + assets
│   ├── prompts.md               # 🔜 Day 53 evening: canonical Claude prompts
│   ├── wireframes/              # 🔜 Day 53 evening: wireframe PNGs
│   └── design-tokens.md         # 🔜 Day 54: colors, fonts, spacing scale
│
├── docs/                        # Living technical documentation
│   ├── ARCHITECTURE.md          # ✅ Day 52
│   ├── SCHEMA.md                # ✅ Day 52
│   ├── API.md                   # ✅ Day 52
│   ├── UI-WIREFRAMES.md         # ✅ Day 52
│   ├── PROJECT-STRUCTURE.md     # ✅ Day 52 (this file, now v1.1)
│   ├── SETUP.md                 # ✅ Day 53: reproducible install guide
│   ├── ENVIRONMENT.md           # ✅ Day 53: tools + env vars + secrets reference
│   ├── DAY3-SUMMARY.md          # ✅ Day 53: end-of-day summary
│   ├── v2-parking-lot.md        # 🔜 Ongoing: deferred features
│   └── beta-log.md              # 🔜 Day 58: beta feedback tracker
│
└── launch/                      # Launch-day assets (Days 59-60)
    └── (empty until Day 59)
```

**Legend:**
- ✅ = created / populated today (Day 53) or earlier
- 🔜 = scheduled for a specific future day per the Implementation Blueprint

---

## 2. What each major folder is responsible for

### `public/` — Frontend deployment target (Cloudflare Pages)

Everything Cloudflare Pages will serve as static assets. Currently just the placeholder `index.html`. Days 54-57 progressively fill this out with the real Kickoff app (interview modal, brief view, permalink viewer).

### `api/` — Backend Worker

The Cloudflare Worker that will proxy Anthropic calls, enforce rate limits, and read/write KV. Currently has:
- `src/index.ts` with the working `/api/health` and `/` endpoints (Hello World milestone)
- `wrangler.toml` scaffolded (KV bindings commented out — added Day 55)
- `tsconfig.json` targeting the Workers runtime
- `package.json` with Wrangler + TypeScript + Cloudflare types as dev deps

Days 55-56 will add the real endpoint files under `src/endpoints/` and helper libraries under `src/lib/`.

### `design/`, `docs/`, `launch/`

Unchanged from Day 52. See v1.0 for full descriptions.

---

## 3. What's new since Day 52

### Added

- `api/package.json`, `api/package-lock.json`, `api/node_modules/` (gitignored)
- `api/wrangler.toml` (basic scaffold with commented KV bindings)
- `api/tsconfig.json` (Workers-compatible TypeScript config)
- `api/src/index.ts` (entry point with 3 routes: `/`, `/api/health`, and a 404 handler)
- `public/index.html` (placeholder landing page)
- `docs/SETUP.md` (this reproducible install guide)
- `docs/ENVIRONMENT.md` (tools + env vars + secrets reference)
- `docs/DAY3-SUMMARY.md` (end-of-day report)

### Unchanged from Day 52

- `docs/ARCHITECTURE.md`, `docs/SCHEMA.md`, `docs/API.md`, `docs/UI-WIREFRAMES.md`
- `LICENSE`, `README.md`, `.gitignore`

### Deferred to later days (was originally in Blueprint Day 53)

Two items originally slotted for Day 53 in the Implementation Blueprint have been moved to **Day 55 pre-work** to respect the course-mandated "no core features yet" rule for Day 53:

- Cloudflare KV namespace creation (`wrangler kv namespace create`)
- `wrangler deploy` to production

These are foundational infrastructure but touch the "real production" surface. Deferring them keeps Day 53 focused on local dev environment integrity. The Blueprint update note appears in Day 55's section.

---

## 4. Where future code will live (updated)

| What | Where | When |
|---|---|---|
| Landing page + interview modal + brief view (full app) | `public/index.html` | Day 54 (built) → refined Days 55, 57 |
| Endpoint handlers | `api/src/endpoints/{interview,brief,save,retrieve}.ts` | Days 55-56 |
| Anthropic API wrapper | `api/src/lib/anthropic.ts` | Day 55 |
| Rate limiter | `api/src/lib/ratelimit.ts` | Day 55 |
| Validation helpers | `api/src/lib/validation.ts` | Day 55 |
| Prompt constants (imported from `design/prompts.md`) | `api/src/lib/prompts.ts` | Day 55 |
| Shared types | `api/src/types.ts` | Day 55 |
| KV namespaces on Cloudflare + `wrangler.toml` bindings | (deployed) | Day 55 |
| Anthropic API key as Worker secret | (deployed) | Day 55 |
| Production Worker deployment | (deployed) | Day 55 |
| Cloudflare Pages deployment | (deployed) | Day 57 |

---

## 5. `.gitignore` — what stays out of the repo

Current `.gitignore` (already at repo root as of Day 52):

```
node_modules/
.pnp
.pnp.js
coverage/
.wrangler/
.dev.vars
wrangler.toml.bak
.env
.env.local
.env.*.local
dist/
build/
.next/
.output/
.nuxt/
.vscode/
.idea/
*.swp
*.swo
.DS_Store
npm-debug.log*
yarn-debug.log*
yarn-error.log*
Thumbs.db
```

The critical entries for Day 53 are `node_modules/` (100+ MB of installed deps we don't want to track) and `.dev.vars` / `.env*` (secrets like the Anthropic API key must never be committed).

---

*End of PROJECT-STRUCTURE.md v1.1 — Day 53 of 60*
