# Day 18 — Brain Dump → Action Planner Skill

**Project:** Custom Skill `brain-dump-action-planner` + Live test with capstone kickoff notes
**Challenge:** #60DayClaudeChallenge — Day 18
**Skill Name:** `brain-dump-action-planner`
**Test Input:** MS Capstone Project kickoff meeting (stream-of-consciousness notes)
**Why this test:** Realistic messy meeting notes — multiple speakers, deadlines, conflicts, open questions, action items — exactly the kind of input the Skill exists to handle.

---

## Custom Skill Setup (claude.ai → Skills → + New)

### Skill Name
```
brain-dump-action-planner
```

### Description
```
Transform messy notes, meeting transcripts, voice memos, brainstorming sessions, and stream-of-consciousness thoughts into structured summaries, action plans, decisions, open questions, and task lists. Organize information clearly without inventing, assuming, or filling gaps. Preserve all names, dates, numbers, and terminology exactly as provided.
```

### Instructions (paste exact from challenge prompt)

> The full instructions from the challenge — covering Full Breakdown / Transcript Mode / Merge Mode, the 8 required sections (Summary, Key Takeaways, Action Items, Open Questions, Risks/Blockers, Conflicts, Additional Notes, Source Info), the status badge system (🔴🟠🟢⚠️❓✅⏳), and the design goals (Notion/Linear/ClickUp aesthetic). Output is always a self-contained HTML artifact starting with `<style>`.

---

## Files

- `sample-brain-dump.txt` — realistic messy capstone meeting notes (the input I tested with)
- `action-planner-output.html` — the structured HTML dashboard the Skill generated from those notes (open in browser)
- `day18.md` — this writeup
- `screenshots/` — Skill setup + generated dashboard screenshots

---

## How the Skill is Invoked

| Step | Action |
|---|---|
| 1 | claude.ai → Side nav → **Skills** |
| 2 | Click **+ New Skill** |
| 3 | Paste Name + Description + Instructions from above |
| 4 | Save |
| 5 | Start a new chat, paste any messy notes, optionally add a tag like *"full breakdown"*, *"transcript mode"*, or *"merge mode"* |
| 6 | The Skill auto-fires. Returns one HTML artifact you can save as a file. |

**Three modes supported by the Skill:**
- **Full Breakdown** (default) — one source, full 7-section dashboard
- **Transcript Mode** — multi-speaker, adds Speaker Summary + Decisions/Actions by Speaker
- **Merge Mode** — multiple sources combined; surfaces duplicates and conflicts without auto-resolving

---

## Sample Test Run

### Input (excerpt from `sample-brain-dump.txt`)

> *"ok so we finally had the kickoff today and lots of stuff came up... let me dump everything before i forget. topic — AI-Powered Customer Churn Prediction. sarah suggested telco churn dataset has like 7000+ rows pretty clean. marcus wants to also bring in IBM HR analytics data for comparison says it'll make our model more robust. i'm not sure yet. prof anderson said we need to submit final topic confirmation by july 5..."*

(~450 words of stream-of-consciousness, no formatting, lots of inline thinking, deadlines and names scattered throughout)

### Output (rendered in `action-planner-output.html`)

The Skill returned a dashboard with all 7 required sections:

| Section | Items Generated |
|---|---|
| **Summary** | 1 paragraph capturing meeting context, decisions made, and primary risk |
| **Key Takeaways** | 6 grid cards (topic, dataset, 3 roles, budget) |
| **Action Items** | 6-row interactive table with Task / Owner / Deadline / Status / Priority |
| **Open Questions** | 3 unresolved items (dataset scope, IRB approval, paper publication) |
| **Risks & Blockers** | 2 cards (Marcus travel overlap, budget for neural-net training) |
| **Conflicts** | 1 conflict card (Prof's 3-dataset cap vs Marcus's 5+ preference) |
| **Additional Notes** | 4 supporting context items (weekly sync, job opportunity, GPU access, panel signal) |

Every field traced back to the source notes. **Nothing was invented.** Where information was missing (e.g., budget contingency plan), the Skill correctly left it blank or flagged it as TBD rather than guessing.

---

## What the Skill Does That a Plain Prompt Doesn't

| Plain prompt | Custom Skill |
|---|---|
| Re-paste the 500-word instruction every chat | Auto-fires on any notes pasted |
| Output varies — sometimes markdown, sometimes plain text | Output is always self-contained HTML dashboard |
| Hallucination risk — the model may "fill in" missing fields | Hard rule: "Never invent values" enforced every time |
| Status indicators inconsistent across runs | Badge system (🔴🟠🟢⚠️❓✅⏳) standardized via the Skill |
| Three modes (Full / Transcript / Merge) need re-explaining | Modes baked in — invoke with a single tag |

---

## Key Learnings

**1. Skills compound trust.** A plain prompt produces a plausible output. A Skill with hard rules ("Never invent values," "Preserve names exactly," "Output only HTML artifact") produces a *trustworthy* output. For high-stakes work — board notes, research interviews, legal transcripts — trust is the actual value.

**2. The dashboard format does cognitive work the markdown doesn't.** Same data shown as a Notion-style dashboard with cards + tables + status badges is parsed by your brain in 30 seconds. The same data as markdown bullets takes 2 minutes and you'll miss the conflicts. Format is information.

**3. Three modes future-proof the Skill.** Most "summarize meeting" tools handle one input format. This Skill handles: one note, one multi-speaker transcript, OR several conflicting sources merged. The Skill becomes useful across every "messy input → structured output" workflow you'll ever have.

**4. The hardest design choice was "never resolve conflicts automatically."** It's tempting to have the AI pick a winner. The Skill correctly refuses. Surfaces the conflict, names the parties, lets the humans decide. This is the right default for any AI handling team workflows.

**5. The 8-section output spec is the Skill's spine.** Once you can rely on the structure showing up every time, you stop verifying *whether* the output covers everything and start using it as a working document. The structure is what turns AI output into a decision tool.

**6. This Skill effectively replaces three tools.** Notion's AI summary + Otter's meeting transcript breakdown + Linear's auto-task-extraction. One Skill, one prompt invocation, single HTML output. The compound replaces ~$50/month in SaaS subscriptions for most personal users.

---

## What surprised me most

How much "intelligence" lives in the *rules*, not the *model*. The model is the same Claude that responds to a plain prompt. The Skill just adds 5 mandatory rules (never invent, preserve names, never auto-resolve conflicts, always 8 sections, HTML only). Those 5 rules turn the same model from a brainstorm partner into a project assistant.

If I were building an AI product startup, I'd care less about the underlying model and more about the rule design layer on top of it. That's where the moat is.

---

## Tech Stack

- Claude Custom Skills (claude.ai → Skills feature)
- Sample input: realistic ~450-word stream-of-consciousness meeting notes
- Sample output: self-contained HTML artifact with embedded CSS (no CDN, mobile responsive)
- Aesthetic: Linear / Notion / ClickUp dark-theme project dashboard

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
