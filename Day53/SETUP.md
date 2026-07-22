# Kickoff — SETUP.md

**Version:** 1.0 · Day 53
**Purpose:** Reproducible install guide. Anyone should be able to clone this repo, follow this doc top to bottom, and end up with a running local dev environment in under 15 minutes.

---

## Prerequisites

- **macOS 12+** (this guide is written for macOS; Linux and Windows steps differ slightly on Homebrew)
- **~500 MB of disk space** for Node + Wrangler + workerd binaries
- **A Cloudflare account** — free tier, no credit card required
- **A code editor** — VS Code recommended but any editor works

---

## 1. Install Homebrew (macOS package manager)

Skip this if `brew --version` already prints a version.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the on-screen prompts. May take 5-10 minutes on first install.

## 2. Install Node.js (JavaScript runtime)

```bash
brew install node
node --version   # should print v20+ or higher
npm --version    # should print v10+ or higher
```

Non-Homebrew alternative: download the LTS installer from https://nodejs.org.

## 3. Install Wrangler (Cloudflare CLI)

```bash
npm install -g wrangler
wrangler --version   # should print ⛅️ wrangler 4.x.x
```

## 4. Log into Cloudflare

```bash
wrangler login
```

Opens a browser tab. If you don't have a Cloudflare account, click **Sign up** (free, 30 seconds, no credit card). Then click **Allow** to authorize Wrangler.

Confirm you're logged in:

```bash
wrangler whoami
```

Should print your email and Cloudflare Account ID.

## 5. Clone the repo

```bash
cd ~/Desktop
git clone https://github.com/garvit-mittal04/kickoff.git
cd kickoff
```

## 6. Install backend dependencies

```bash
cd api
npm install
```

Takes ~30 seconds. Wrangler + TypeScript + Cloudflare Workers type definitions get installed into `api/node_modules/`.

If you see `npm warn allow-scripts` messages about workerd, esbuild, or sharp, re-run with:

```bash
npm install --foreground-scripts
```

This lets those packages complete their postinstall scripts (needed for `wrangler dev` to work).

## 7. Run the backend locally

```bash
npx wrangler dev
```

Should print:

```
[wrangler:info] Ready on http://localhost:8787
```

Leave it running. Open a new Terminal tab.

## 8. Test the backend

```bash
curl http://localhost:8787/api/health
```

Should return JSON with `"status": "ok"` and a timestamp.

```bash
curl http://localhost:8787/
```

Should return JSON listing the planned API endpoints.

## 9. Open the frontend

```bash
open ~/Desktop/kickoff/public/index.html
```

Opens the placeholder landing page in your default browser. You should see the Kickoff logo, tagline, and "UNDER CONSTRUCTION · SHIP TARGET DAY 60" tag.

## 10. You're set up

- Backend: `http://localhost:8787` (Cloudflare Worker running via `wrangler dev`)
- Frontend: `file:///.../kickoff/public/index.html` (static file, will move to Cloudflare Pages Day 54+)
- Repo: `~/Desktop/kickoff`

To stop the local Worker: `Ctrl+C` in the `wrangler dev` tab.

---

## Troubleshooting

### `wrangler dev` fails with "workerd binary not found"

The workerd postinstall script didn't run. Fix:

```bash
cd ~/Desktop/kickoff/api
npm rebuild workerd
```

### `wrangler login` doesn't open the browser

Copy the URL that appears in Terminal and paste it into your browser manually.

### Port 8787 already in use

Kill whatever's using it:

```bash
lsof -ti :8787 | xargs kill
```

Or run wrangler on a different port:

```bash
npx wrangler dev --port 8788
```

### TypeScript errors in `src/index.ts`

Make sure `@cloudflare/workers-types` installed:

```bash
cd ~/Desktop/kickoff/api
npm install --save-dev @cloudflare/workers-types
```

---

*End of SETUP.md v1.0 — Day 53 of 60*
