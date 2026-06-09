# Day 10 — Build Your Personal Portfolio Website

**Project:** Personal Portfolio — garvitmittal.com
**Challenge:** #60DayClaudeChallenge — Day 10
**Deliverable:** Single-file responsive portfolio built with Claude

---

## Overview

Built a complete personal portfolio website in a single `index.html` file using HTML + Tailwind CSS (CDN) + vanilla JavaScript. Dark/light mode toggle, typing animation, smooth scroll, active nav highlighting, animated skill bars, scroll reveals, mobile responsive, SEO meta tags. Blue + Orange color theme.

Open `index.html` in any browser. Photo file `profile.jpg` sits alongside it.

---

## Files in this folder

- `index.html` — the complete portfolio (single file, no build step)
- `profile.jpg` — profile photo (referenced relatively)
- `day10.md` — this writeup
- `screenshots/` — portfolio screenshots in dark + light mode

---

## Screenshots

> Drop screenshots before committing:
>
> - `screenshots/01-hero-dark.png` — hero section, dark mode
> - `screenshots/02-hero-light.png` — hero section, light mode
> - `screenshots/03-about-skills.png` — about + skills sections
> - `screenshots/04-projects.png` — projects grid
> - `screenshots/05-experience.png` — experience timeline
> - `screenshots/06-contact.png` — contact section
> - `screenshots/07-mobile.png` — mobile responsive view

---

## Sections Included

The prompt required 8 sections — all implemented:

1. **Hero** — name, typing animation (5 rotating titles), social links, profile photo with gradient glow, "Available for Summer 2026 Internships" status badge
2. **About Me** — 3-paragraph bio + 4-stat impact grid (53K records, 91.7% accuracy, $840K savings, 10/60 challenge progress)
3. **Skills** — 4 categories (Query/Programming, BI/Viz, Analytics Methods, AI/ML) with animated progress bars + 14 tech tag chips
4. **Projects** — 3 cards: Warehouse Decision System (R²=0.934), FP&A AI Agent (Streamlit + Groq), 60-Day Claude Challenge
5. **Experience** — JityAI, Harsiddhi Foods, Dipankar Gupta & Co. with metric-highlighted bullets
6. **Education** — UT Dallas MS, Christ University BS
7. **Certifications & Achievements** — UMD Data Science, Tableau Desktop Specialist, Power BI, Wayland Baptist Economics, plus a featured 60-Day Challenge card
8. **Contact** — direct links (email, LinkedIn, GitHub, location) + working form with mailto fallback

---

## Technical Highlights

- **Tailwind CSS via CDN** — no build step
- **Inter font** via Google Fonts
- **Dark mode default** with class-based toggle persisted in localStorage
- **Typing animation** — JS-driven 5-phrase rotation with blinking cursor
- **Skill bars** — animate on scroll into view via IntersectionObserver
- **Smooth scroll** — native CSS `scroll-smooth`
- **Active nav highlighting** — `scroll`-driven gradient underline
- **SEO meta tags** — title, description, keywords, OG tags, Twitter card, favicon SVG
- **Contact form** — mailto fallback (no backend needed)
- **Custom gradient utilities** — `gradient-text`, `gradient-bg` reused across components
- **Performance** — single HTTP request (Tailwind CDN + fonts only), <50KB photo
- **Mobile responsive** — collapses to single column below 768px, hides desktop nav links on mobile

---

## Key Learnings

**1. The portfolio is the resume now.** Recruiters spend 6 seconds on a resume; they spend a minute on a portfolio if the first 5 seconds work. The hero section is the entire fight — name, what you do, proof you exist (working photo, "available" badge, live status indicator).

**2. Specificity beats breadth.** "I do data" gets ignored. "MS Business Analytics & AI at UT Dallas. I build data products that turn analytics into decisions" tells a hiring manager exactly what to do with you. Every section narrows the funnel.

**3. Numbers make the difference.** "Improved forecast accuracy" is invisible. "15–20% lift" is a sentence a recruiter can quote in an interview. Every bullet in the experience section ends with a metric.

**4. AI shortens the build but not the thinking.** Claude wrote 600 lines of clean Tailwind in under a minute. But the strategic decisions — what stats to feature, which projects make the cut, what tone the bio takes, which two colors carry the brand — those were mine. The output quality directly tracks input clarity.

**5. Deploy is the deliverable, not the file.** A portfolio on your hard drive isn't a portfolio. Saving as `index.html`, pushing to GitHub, and getting a public URL (Vercel, Netlify, or even raw.githack.com) is what makes it real.

---

## What surprised me most

How few decisions a portfolio actually needs once the inputs are clean. Resume, GitHub URLs, color preference, photo — that's the entire spec. Everything else (animation, layout, typography, copy tone, micro-interactions) Claude handled by default. The bottleneck isn't AI; it's how clear you are about who you are before you sit down to build.

---

## Tech Stack

- HTML5 + Tailwind CSS (CDN) + Vanilla JavaScript
- Google Fonts (Inter, 7 weights)
- IntersectionObserver for scroll animations
- localStorage for theme persistence
- mailto: contact form (no backend)
- Single HTML file + 1 image file = entire site

---

## Live URLs

- **Live preview:** https://raw.githack.com/garvit-mittal04/claude-60-days-challenge/main/Day10/index.html
- **Source:** https://github.com/garvit-mittal04/claude-60-days-challenge/tree/main/Day10
- **Custom domain (planned):** garvitmittal.com

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
