# Day 33 — Media Integrity Analyzer

**Project:** `media-integrity-analyzer.html` — guided-discovery media literacy simulator
**Challenge:** #60DayClaudeChallenge — Day 33
**Type:** Single-file vanilla HTML/CSS/JavaScript application. No frameworks, no CDN, no images, no APIs.

---

## What I built

An interactive lesson that teaches media literacy through *guided discovery* — the user observes a piece of media, marks what they think is misleading, and only then sees what a media analyst would flag. It's designed to feel like a lesson, not a test.

Two challenges, both with fictional but realistic content:

**Challenge 1 — Headline Detective.** A fictional news headline and matching article body appear on-screen in an editorial "newspaper card" layout. The user reads both, decides whether they would click the headline (Yes / Maybe / No), and taps the words or phrases they consider exaggerated or misleading. The reveal shows the Headline Accuracy Score (0–100%), a side-by-side comparison of the claims vs. what the article body actually says, a fair rewritten headline, and a plain-English key takeaway.

**Challenge 2 — Emotion Detector.** A fictional social media post appears with a handle, body text, and engagement stats. The user picks how it made them feel from five options (angry / afraid / curious / skeptical / unmoved), then taps the specific words that produced that feeling. The reveal shows the intended target audience, the intended emotional response, the manipulation technique in plain language, a neutral rewrite, and the key takeaway.

Between the two challenges, four live Media Integrity metrics update at the top of the screen: Headline Accuracy, Source Reliability, Emotional Manipulation, and Audience Targeting.

At the end, a Media Integrity Dashboard displays the user's overall score, what they learned, the single biggest red flag from today's session, and three practical media literacy habits calibrated to make the skills automatic.

---

## The guided-discovery pattern (why it's different from a quiz)

Most media literacy tools work like tests. They show you a headline, ask you if it's real or fake, and grade you. That format works for measurement — it doesn't work for learning.

Guided discovery reverses the order:

1. **Explain the concept before the exercise** — "headlines are optimized for the click, not the truth." Two sentences. The user reads this *before* they see the headline.
2. **Show the artifact** — the headline + article body.
3. **Ask the user to observe first, decide second, mark third** — Would you click? What feels off?
4. **Reveal after commitment** — once the user has made a call, show the analysis. Now they can compare their intuition to a trained one.

That third step is what turns a test into a lesson. The user has to commit to a position before seeing the "right answer," which means the reveal actually teaches them something about their own instincts.

---

## The two challenges, in detail

### Challenge 1 · Headline Detective

The content pool includes three headline+article pairs that model real-world manipulation patterns:

- **Wellness spam** — "Doctors HATE This 1 Simple Morning Habit That Melts Belly Fat Overnight." Article body reveals the source is a supplement company landing page and the "study" involved 12 mice. Accuracy score: 12%.
- **Political outrage** — "City Council SHOCKED As Local School Board Secretly Approves Radical New Curriculum." Article body reveals the vote happened at a public meeting after 6 weeks of public comment. Accuracy score: 26%.
- **Financial fear** — "Housing Market COLLAPSES: Experts Warn 40% Crash Coming In Next 60 Days." Article body reveals only one "expert" is named, they run a subscription newsletter, and the 40% figure appears nowhere in official data. Accuracy score: 8%.

Each headline includes 3–4 clickable misleading phrases. The user taps them; the analyzer scores how many of the true manipulation phrases they caught. The reveal shows every claim vs. what the article body actually reports.

### Challenge 2 · Emotion Detector

Three social media posts, each modeling a distinct manipulation technique:

- **Wellness Coach** using the "which side are you on?" identity binary — turns a purchase into a moral choice.
- **News aggregator** using ambiguity as bait — "ROBBED by ONE group of people" that lets every reader project their own villain.
- **Poll citation without source** — "87% of parents say" with no linked survey, no sample size, no methodology.

Each post ships with a target audience description, the intended emotional response, the specific manipulation technique in plain English, tagged emotional phrases, and a neutral rewrite that models what the same content would look like without the emotional loading.

---

## Live Media Integrity metrics

Four bars update at the top of the screen after each challenge:

| Metric | What it captures |
|---|---|
| **Headline Accuracy** | How close the fictional headline was to the underlying article body |
| **Source Reliability** | Whether the source is named, credentialed, and independent |
| **Emotional Manipulation** | How high the emotional loading of the social post is |
| **Audience Targeting** | How narrowly the post is aimed at a specific psychographic segment |

