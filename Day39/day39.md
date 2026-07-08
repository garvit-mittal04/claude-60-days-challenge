# Day 39 — PDF Splitter & Merger

**Project:** `pdf-splitter-merger.html` — a premium client-side PDF utility
**Challenge:** #60DayClaudeChallenge — Day 39
**Design mode:** Claude decides — full-featured build
**Type:** Single-file HTML/CSS/JS application. Uses PDF-lib and PDF.js loaded from CDN on first visit, then works fully offline. Zero backend, zero uploads.

---

## What I built

A commercial-grade PDF toolkit that runs 100% inside the browser. Two tools in one app — Splitter and Merger — both with drag-and-drop uploads, live page-thumbnail previews, real-time output structure previews, progress indicators, and download of the processed files without any file ever leaving the user's device.

The single most important design choice: **nothing gets uploaded**. Every byte of every PDF is read into memory, processed with PDF-lib, and downloaded back — all locally. That's a real privacy feature, not marketing copy.

---

## PDF Splitter

Upload one PDF (drag-drop or click). The app auto-detects the total page count and renders visual thumbnails for every page (PDF.js). Then you pick from four split methods:

**1. Custom Ranges.** Type ranges like `1-3, 5, 7-9`. Each range becomes a separate output PDF. Multiple ranges in a single operation. Invalid ranges are highlighted in red as you type.

**2. Every N Pages.** Enter a chunk size. The app splits the PDF into that many chunks (`ceil(pages / N)` files). Useful for breaking a large document into equal parts.

**3. Split After Pages.** Type page numbers where you want cuts. `3, 7, 12` creates four output files: pages 1-3, 4-7, 8-12, 13-end. This is the mode that matches how humans usually think about splitting a document — "cut here, here, and here."

**4. Extract Selected.** Tap thumbnails to select the pages you want. All selected pages are extracted into one new PDF, in their original order.

Below every split-method config, an **Output Preview** panel shows the exact list of files that will be produced — filename and page count for each — before you click Split. So you can validate the plan before generating anything.

---

## PDF Merger

Upload multiple PDFs at once via drag-and-drop or multi-select file picker. Each PDF appears in a sortable list with:

- A drag handle at the left.
- A thumbnail of page 1 (rendered by PDF.js).
- Filename, page count, file size.
- Move Up / Move Down / Remove buttons.

**Drag-to-reorder** works with native HTML5 drag-and-drop — you can drag any row and drop it above another row to reorder the merge sequence.

Above the merge button, a live summary shows: number of files, total combined page count, estimated output size. So the user always knows what they're about to generate.

Hit Merge → the app copies all pages from all PDFs into a new document in the order you set, saves it, and triggers a download. All in-browser.

---

## The libraries + why

**PDF-lib** for creating and modifying PDFs (the actual splitting and merging). Zero-dependency, works in the browser, actively maintained. Chosen because it exposes exactly the operations we need — `PDFDocument.create()`, `copyPages()`, `addPage()`, `save()` — with a clean API.

**PDF.js** (Mozilla) for rendering thumbnails. Battle-tested, ships with Firefox itself. Chosen because it can render any PDF page to a `<canvas>` at any scale — perfect for thumbnails at 0.2–0.3× scale.

Both loaded from cdnjs.cloudflare.com on first load. After the first load, the browser cache holds them, and the app works completely offline.

---

## Full feature list (per task spec)

**Splitter:**
- Auto-detects page count ✓
- Visual page thumbnails for every page ✓
- Preview before splitting ✓
- Split by page numbers ✓
- Split by custom page ranges ✓
- Split after specific pages ✓
- Split every N pages ✓
- Extract selected pages ✓
- Multiple split ranges in one operation ✓
- Validate all page ranges ✓
- Preview output document structure before processing ✓
- Highlight invalid inputs in red ✓

**Merger:**
- Upload multiple PDFs via drag-drop or picker ✓
- Sortable list with page counts ✓
- Visual previews (page 1 thumbnails) ✓
- Reorder via drag-and-drop ✓
- Display total files + total page count + estimated output ✓
- Generate merged PDF + download ✓

