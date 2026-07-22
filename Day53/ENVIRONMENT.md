# Kickoff — ENVIRONMENT.md

**Version:** 1.0 · Day 53
**Purpose:** Single reference for every tool, package, environment variable, and secret used by the Kickoff project.

---

## 1. System requirements

| Tool | Version | Purpose | How installed |
|---|---|---|---|
| **macOS** | 12+ | Development OS | Native |
| **Homebrew** | 4.0+ | macOS package manager | `curl` install script |
| **Node.js** | 20+ | JavaScript runtime for Wrangler and TypeScript | `brew install node` |
| **npm** | 10+ | Node package manager (bundled with Node) | Bundled |
| **Wrangler CLI** | 4.113+ | Cloudflare Workers deployment + local dev | `npm install -g wrangler` |
| **Git** | 2.30+ | Version control | Pre-installed on macOS |

---

## 2. Runtime binaries installed by Wrangler

These are downloaded automatically by npm's postinstall scripts (or via `npm install --foreground-scripts` if scripts were blocked):

| Binary | Purpose |
|---|---|
| **workerd** | Cloudflare's V8-based runtime. Simulates the Worker locally when you run `wrangler dev`. |
| **esbuild** | Fast JavaScript bundler. Wrangler uses it to compile TypeScript before serving. |
| **sharp** | Image processing library. Used by Wrangler's dev tools for icon/image handling. |

---

## 3. npm dependencies (per package)

### `api/package.json` — devDependencies

```json
{
  "devDependencies": {
    "@cloudflare/workers-types": "^4.x",
    "typescript": "^5.x",
    "wrangler": "^4.113.x"
  }
}
```

**Why each:**
- `@cloudflare/workers-types` — TypeScript type definitions for the Workers runtime (KVNamespace, ExecutionContext, etc.). Autocomplete + type checking.
- `typescript` — TypeScript compiler. Not strictly required to run (Wrangler bundles TS on the fly) but needed for editor tooling.
- `wrangler` — installed locally so `npm run dev` behaves consistently regardless of global version.

### Root repo — no dependencies

The root repo has no `package.json`. Each deployable unit (api/, later maybe a build tool) has its own.

---

## 4. Environment variables

Kickoff uses **zero client-side environment variables**. Everything sensitive lives on the Worker as a secret.

### Worker environment variables (non-secret)

Declared in `wrangler.toml` under `[vars]`. Not yet used in v1.0.

Reserved for future:
- `LOG_LEVEL` (default: `info`) — controls Worker log verbosity in production
- `ENVIRONMENT` (`dev` / `prod`) — for logic branches

### Worker secrets (encrypted, server-side only)

Set via `wrangler secret put <NAME>`. Never checked into git.

| Secret | Purpose | Set on | Rotation |
|---|---|---|---|
| **`ANTHROPIC_API_KEY`** | Anthropic Messages API key. Used by the Worker to call Claude. Never exposed to browser. | Day 55 (before /api/interview implementation) | Rotate every 90 days |

**How to set:**
```bash
cd ~/Desktop/kickoff/api
npx wrangler secret put ANTHROPIC_API_KEY
# Paste the key when prompted. Wrangler encrypts and uploads to Cloudflare.
```

**How to verify (won't show the value):**
```bash
npx wrangler secret list
```

**How to remove:**
```bash
npx wrangler secret delete ANTHROPIC_API_KEY
```

### Local dev secrets

For local dev, create `api/.dev.vars` (already in `.gitignore`):

```
ANTHROPIC_API_KEY=sk-ant-...
```

Wrangler auto-loads this file when running `wrangler dev`. Do NOT commit it.

---

## 5. Cloudflare account resources

| Resource | Name | Purpose | Created | Cost |
|---|---|---|---|---|
| **Worker** | `kickoff-api` | Backend API (5 endpoints per API.md) | Day 55 (`wrangler deploy`) | Free tier: 100k req/day |
| **KV Namespace** | `KICKOFF_BRIEFS` | Persistent brief storage | Day 55 (`wrangler kv namespace create`) | Free tier: 100k reads/day |
| **KV Namespace** | `KICKOFF_SESSIONS` | In-progress interview state (1hr TTL) | Day 55 | Free tier |
| **KV Namespace** | `KICKOFF_RATELIMIT` | IP-hash-based rate limit counters (1hr TTL) | Day 55 | Free tier |
| **Pages Project** | `kickoff` | Static frontend hosting from `public/` | Day 54 or 55 | Free tier: unlimited bandwidth |
| **Custom domain** | tbd (`kickoff.app` / `trykickoff.com`) | Production URL | Day 55 evening | ~$10/year |

---

## 6. Wrangler configuration reference

### `api/wrangler.toml`

Current state (Day 53):

```toml
name = "kickoff-api"
main = "src/index.ts"
compatibility_date = "2026-07-22"
compatibility_flags = ["nodejs_compat"]
```

**Field meanings:**
- `name` — Worker's slug. Determines its default `*.workers.dev` URL.
- `main` — entry point. TypeScript file that exports the default handler.
- `compatibility_date` — the date whose Workers runtime behavior this project targets. Locks the runtime version so future Cloudflare updates don't silently break things.
- `compatibility_flags` — opt-in feature flags. `nodejs_compat` gives us the Node.js standard library polyfills (`crypto`, `url`, etc.) inside the Worker.

### KV bindings (added Day 55)

Will look like:

```toml
[[kv_namespaces]]
binding = "KICKOFF_BRIEFS"
id = "<32-char hex ID from wrangler kv namespace create>"

[[kv_namespaces]]
binding = "KICKOFF_SESSIONS"
id = "<32-char hex ID>"

[[kv_namespaces]]
binding = "KICKOFF_RATELIMIT"
id = "<32-char hex ID>"
```

---

## 7. Local dev URLs

| URL | What it is |
|---|---|
| `http://localhost:8787` | Local Worker via `wrangler dev` (backend) |
| `http://localhost:8787/api/health` | Health check endpoint |
| `http://localhost:8787/` | API info endpoint |
| `file:///Users/<you>/Desktop/kickoff/public/index.html` | Local frontend (Day 53 placeholder) |

---

## 8. Production URLs (planned)

| URL | What it is | When |
|---|---|---|
| `https://kickoff-api.<subdomain>.workers.dev` | Default Worker URL (before custom domain) | Day 55 (`wrangler deploy`) |
| `https://api.<domain>` | Worker on custom domain | Day 55 evening |
| `https://<domain>` | Cloudflare Pages frontend | Day 57 |

---

## 9. `.gitignore` reference

Anything on this list should NEVER be committed to the repo:

```
node_modules/
.wrangler/
.dev.vars
.env
.env.local
dist/
build/
.DS_Store
```

Full `.gitignore` is committed to the repo root — see there for the exhaustive list.

---

*End of ENVIRONMENT.md v1.0 — Day 53 of 60*
