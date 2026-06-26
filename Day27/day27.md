# Day 27 — Prior Authorization Story Simulator

**Project:** `prior-auth-story-simulator.html` — interactive conversational story tool
**Challenge:** #60DayClaudeChallenge — Day 27
**Builds on:** Day 26 Prior Authorization Workflow Simulator
**Type:** Healthcare education tool that teaches the PA workflow through an 8-chapter interactive dialogue between a patient and a healthcare operations specialist.

---

## What I built

A single self-contained HTML application that teaches the US healthcare Prior Authorization (PA) workflow as a **branching, chapter-by-chapter conversation between two characters** — Rahul, a 34-year-old patient newly diagnosed with Rheumatoid Arthritis, and Priya, a healthcare operations specialist who explains how PA actually works at each step.

The simulator walks through **eight chapters**, each one representing a real-world checkpoint in a PA journey:

1. **Doctor Visit** — Rahul sees Dr. Patel and gets prescribed Humira (adalimumab)
2. **Insurance Roadblock** — The pharmacy can't fill the prescription; PA is required
3. **What is Prior Authorization?** — Priya explains in plain language, including why it exists
4. **Insurance Review** — Priya walks through the four things StarCare Health actually checks
5. **Denial** — The first PA submission is denied for missing step therapy documentation
6. **Appeal** — Dr. Patel's office assembles a Letter of Medical Necessity
7. **Approval** — The appeal succeeds; confetti animation marks the win
8. **Takeaways** — Two perspectives: what Rahul (patient) learned, and how health systems (operations) actually track this work

After each chapter, the reader chooses one of two dialogue options that subtly influence the narrative direction and get recorded in a "the path you chose" summary at the end.

---

## Technical implementation

### Append-only chat feed

The single most important constraint in the prompt was: **never call `innerHTML =` on the chat container.** Every new chat bubble, narrator line, scene header, choice block, and takeaway panel is built by calling `document.createElement()`, attaching properties and children, then `appendChild()` to the container. The chat feed only ever grows.

That constraint forced a particular code architecture:

- One primitive function per UI element type: `appendBubble()`, `appendNarrator()`, `appendSceneHeader()`, `appendChoices()`, `appendChoiceMarker()`, `appendTakeaway()`
- Each primitive constructs a DOM subtree with `createElement`, applies classes and text via `.className` and `.textContent`, then appends to the chat container
- The only `innerHTML` use anywhere in the file is `removeChild` in a `while` loop to fully clear the chat on Restart — which is allowed because Restart is outside the append cycle, not inside it

### Character positioning

- **Rahul** (patient) always appears as a chat bubble on the **left** with a light-blue avatar circle
- **Priya** (operations specialist) always appears on the **right** with a navy avatar circle and a darker bubble background
- **Narrators, doctors, pharmacists, nurses, and insurer letters** appear as centered italic text — never as chat bubbles — labeled with a small pill ("Narrator," "Dr. Patel," "Pharmacist," "Nurse," "StarCare Health Letter")

That visual grammar makes it obvious at a glance who is speaking without the reader having to parse names.

### Scene timing

Each chapter unfolds with `setTimeout`-paced appends that approximate the rhythm of a real conversation — Rahul asks a question, Priya answers after 800ms, a citation appears in a callout under her bubble after another 1000ms, and the two choices appear at the end. The pacing keeps the reader engaged without feeling artificial.

### Progress bar

A top progress bar updates as the user advances through chapters. Labels under the bar name all eight checkpoints (Doctor → Insurance → What is PA → Review → Denial → Appeal → Approval → Lessons), so the reader can see where they are in the story at any time.

### Stack

- **Tailwind via CDN** (per the prompt) for utility-class styling
- **Vanilla JavaScript** — no framework, no router, no state library
- **In-memory state object** holds chapter index and choice history; nothing persists to localStorage
- **Pure CSS keyframe confetti** triggers when the appeal is approved

---

## Why this format works for healthcare education

