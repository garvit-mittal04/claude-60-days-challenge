# Day 5 — Context Engineering

**Challenge:** #60DayClaudeChallenge
**Repository:** https://github.com/garvit-mittal04/claude-60-days-challenge
**Author:** Garvit Mittal
**Date:** June 4, 2026

---

## What is Context Engineering?

Context Engineering is the practice of providing relevant information, constraints, background, goals, and user-specific details to an AI before asking it to perform a task. The quality of context often matters more than the prompt itself — the same prompt with rich context produces a dramatically more useful output than the prompt alone.

---

## Prompt A — Without Context

```
Create a 30-day learning roadmap.
Include:
- Weekly milestones
- Daily tasks
- Resources
- Projects
- Final outcome
Make it practical and beginner-friendly.
```

> Note: Claude asked clarifying questions when I sent this. I skipped them, and Claude defaulted to a generic Python roadmap — exactly demonstrating the point: with no context, the model has to guess.

### Output A — Generic Python Roadmap (Claude's default guess)

# 30-Day Python Learning Roadmap

A practical, beginner-friendly plan. **Time commitment:** ~1–2 hours/day. **Outcome:** You'll be able to write Python scripts, work with data, build a small web app, and ship a portfolio project to GitHub.

## How to use this plan
- Do the daily task first (concept + short exercise).
- Use the listed resource as your primary reference for that day.
- Build the weekly project on the weekend (Days 6–7 of each week).
- Commit code to GitHub daily. Even one file. Streak matters.

## Week 1 — Python Fundamentals
**Milestone:** Write small scripts using variables, conditionals, loops, and functions. Run them from the terminal.

| Day | Topic | Daily task | Resource |
|---|---|---|---|
| 1 | Setup + Hello World | Install Python 3, VS Code. Run `print("Hello")`. Create GitHub repo `python-30days`. | python.org; VS Code docs |
| 2 | Variables & types | int, float, str, bool. Write a tip calculator. | Automate the Boring Stuff — Ch. 1 |
| 3 | Strings & input | input(), f-strings, slicing. Build a name formatter. | Real Python — Strings |
| 4 | Conditionals | if/elif/else. Number-guessing game (one round). | ATBS — Ch. 2 |
| 5 | Loops | for, while, range. Print a multiplication table. | Real Python — Loops |
| 6 | Functions | Parameters, return values, scope. Refactor Day 5. | ATBS — Ch. 3 |
| 7 | **Project** | CLI Number-Guessing Game with score tracking. Push to GitHub. | — |

## Week 2 — Data Structures & Files
**Milestone:** Manipulate lists, dicts, files. Handle errors.

| Day | Topic | Daily task |
|---|---|---|
| 8 | Lists | append, sort, list comprehensions. To-do list in memory. |
| 9 | Dictionaries & sets | Build a contact book. |
| 10 | Tuples & unpacking | Multi-return functions. |
| 11 | File I/O | open, with, count words in a .txt. |
| 12 | CSV & JSON | Convert JSON to CSV. |
| 13 | Exceptions | try/except/finally. |
| 14 | **Project** | Expense Tracker CLI with JSON storage. |

## Week 3 — Modules, OOP & Real-World Python
**Milestone:** Organize code into modules and classes. Use third-party packages.

| Day | Topic | Daily task |
|---|---|---|
| 15 | Modules & imports | os, pathlib, datetime tour. |
| 16 | pip & virtualenv | Install requests. Fetch a JSON API. |
| 17 | Classes & objects | Build a BankAccount class. |
| 18 | Inheritance | Extend BankAccount → SavingsAccount. |
| 19 | Comprehensions + lambdas | map, filter, 5 short problems. |
| 20 | Web APIs | Use requests, parse JSON. |
| 21 | **Project** | Weather CLI with forecast. |

## Week 4 — Applied Python: Web, Data, Shipping
**Milestone:** Build a small web app. Analyze a dataset. Deploy.

| Day | Topic | Daily task |
|---|---|---|
| 22 | Flask intro | Hello World web server. |
| 23 | Routes & templates | Add Jinja2 template. |
| 24 | Forms & POST | Echo user input. |
| 25 | pandas intro | Load CSV, head/info/describe. |
| 26 | Data wrangling | Filter, group by, matplotlib. |
| 27 | Git polish | READMEs, .gitignore, branches, PRs. |
| 28 | Deploy | Render/Railway free tier. |
| 29 | Capstone build | Start final project. |
| 30 | Capstone ship | README, demo video, post to LinkedIn. |

## Final Capstone Project (Days 29–30)
Pick one: Personal Expense Dashboard (Flask + pandas), Habit Tracker Web App (Flask + JSON), or Weather Dashboard (Flask + API).