Bars animate on every reveal. The metrics contextualize the individual challenges — the user sees that a headline with an accuracy of 12% isn't just "bad wording," it's a structural pattern.

---

## Tech notes

- **Single HTML file.** ~48 KB. Pure vanilla CSS + JavaScript. No frameworks, no CDN, no images, no external assets, no APIs.
- **Five color themes** including Claude Orange (default), Indigo Night, Teal Studio, Rose Editorial, and Paper Warm. Switching themes swaps a set of CSS variables — no re-render needed.
- **Editorial typography** — headlines in Georgia/New York serif to match a real news feel, UI labels in system sans-serif for contrast.
- **Interactive highlighting.** Each headline chunk and each emotional phrase is a clickable `<span>` with a marker underline animation. Users can toggle marks on and off.
- **Six-stage state machine** — welcome → challenge 1 → reveal 1 → challenge 2 → reveal 2 → dashboard. Progress bar at the top animates smoothly between stages.
- **Randomized content on Replay.** Every playthrough surfaces a different headline and different social post.
- **Computed final score** using a weighted blend of the user's own detection performance and the manipulation intensity of what they saw. Composite scores map to four bands: Sharp / Solid / Developing / Early Stage.
- **Personalized red flag callout** — the dashboard names the specific headline or post that had the worst score, rather than generic feedback.
- **Editorial dark UI** with smooth `fadeInUp` animations, gradient buttons, hover effects, and a warm-cream news card that visually breaks from the dark background to look like actual print.

---

## What this exercise demonstrates

### 1. Media literacy is a set of small, repeatable patterns

The two challenges cover the same underlying pattern from two angles: someone is trying to move you emotionally, and the way they do it is by loading specific words. Once you learn to notice the words, you can't unsee them — in a wellness ad, a political post, a stock tip, or a news headline. The content changes; the pattern doesn't.

### 2. Guided discovery is a stronger teacher than a quiz

Every stage of this simulator forces the user to commit to a reading of the media *before* they see the analysis. That single design choice — explain the concept, then observe, then commit, then reveal — is why users leave with an internal instinct they didn't have when they started. A quiz would have measured what they already knew. This teaches them to notice something new.

### 3. Fictional teaching examples work better than real ones

Every headline and post in the simulator is fictional. That choice was deliberate. Real examples come with political baggage, brand associations, and news cycles that distract from the pattern. Fictional but realistic examples let the user focus on the structural technique instead of arguing about the specific real-world case. This is the same reasoning behind case-study teaching in MBA programs.

### 4. The dashboard should teach, not just score

The final dashboard's most valuable section is not the overall score — it's the three habits. Naming the specific behavior change ("read the article body before the headline," "name the word that made you feel that way before sharing," "check for one named person when you see 'experts say'") gives the user something they can actually take with them. A score without a habit is just a number.

---

## Key learnings

**1. Editorial UI signals seriousness in a way a generic dark card doesn't.**
The warm-cream news card and Georgia headline font are the single most important design choice in the simulator. They convince the user, in half a second, that this is a media literacy tool that respects journalism — not another social media game.

**2. Clickable phrases beat multiple-choice quizzes for teaching attention.**
When the user has to *physically tap* the word "SHOCKED" to mark it as manipulation, they encode that word into memory. Multiple-choice questions don't create that same trace.

**3. Three carefully-crafted headlines beat ten quick ones.**
Each headline in the pool has 3–4 layered manipulation techniques (unnamed authority + emotional verb + false urgency + implied causation). One playthrough teaches more patterns than ten generic quiz questions would.

**4. The reveal is where the teaching happens.**
The user does the observation. The reveal does the teaching. The gap between what they marked and what the analyst would mark is the specific lesson each user needs — different for every user, but personalized because the reveal has ground-truth to compare against.

---

## Narrative continuity

- **Day 26–28** — healthcare workflow simulators
- **Day 29–31** — supply chain crisis simulators
- **Day 32** — marketing strategy simulator
- **Day 33 — media literacy analyzer**

The domain keeps changing. The pedagogy — teach-before-decide, guided discovery, personalized reveals — stays constant. That's the actual meta-lesson of the challenge: educational simulators generalize across domains as long as you hold the teaching architecture stable and swap the content.

---

## Tags

`#60DayClaudeChallenge` `@Anthropic` `@ABTalksOnAI` `@AnilBajpai`