**Cross-cutting:**
- Drag-and-drop uploads ✓
- Processing indicators with progress bar ✓
- Loading animations ✓
- Responsive layout ✓
- Dark mode default + light mode toggle ✓
- Accessibility (keyboard nav, focus rings, ARIA) ✓
- Keyboard shortcuts (Space/Enter on upload zones) ✓
- Intuitive error handling (toasts for failures) ✓
- Smooth micro-interactions throughout ✓
- Works offline after first load ✓

---

## Design details

**Two-tab layout at the top** — Split PDF and Merge PDFs — with an active-tab gradient pill. Switching tabs preserves the other tab's state (upload a PDF to split, hop to merge, hop back — your split PDF is still loaded).

**Privacy pill** in the header: *"🔒 100% in your browser"* — a small green badge that names the feature that matters most to users of PDF tools.

**Upload zones** with 2.5px dashed borders, hover states, and a giant emoji icon. Drop or click.

**Thumbnail grid** for split — 110px minimum column width, responsive. Selected thumbnails get an accent border and a floating ✓ badge in the top-right corner.

**Merge list rows** with a drag handle (`≡`), a small page-1 thumbnail, filename + metadata, and inline reorder/remove buttons.

**Live output preview** on every split method — before you click Split, you see the exact filenames and page ranges the app will produce.

**Progress cards** during processing — spinner + status message + animated progress bar. Every file operation shows exactly what's happening.

**Toast notifications** — top-right, auto-dismiss after 2.6s, color-coded (green for success, red for errors).

**Dark mode default** with soft blue-purple accents. Light mode toggle in the header — all colors swap through CSS custom properties.

---

## Tech notes

- **Single HTML file.** ~55 KB (excluding CDN libs). Vanilla CSS + JavaScript. No frameworks.
- **PDF-lib and PDF.js** loaded from CDN once; cached and offline after that.
- **State machine** — separate render function per tab; a single `render()` dispatcher.
- **HTML5 drag-and-drop** for reorder and for the upload zones.
- **Blob URLs** for downloads via `URL.createObjectURL` + `<a download>`.
- **Progressive thumbnail rendering** — thumbnails render in batches of 4 with re-renders in between, so a 200-page PDF doesn't block the UI.
- **`ignoreEncryption: true`** on PDF-lib load — the app can open a lot of "encrypted" PDFs that other tools reject.
- **`prefers-reduced-motion`** respected.

---

## What this exercise demonstrates

### 1. Client-side PDF processing is now a real UX

For years, "PDF splitter" websites required uploading your document to a server. That's a privacy compromise most people accept out of resignation. The combination of PDF-lib + PDF.js means the same tool can now run entirely in the browser — no upload, no privacy risk, no waiting on a network round-trip. This build is a proof point that the modern browser can do what dedicated PDF servers used to do.

### 2. Preview-before-do is the pattern that separates tools from utilities

Every PDF utility on the internet lets you click Split and gives you files. The difference between a tool and a utility is showing you the exact list of files with page ranges *before* you click. If the plan is wrong, you fix the config, not the output. This is why the Output Preview panel is the most important UI element in the split tool.

### 3. The best privacy feature is the one that's structurally impossible to violate

*"We don't upload your files"* is a promise. *"The files are in a browser tab that has never made a network request with them"* is a fact. The distinction is why serious PDF users prefer client-side tools even when they're slower.

### 4. Libraries in a single-file app are still a single-file app

The task spec allowed CDN libraries where necessary. PDF-lib and PDF.js are two-third-party dependencies loaded via `<script src="...">` tags. The rest of the app — every UI element, every state transition, every event handler — is vanilla. There's no build step, no npm, no framework, no bundler. Anyone can open the file, view source, and understand the code.

---

## Narrative continuity

- **Day 35** — prompt puzzle
- **Day 36** — cognitive pattern explorer
- **Day 37** — task compass
- **Day 38** — typing speed studio
- **Day 39 — PDF splitter & merger**

Meta-skill arc pauses briefly for a genuinely useful utility. Day 38's typing platform and Day 39's PDF toolkit are both examples of "commercial-grade single-file webapps" — the pattern where a full-featured product ships as one file with no server, no accounts, no data collection. That combination is genuinely underused, and it's what makes both apps feel different from the average online tool.

---

## Tags

`#60DayClaudeChallenge` `#PDFTools` `#WebApp` `#Privacy`