## Core resources (free)
Automate the Boring Stuff, Real Python, python.org docs, CS50P, Exercism, pandas docs.

## Final outcome
A GitHub repo with 30 days of commits, 4 working projects (CLI game, expense tracker, weather CLI, deployed web app), and practical fluency in Python — enough to apply for junior dev roles or move on to specialization.

---

## Prompt B — With Context

```
Create a 30-day learning roadmap.

Context:
- Current Situation: Student
- Current Skills: Basic Python, comfortable with ChatGPT/Claude as a user
- Goal: Become proficient in Prompt Engineering and start building AI workflows
- Available Time: 2 hours per day
- Experience Level: Beginner
- Preferred Learning Style: Projects (hands-on)

Include:
- Weekly milestones
- Daily tasks
- Resources
- Projects
- Final outcome

Make it practical and beginner-friendly.
```

### Output B — Personalized Prompt Engineering Roadmap

# 30-Day Prompt Engineering & AI Workflows Roadmap

**For:** Beginner student with basic Python, 2 hrs/day, hands-on learner
**Goal:** Proficiency in prompt engineering + ability to build real AI workflows
**Time budget:** 60 hours total (≈2 hrs × 30 days)

## How to Use This Plan
Each day has ~30 min reading/learning + ~90 min hands-on building. Keep a `prompt-journal.md` file where you save every prompt that worked, every one that failed, and why.

**Setup before Day 1 (30 min):** Create accounts for Claude, ChatGPT, OpenAI API ($5 credit), Anthropic API ($5 credit). Install VS Code, Python 3.10+, `pip install anthropic openai python-dotenv`. Create GitHub repo `prompt-engineering-journey`.

## Week 1 — Prompting Fundamentals
**Milestone:** Write prompts that consistently produce the output you want, and understand *why* they work.

- **Day 1** — How LLMs work. Read Anthropic's "Intro to Prompting". Watch Karpathy's Intro to LLMs. Write 10 prompts, predict output before sending.
- **Day 2** — Clarity, specificity, structure. Rewrite 5 vague prompts 3 ways each.
- **Day 3** — Few-shot prompting. *Mini-project:* Sentiment classifier, zero-shot vs few-shot, test on 20 tweets.
- **Day 4** — Roles, personas, system prompts. Build a Q&A bot with 5 personas.
- **Day 5** — Chain-of-thought. *Mini-project:* 10 word problems with/without CoT.
- **Day 6** — Output formatting (JSON, XML, markdown). Recipe extractor from blog posts.
- **Day 7** — **Week 1 Project: Personal AI Tutor** (CLI teaching at ELI5 / intermediate / expert levels using Anthropic SDK).

## Week 2 — Advanced Prompting Techniques
**Milestone:** Handle complex tasks reliably with multi-step prompts and self-correction.

- **Day 8** — Prompt chaining (outline → draft → edit blog post generator).
- **Day 9** — Self-consistency & self-critique. Add critic prompt to Day 8.
- **Day 10** — ReAct pattern. *Mini-project:* Wikipedia research assistant.
- **Day 11** — Prompt templates & variables. Refactor Day 7 tutor.
- **Day 12** — Handling hallucinations. Citation-required QA bot.
- **Day 13** — Token economics. Build `cost_tracker.py` wrapper.
- **Day 14** — **Week 2 Project: Smart Email Triage** (categorize emails, draft replies, daily digest).

## Week 3 — Building AI Workflows
**Milestone:** Stitch prompts + tools + data into real, multi-step workflows.

- **Day 15** — Intro to LangChain (honest take: try it, skip if bloated).
- **Day 16** — Tool use / function calling. *Mini-project:* Calculator + weather API tool.
- **Day 17** — RAG part 1. Naive RAG with cosine similarity on embeddings.
- **Day 18** — RAG part 2. Upgrade to ChromaDB. Index a book or 50 articles.
- **Day 19** — Agents 101. Sketch (on paper) how 3 agents collaborate.
- **Day 20** — Build the agent from Day 19.
- **Day 21** — **Week 3 Project: Research Assistant Workflow** (Tavily API + agent that fetches, summarizes, synthesizes report with citations).

## Week 4 — Production, Evaluation, Capstone
**Milestone:** Ship something real, with tests, error handling, and quality measurement.

