# Day 38 — Typing Speed Studio

**Project:** `typing-speed-studio.html` — a premium adaptive typing platform
**Challenge:** #60DayClaudeChallenge — Day 38
**Category selected:** Adaptive (all 7 categories)
**Features:** Claude decided (all premium features)
**Type:** Single-file vanilla HTML/CSS/JavaScript. No frameworks, no CDN, no images, no APIs. Works fully offline.

---

## What I built

A commercial-grade typing platform inspired by Monkeytype. Fully adaptive across 7 content categories, 8 modes, live stats during every session, and a Monkeytype-style analytics dashboard at the end with WPM/accuracy charts, character breakdown, error heatmap, achievement badges, personal-best tracking, and a personalized performance summary. Session history persists in `localStorage` so users can track improvement over time without any account.

The whole thing lives in one 65 KB HTML file. No frameworks, no CDN, no external dependencies.

---

## The two-question intro (per task spec)

Before generating anything, I asked the user two questions — one at a time as the prompt required:

1. **What kind of typing experience would you like to build?** — with options including General English, Programming, Business, Academic, Medical, Legal, Creative Writing, and Adaptive.
2. **Should Claude decide the features, or customize?** — Claude decides (all premium features) or Custom.

The user chose **Adaptive · Claude decides**. That meant building every category and every feature in one app.

---

## The 7 content categories (all built)

Every category has a fresh, hand-authored passage library. No copy-paste across modes.

| Category | Sample passages |
|---|---|
| **General** | Common-word random streams + 7 flowing prose passages |
| **Programming** | 9 real code snippets across JavaScript, Python, HTML, CSS, SQL, Java, C++ |
| **Business** | Executive emails, board updates, quarterly reports, proposals |
| **Academic** | Methodology sections, literature review paragraphs, findings paragraphs |
| **Medical** | ER presentation notes, medication reconciliation, discharge summaries |
| **Legal** | Contract clauses, dispute resolution, confidentiality, termination |
| **Creative** | Literary prose in the style of published fiction |

The category selector is a dropdown in the settings bar. Change it and the next session pulls from that category's library.

---

## The 8 modes

1. **Time** — 15 / 30 / 60 / 120 seconds. Session auto-finishes when the clock hits zero.
2. **Words** — 25 / 50 / 100 / 250 words. Session finishes when word count is met.
3. **Quote** — Random famous quote passage. Fixed length, no timer.
4. **Programming** — Code snippet with real syntax. Language selector (JS/Python/HTML/CSS/SQL/Java/C++).
5. **Custom** — User pastes their own text. Fixed length.
6. **Adaptive** — Category dropdown swaps content; every mode adapts to it.
7. **Focus** — Only the current typing area stays fully visible; everything else dims to 15% opacity for distraction-free concentration.
8. **Zen** — No timer, no stats, no scoring. Pure practice — the entire live-stats bar is hidden and no history is recorded.

---

## Live stats during typing

The stats bar animates in real time as you type:

- **WPM** (net — correct chars / 5 / minutes)
- **Raw WPM** (all typed chars / 5 / minutes)
- **Accuracy** (correct / (correct + wrong) × 100)
- **CPM** (correct chars per minute)
- **Elapsed time** or **remaining time** depending on mode
- **Word count** (for Words mode)
- **Mistake count**
- **Max streak** — longest run of consecutive correct characters
- **Completion %** — animated progress bar under the stats

All calculations are bounded — no unrealistic 20,000 WPM values.

---

## The analytics dashboard

When a session ends, the entire card transforms into a Monkeytype-style analytics dashboard.

**Top row — hero stats.**
Four large tiles: WPM (with raw WPM below), Accuracy (with mistake count), Consistency (with max streak), and Percentile estimate (with personal-best callout).

**Consistency calculation.**
The app samples WPM every second during typing. Standard deviation divided by mean, subtracted from 100, gives a proper Monkeytype-style consistency score.

**WPM and Accuracy over time.**
Two side-by-side line charts rendered inline in SVG. Filled gradient underlay, sharp curve, data-point dots. Both use the current theme color.

**Characters typed breakdown.**
Four color-coded bars: Correct (green), Incorrect (red), Extra (orange), Missed (yellow). Each animates from 0 to its final width.

**Error heatmap.**
The top 10 mistyped keys as a grid of cells. Cells that had 5+ errors get a hot-red background. 3-4 errors get amber. 1-2 errors get neutral. Each cell shows the key + the miss count.

**Achievement badges.**
Nine possible earn conditions: ⚡ 40+ WPM, 🚀 60+ WPM, 💨 80+ WPM, 🔥 Century Club, 🎯 Sharp (95%+ acc), 💎 Perfect Run, 📏 Consistent, ⛓️ 100-char streak, 👩‍💻 Code Sprint.

