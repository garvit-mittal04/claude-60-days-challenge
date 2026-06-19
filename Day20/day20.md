# Day 20 — Build an AI Face Puzzle Game

**Project:** `face-puzzle-game.html` — single self-contained HTML file, no frameworks, no build step
**Challenge:** #60DayClaudeChallenge — Day 20
**Deliverable:** A browser game that turns your webcam selfie into a draggable jigsaw puzzle with timer, move counter, win detection, and a persistent local leaderboard.

---

## What it does

1. Asks for webcam permission, shows a mirrored live preview with a circle face guide.
2. On **Take Photo**, snapshots a square crop of the center of your face onto a canvas.
3. You pick a difficulty: **3×3 (9 pieces)**, **4×4 (16)**, or **5×5 (25)**.
4. The captured image is sliced into N×N tiles. Pieces are scrambled (guaranteed not-identity) and laid out on a square board.
5. You drag pieces with mouse or touch. Dropping a piece on another swaps them. Snap-to-grid on release. Dragged pieces get a cyan glow; pieces in their correct cell get a green border.
6. Live HUD: `mm:ss.t` timer, total moves, correctly-placed-out-of-total counter, current grid size.
7. When every piece lands in its correct cell, the timer stops, a confetti burst fires, and a results overlay shows time / moves / grid.
8. The result is saved to `localStorage` and the **Top 5 Best Times** leaderboard updates with rank, time, moves, grid, and timestamp.
9. **Retake Photo**, **Play Again**, and **New Photo** buttons cover the full reset flow. A **Demo Image** button bypasses the camera so you can play without granting webcam access (useful for shared/desktop screenshots).

---

## Tech notes

- **Single HTML file. No frameworks.** ~28 KB total. All CSS and JS inline. No external dependencies.
- **Camera:** `navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } })`. Permission denial is caught and shown as an inline error — the demo image option still lets you play.
- **Capture:** center-cropped square drawn to a 720×720 canvas with horizontal flip (so the snapshot matches the mirrored preview the user sees).
- **Slicing:** each piece is a positioned `<div>` with `background-image: url(<data-url>)`, `background-size: <board-size>` (the full board), and `background-position` offset to the piece's correct row/column. The same image is used as one big sprite sheet — much faster than slicing into N² individual canvases.
- **Drag:** pointer events (`pointerdown` / `pointermove` / `pointerup`) handle mouse and touch in one path. `setPointerCapture` keeps the pointer locked to the piece even if it leaves the cell briefly. `touch-action: none` on the board kills the mobile scroll-fight.
- **Swap logic:** on drop, the nearest grid cell is computed by rounding `(currentLeft / cellSize)`. If a piece already lives there, the two pieces' `(row, col)` swap, both move to their new pixel coords, and the move counter increments.
- **Solvability:** because every move is a *swap* (not a 15-puzzle slide), every permutation is reachable from every other permutation — no parity check needed. The initial scramble is only re-rolled if it happens to land on identity (would be a 0-move "win").
- **Timer:** `performance.now()` deltas, repainted every 80 ms in `mm:ss.t` format.
- **Persistence:** top 5 best times stored as JSON under `facePuzzle.topTimes.v1` in `localStorage`. Sorted ascending by `time`. A **Clear** button wipes them with a confirm dialog.
- **Responsive:** the board uses `aspect-ratio: 1/1` and caps at 540 px. A `resize` listener rebuilds piece positions/sizes so cell math stays correct when you rotate a phone or resize a desktop window.
- **Win effects:** 80 colored confetti slivers animated with `@keyframes fall` for 3.2 s. Pure CSS, no canvas needed.

---

## Key learnings

**1. Pointer events > separate mouse + touch listeners.**
Writing one drag path that handles both desktop and mobile is cleaner than maintaining `mousedown` + `touchstart` separately. The browser unifies them. Combined with `touch-action: none`, mobile drag just works.

**2. One sprite sheet beats N² canvases.**
Slicing the captured photo into 25 individual canvases (for 5×5) is wasteful. Using the *full* captured image as a CSS background and just offsetting `background-position` per piece gives you the same visual result with one image decode and effectively free piece updates.

**3. Swap puzzles are always solvable. Slide puzzles aren't.**
The classic 15-puzzle has a parity constraint — only half of all permutations are reachable. Because this game lets you pick up any piece and swap it with any other, every starting permutation reaches the solved state. That subtle design choice eliminates a whole class of "shuffle until valid" code.

**4. The hardest part wasn't drag-and-drop. It was capture geometry.**
The mirrored preview vs the unflipped capture, the center-crop math (`(videoWidth - size) / 2`), the relationship between board pixel size and piece background offset — those took more iterations than the drag logic. UI layout and image geometry are where small bugs hide and look correct until you really look.

**5. Add a Demo Mode for anything that needs hardware.**
Webcam-gated apps die in 1-on-1 screen shares and demo videos. A "use this placeholder" button costs 20 lines of code and unblocks every reviewer who isn't willing to grant camera access in the moment. Good defensive UX.

**6. `localStorage` is enough for solo-leaderboards.**
No backend. No auth. No JSON files. Just `setItem` + `getItem` + a JSON sort. For a personal best-times tracker that's the entire stack.

---

## What surprised me most

The win-detection code is one line — `pieces.every(p => p.row === p.correctRow && p.col === p.correctCol)`. That's it. The 600 lines of game logic around it exist purely so that one line can return `true`.

Most of what feels like "the game" is plumbing. The actual game-state question is trivial. That ratio is a useful intuition pump for shipping anything: most of the work is the surface area that lets the core decision happen, not the decision itself.

---

## Run it

1. Download `face-puzzle-game.html`.
2. Open it locally in Chrome / Firefox / Safari. (Camera requires HTTPS or `localhost` — opening directly via `file://` will block the webcam on some browsers, but the **Demo Image** button works regardless.)
3. Allow camera, take a photo, pick a grid, solve it.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