- **Day 22** — Evaluation. Build a 20-example eval set for Day 14 triage.
- **Day 23** — Iterate using evals. Goal: +15% accuracy.
- **Day 24** — Guardrails & safety. Try to break your own bot.
- **Day 25** — Streaming, async, UX. Add Streamlit UI to research assistant.
- **Day 26** — Capstone planning. Pick: Personal Knowledge Base agent / Code reviewer bot / Meeting notes → action items / Customer support triage.
- **Days 27–29** — Build the capstone (scaffolding → tools/RAG/UI → evals + polish + demo).
- **Day 30** — Ship it. GitHub README, demo video, LinkedIn post, share in r/PromptEngineering or Anthropic Discord.

## Core Resources
**Docs:** Anthropic Prompt Engineering Guide, OpenAI Prompt Engineering Guide, promptingguide.ai
**Courses:** Anthropic Courses (GitHub), DeepLearning.AI Prompt Engineering courses
**Reading:** Anthropic — Building Effective Agents, Lilian Weng on Prompt Engineering
**Communities:** r/PromptEngineering, Anthropic + OpenAI Discords

## What You'll Have on Day 30
- GitHub repo with **5+ working projects**: tutor CLI, email triage, research agent, capstone, plus mini-projects
- A `prompt-journal.md` with ~100 documented prompts and lessons
- Fluency with: few-shot, CoT, prompt chaining, tool use, RAG, evals, agents
- A shipped capstone with demo video
- Confidence to estimate, build, and ship AI workflows from scratch

## Honest Advice
- 30 min reading + 90 min building beats 2 hrs of tutorials every time.
- Save every prompt. Your journal becomes more valuable than any course.
- Steal shamelessly from open-source agents (Aider, Continue, Cline).
- Ship on Day 30 even if it's ugly. A shipped ugly thing teaches more than a polished thing you never finished.

---

## Comparison

### 1. Which roadmap feels more personalized?

**Prompt B**, by a huge margin. Output A is a generic Python plan that Claude essentially guessed at after I skipped its clarifying questions — it doesn't know I'm a student, doesn't know I already have basic Python, doesn't know I care about prompt engineering or AI workflows. Output B addresses each of those directly: it skips Python basics I already know, picks projects relevant to my actual goal (tutor bot, email triage, research agent), and frames the entire plan around the 2-hour daily budget with a "30 min reading + 90 min building" split.

### 2. Which roadmap would you actually follow?

**Prompt B**, without hesitation. Output A would be wasted weeks for me — I'd spend Week 1 relearning variables and loops. Output B starts where I actually am, names specific docs/papers/APIs to use, and ends each week with a concrete shipped project. The "honest advice" section even tells me what to skip if I dislike it (e.g., LangChain). That kind of personalized guidance only emerges when the model knows who it's talking to.

### 3. What role did context play in improving the result?

Context turned a template into a tailored plan. Specifically:
- **Goal** ("Prompt Engineering and AI workflows") swapped the entire curriculum from Python basics to prompt engineering — a fundamentally different domain.
- **Current skills** ("basic Python, ChatGPT user") let it skip prerequisites and jump straight into SDK calls, chaining, and agents.
- **Time budget** ("2 hrs/day") shaped the daily task size to be realistic — and even structured *how* to split that time (reading vs building).
- **Learning style** ("Projects") meant every single day has a hands-on task and every week ends with a shipped artifact.
- **Situation** ("Student") shaped project ideas (tutor, research assistant) and the framing of the final outcome (portfolio, internships).

Without these, Claude defaulted to a generic Python roadmap — useful for *someone*, but not for *me*. With them, it became something close to a personal coach.

---

## Key Learnings

1. **Without context, Claude either asks or guesses.** I skipped the clarifying questions and got a Python roadmap I didn't ask for. The model can't read your mind — whatever you don't tell it, it has to fill in.
2. **Context > prompt wording.** Prompt B isn't longer because of fancier language — it's longer because of *facts about me*. Those facts did 10× more work than any clever phrasing could.
3. **Structured context beats prose context.** Labeled fields (`Goal:`, `Current Skills:`, `Available Time:`) made it trivial for Claude to pick the right curriculum, skill level, and pacing.
4. **Personalization makes a plan followable.** The generic roadmap is technically complete but emotionally inert — I wouldn't open it tomorrow. The personalized one feels like it was written *for* me, which is the entire reason I'd actually use it.
5. **This generalizes everywhere.** The same principle applies to using Claude for code reviews, writing, research, debugging — always front-load relevant context. It's the single highest-leverage habit in working with AI.

---

## Screenshots

Screenshots of both Claude conversations are saved in this folder as `prompt-a-output.png` and `prompt-b-output.png`.

---

## Resources Used

- [Anthropic — Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Prompting Guide — Context Engineering Guide](https://www.promptingguide.ai/guides/context-engineering-guide)
- [LangChain — Context Engineering for Agents](https://www.langchain.com/blog/context-engineering-for-agents)