**Personalized performance summary.**
Not template text. Three paragraphs — Strengths, Weaknesses, and Suggestion — computed from the specific session's WPM, accuracy, consistency, and top mistyped keys. A user with 92% accuracy + high WPM gets a different suggestion than a user with 78% accuracy + medium WPM.

---

## Session history — localStorage persistence

- Every completed session saves to `localStorage` (Zen mode excluded).
- History view shows: WPM, accuracy, mode, category, date/time — with the personal best flagged with a 🏆 icon.
- Summary tiles at the top of history: Personal Best, Average WPM, Average Accuracy, Total Sessions.
- Clear-history button with confirmation.
- Cap at 50 most recent sessions.

Nothing leaves the browser. No accounts, no server.

---

## Interaction details

**Character-by-character rendering.** Every character in the target text is a `<span class="char">`. As the user types, each span gets `.correct`, `.wrong`, or `.extra`. A blinking `.current` cursor appears at the active position via CSS pseudo-element.

**Wavy underline on errors.** Wrong characters get the `text-decoration: underline wavy` treatment — matches Monkeytype's approach.

**Auto-scroll.** When the current character is offscreen, the app scrolls it into view.

**Extra characters.** Type past the end of a word? Those characters get the "extra" orange color instead of being silently absorbed.

**Backspace.** Full backspace support — including subtracting from the correct/wrong counters when you erase.

**Sound feedback.** Optional Web Audio API tones — sine wave 660 Hz on correct keystrokes, 220 Hz on incorrect. No external audio files.

**Themes.** Five color themes as dots in the header — Gold (Monkeytype signature default), Claude Orange, Mint, Sky, Rose.

**Font size.** Four sizes (S/M/L/XL) with quick toggles in the settings bar.

**Keyboard shortcuts.** Tab restarts with the same text. Escape generates new text. Space pauses/resumes.

**Pause / resume.** The elapsed time correctly excludes paused duration.

**`prefers-reduced-motion`** respected — all animations disable if the OS setting is on.

---

## Tech notes

- **Single HTML file.** ~65 KB. Vanilla CSS + JavaScript. No frameworks, no CDN, no external assets, no APIs.
- **Five color themes** via `data-theme` on `<body>` — all colors resolve through CSS custom properties.
- **Web Audio API** for click sounds — no audio files.
- **Inline SVG** for WPM and Accuracy line charts.
- **localStorage** for session history with a 50-session cap.
- **Hidden input trick** for keystroke capture — the visible typing area is a `<div>`, but a hidden `<input>` off-screen captures every keydown so all text-input browser behaviors (including mobile keyboards) work.
- **Reusable component patterns** — every mode uses the same `renderStage`, `renderTypingText`, and `updateLiveStats` functions.
- **Realistic value clamping** — WPM capped at 300, CPM at 1500. No impossible metrics.

---

## What this exercise demonstrates

### 1. Live feedback teaches faster than a scoreboard

A quiz measures whether you can type. A live-WPM display teaches you what typing feels like at 60 WPM. That in-session feedback loop — see the number, feel the pace — is why real typing platforms use it and why traditional word-processor-based practice never taught anyone to type fast.

### 2. Consistency is the real graduation metric

WPM measures your best 5-second moment. Consistency measures how close your slowest moments are to your fastest. That's the difference between hitting 60 WPM once and being a 60 WPM typist. The consistency score is the number that matters after your first month of practice.

### 3. Content matters as much as mechanics

Typing "the quick brown fox" one thousand times will not make you a good programmer at a keyboard. That's why programming mode uses real code snippets with actual syntax, medical mode uses actual clinical documentation, and legal mode uses actual contract language. The category system is the whole point of "adaptive" — you're training the muscle memory for the text you'll actually type in your work.

### 4. The dashboard should teach, not just score

Monkeytype's dashboard is famous because it doesn't just give you a number — it shows you a graph of your WPM over the session, a heatmap of your errors, and a percentile estimate. The performance summary in this app follows the same principle: three specific paragraphs about your strengths, weaknesses, and next practice target — not a generic "keep practicing" message.

---

## Narrative continuity

- **Day 32** — marketing strategist simulator
- **Day 33** — media literacy analyzer
- **Day 34** — marketing detective
- **Day 35** — prompt puzzle
- **Day 36** — cognitive pattern explorer
- **Day 37** — task compass
- **Day 38 — typing speed studio**

Meta-skill arc continues — Day 35 taught prompting, Day 38 teaches the physical mechanics of typing itself. Same "teach → observe → commit → reveal" pedagogy applied to a domain where the "reveal" is your own real-time performance graph.

---

## Tags

`#60DayClaudeChallenge` `#TypingPractice` `#WebApp`