A static flowchart of the PA process has been written a hundred times. Most readers skim it once and remember nothing. A conversational simulator works differently:

### 1. The reader is inside the story, not above it

A flowchart is a map you look down at. A dialogue is an experience you move through. When Rahul gets denied in Chapter 5 and reacts with "Denied?! I thought you said this was treatable. What do I do now?" — that's the emotional beat real patients hit, and the reader hits it too. The teaching lands harder because the reader felt the frustration before the explanation arrived.

### 2. Two perspectives are explicitly preserved

The final chapter shows **what Rahul learned (patient perspective)** and **how health systems track this (operations perspective)** side by side. Most healthcare education tools pick one side. By explicitly framing both, the simulator teaches that the same workflow looks completely different depending on which seat you're sitting in — which is the actual core insight of healthcare operations work.

### 3. Citations turn the story into evidence

Each chapter includes a real citation inside Priya's bubble — the AMA 2023 Prior Authorization Physician Survey for treatment delays, industry benchmarks for staff-hours per case, etc. The citations don't interrupt the story; they appear as small italic callouts under the relevant explanation. That gives the reader something to look up if they want to verify, without breaking the narrative flow.

### 4. The two-choice structure invites re-play

Every chapter ends with two options that branch the dialogue slightly. The simulator records every choice and shows the user "the path you chose" at the end. That turns a one-time read into a multi-path exploration — and users naturally restart to try the other branch.

---

## Key learnings

**1. Conversational UI teaches sequence better than diagrams do.**
A reader can skip a box in a flowchart. They cannot skip a chapter in a story. The conversational format enforces the sequence of the PA workflow because the next dialogue line depends on the previous one.

**2. Two perspectives is the actual lesson, not a footnote.**
Most healthcare-education content I've seen picks one viewpoint and never reveals the other. The dual-takeaway structure of this simulator — patient lessons + system metrics, side by side — is what separates an explainer from an actual education tool. The lesson is *both perspectives are simultaneously true.*

**3. Real citations cost nothing and change credibility.**
Citing the AMA 2023 PA Physician Survey and naming specific industry benchmarks (1 in 5 denial rate, 40–65% appeal overturn rate, 7-day best-in-class resolution) turns the simulator from "fictional explainer" into "evidence-backed teaching tool." A healthcare-administration instructor could assign this and trust it.

**4. The append-only constraint produced cleaner code than I would have written without it.**
Forcing every element to be built via `createElement` made me factor each UI type into its own constructor function. That structure is more maintainable than the typical "string-template-and-innerHTML" approach. Sometimes the prompt's constraint is the architecture lesson.

**5. Dialogue is a content format, not just a UI choice.**
Writing this required thinking through what Rahul would actually ask and what Priya would actually answer — not just what the lesson plan said to cover. That forced the content to be tighter and more concrete. Conversational design is constraint-driven content design.

---

## What surprised me most

The simulator runs in ~31 KB of HTML — under the size of most documentation PDFs of the same topic — and teaches the PA workflow with more retention than the PDF would. The dialogue format isn't just engagement theater. It changes how the reader encodes the information. A patient who has "met" Rahul will remember "step therapy must be documented" longer than a patient who read it as a bullet point.

That's the underused leverage of conversational educational tools. The medium changes the memory.

---

## Narrative continuity with Day 26

- **Day 26** (Prior Authorization Workflow Simulator): showed the workflow as a **drag-and-drop game** with three swim lanes, four scenarios, and probabilistic outcomes. The user *operated* the workflow.
- **Day 27** (Prior Authorization Story Simulator): shows the same workflow as a **conversational story** between a patient and an operations specialist. The user *experiences* the workflow from inside.

Both teach the same domain — but via two completely different educational modalities. Day 26 is for someone who wants to understand the system mechanics. Day 27 is for someone who wants to understand what the workflow feels like from the patient's chair.

Stacking both formats — operate it AND experience it — is a more complete education than either alone.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
