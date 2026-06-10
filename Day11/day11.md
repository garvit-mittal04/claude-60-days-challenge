# Day 11 — ATS Resume Optimizer & Resume Generator

**Project:** Resume × Target JD ATS Alignment
**Challenge:** #60DayClaudeChallenge — Day 11
**Target Role:** Business Analyst, Strategic Finance & Analytics — Capital One (Plano, TX, Post-Grad)
**Deliverable:** ATS analysis + optimized resume

---

## Overview

Used the ATS Resume Optimizer & Resume Generator prompt to score my current resume against a representative Business Analyst job description at Capital One — a realistic stretch target combining my finance background, BA&AI MS, FP&A AI Agent project, and Power Platform skills. Claude returned an ATS Match Score, detailed Gap Analysis, and a fully optimized resume in the required format.

---

## Files in this folder

- `target-job-description.md` — the JD I optimized against
- `ats-analysis.md` — ATS Match Score + Gap Analysis (Missing Keywords, Missing Skills, Improvement Opportunities)
- `Garvit_Mittal_Optimized_Resume.md` — the rewritten resume in the prompt's exact format
- `day11.md` — this writeup
- `screenshots/` — screenshots of ATS analysis and optimized resume

---

## Result Headline

# **82 / 100**

Strong match. Cleared all "Required" qualifications and most "Preferred" ones. Remaining gaps concentrate in cloud infrastructure (AWS, Snowflake) and explicit financial services industry exposure — fixable through positioning and one targeted certification, not new credentials.

---

## Screenshots

> Drop screenshots before committing:
>
> - `screenshots/01-ats-score.png` — ATS Match Score (82/100)
> - `screenshots/02-gap-analysis.png` — missing keywords + skills + opportunities
> - `screenshots/03-optimized-resume-top.png` — name, summary, skills
> - `screenshots/04-optimized-resume-exp.png` — experience timeline
> - `screenshots/05-optimized-resume-projects.png` — projects section

---

## What Changed in the Resume

| Section | Before → After |
|---|---|
| **Professional Summary** | Generic analytics summary → Surfaces "financial modeling," "FP&A workflows," "variance analysis," and the $840K savings + sub-2-min automation outcomes upfront |
| **Skills** | Single-paragraph keyword dump → 8 keyword-rich category lines (Programming, BI, ML, FP&A, Automation, Data Engineering, Deployment, Business) |
| **Experience** | 3 roles (JityAI, Harsiddhi, Dipankar Gupta) → 4 roles with current Chicago School internship added; key tools and dollar/percentage metrics bolded for ATS pickup |
| **Projects** | FP&A Agent listed second → FP&A Agent moved to top (most JD-relevant), 60-Day Claude Challenge added as continuous-learning signal |
| **Education** | "A/B Testing" coursework → "A/B Testing & Causal Inference" (closes a keyword gap honestly) |
| **Achievements** | New section surfacing 5 quantified wins recruiters can quote in interviews |

---

## Key Learnings

**1. The ATS isn't reading your resume the way you think it is.** A human reader sees a narrative. The ATS sees a keyword bag-of-words. Both readers matter. The optimization is making the same factual content satisfy both — without inventing anything.

**2. Honest positioning beats fabrication.** Capital One's JD asks for "causal inference." My resume said "A/B Testing." Same coursework, more aligned phrasing. That's not lying — that's translation. The line between "rephrase" and "invent" is whether the underlying experience is real.

**3. The biggest wins are structural, not lexical.** Moving the FP&A AI Agent project to the top of Projects is worth more than any 5 keyword swaps. Recruiters scan top-to-bottom, left-to-right. Put the JD-relevant thing first.

**4. Gaps you can't fix in 1 prompt deserve a separate plan.** The cloud (AWS, Snowflake) gap isn't a writing problem — it's a credentials problem. Claude flagged it. I added it to my Q3 learning plan: AWS Cloud Practitioner in one weekend, Snowflake free tier added to my warehouse project README in another. Same fix, different timeline.

**5. Quantify everything that touches money or time.** $840K, sub-2-minute, 15–20%, 25% close time, 91.7% accuracy — these aren't bragging. They're the only sentences a recruiter can repeat in a hiring committee meeting on your behalf.

---

## What surprised me most

How much of "ATS optimization" is just discipline. The information was already in my resume. Claude didn't add anything — it reorganized, retitled, and bolded. The 82/100 score wasn't a writing quality score. It was a *legibility* score for a machine reader. The same content earlier in the document, in the same words used by the JD, scored 30 points higher.

Also: separating the "missing skills" (real gaps, e.g., AWS) from "missing keywords" (writing fixes, e.g., financial modeling) made it instantly clear what to do this weekend versus what to do this quarter.

---

## Tech Stack

- Resume scoring + rewrite via Claude (single prompt with resume PDF + JD as inputs)
- Output format: Markdown for portability across Word, Google Docs, FlowCV, Overleaf, Canva
- No backend, no scraping — pure prompt engineering

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
