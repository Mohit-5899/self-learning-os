---
name: learning-planner
description: Use this skill after smart-goal-setter, to produce the topic's full 3-month learning plan (weekly planks, week-1 survey list of verified-current resources, elaborative-interrogation question bank, motivation anchors) — and render it as a styled HTML plan dashboard the user can view, save, or print. Triggers when ~/.self-learning-os/<topic>/smart-goal.md exists but plan.md does not. Uses WebSearch and WebFetch heavily to verify every resource is alive and current at execution time; does not rely on training-data lists. Outputs HTML (user-facing), markdown + JSON (state). Required before franklin-scheduler. Do NOT use for picking the topic, for scheduling into days/times (that's franklin-scheduler), or for users without a SMART goal.
---

# Learning Planner (Phase B — Planning)

You produce the **full 3-month learning plan** — what to study, in what order, from which (current, verified) resources, with built-in elaborative-interrogation prompts and motivation anchors.

This is the **most web-search-intensive skill in the bundle**. Resource lists go stale fast; you generate fresh ones at execution time.

## Why this matters

From *The Science of Self-Learning*, Ch.4 — Researching from Scratch (5 steps):

1. **Gather information** — wide net, no filtering yet
2. **Filter sources** — separate signal from noise
3. **Look for patterns and overlap** — what appears across multiple authoritative sources
4. **Seek dissenting opinions** — what the contrarians say
5. **Put it all together** — synthesized understanding

You operationalize steps 1–3 by surveying the field with `WebSearch` and `WebFetch`. The user then does step 4–5 during Phase C execution.

And from Ch.1 — Intrinsic Motivation (Daniel Pink's 3 factors):

- **Autonomy** — you control your own learning
- **Mastery** — you can see yourself improving
- **Purpose** — your effort contributes beyond yourself

These three become the motivation anchors at the end of the plan.

## When to invoke

The coordinator calls you when `smart-goal.md` exists but `plan.md` does not.

## Workflow

### Step 1 — Read prior context

Read all prior artifacts:
- `classification.md` — topic type, adjustments needed
- `confidence.md` — existing knowledge (informs starting depth), progress markers (informs end state)
- `self-management.md` — weekly hours available
- `smart-goal.md` — the goal, milestones, pivot rule

### Step 2 — Identify the 4–6 core planks

A "plank" is a rotating focus area. The user picks one plank per day, rotating across the week — not all planks every day. This matches the book's Franklin model (one virtue per week, rotating).

For Computer Vision, planks might be:
1. **Math foundation** — linear algebra, calculus refresher for CV
2. **Image processing** — OpenCV fundamentals, filters, convolutions, edge detection
3. **Deep learning for vision** — CNNs, ResNet, transfer learning
4. **Object detection** — YOLO, SSD architectures
5. **Hands-on project** — building toward the SMART-goal deliverable
6. **Review** — Cornell summaries + Feynman checks + flashcards

For Spanish, planks might be:
1. **Grammar foundation**
2. **Vocabulary acquisition**
3. **Listening comprehension**
4. **Speaking production**
5. **Cultural / contextual immersion**
6. **Review**

Pick 4–6. Each plank should be a coherent area, not a tiny task.

### Step 3 — Survey the field (heavy web use)

For each plank, invoke `WebSearch` to find current best-of-breed resources. Goals:

- **Primary sources first** — official docs, university course pages, original papers
- **Current** — last commit / last update within 2 years for fast-moving fields (ML, JS frameworks); 5 years for slower fields (math, classical languages)
- **Free or affordable** — prioritize free unless the user signaled they'll pay
- **Multiple modalities** — book + course + practice + community

Run queries like:
- `"best [topic] tutorial 2026"` then verify currency with `WebFetch`
- `"[topic] github stars sorted"` for active repos
- `"[topic] arxiv 2025"` for recent papers
- Official docs for any framework named

For each candidate resource, `WebFetch` to confirm it's alive and assess freshness. Discard anything that 404s or hasn't updated in too long.

**The book's Ch.3 Mortimer Adler four levels of reading** maps directly:

- **Elementary** — beginner tutorials (week 1–2)
- **Inspectional** — surveys / overviews (week 1 survey list)
- **Analytical** — deep single-book engagement (weeks 3–10)
- **Syntopical** — cross-source synthesis (weeks 10–12)

Tag each resource with its appropriate level.

### Step 4 — Build the week-1 "Survey phase" list

The book is explicit (Ch.2 SQ3R "Survey"): before deep reading, the user must **map the field**. Build a focused week-1 list of 3–5 resources to **survey only** (read tables of contents, skim headings, watch the first lecture, browse the docs index). No deep engagement yet — orientation only.

Example for Computer Vision:
- PyImageSearch — read category headings only (verify current via WebFetch)
- fast.ai Practical Deep Learning Part 1 — read syllabus + watch lecture 1 intro
- OpenCV docs — read top-level index, identify which modules matter
- One foundational textbook — read the table of contents only
- One arXiv survey paper — read abstract + section headings only

Each entry tagged with **the specific survey task**, not "read it."

### Step 5 — Generate the question bank

Per the book (Ch.2 — Self-Explanation / Elaborative Interrogation), great learners ask "why" and "how" relentlessly. Generate 10–15 questions the user should be able to answer by the end of 3 months. These guide reading — the user hunts for answers, not passively absorbs.

Example for Computer Vision:
- Why does a convolution work better than a fully connected layer for images?
- How does YOLO achieve real-time detection while two-stage detectors don't?
- What exactly is the vanishing gradient problem and why does ResNet solve it?
- How does an anchor box work, and why are multiple scales needed?
- Why does batch normalization improve training stability?
- ...

Mix "what" (≤30%), "why" (≥40%), "how" (≥30%).

### Step 6 — Build motivation anchors (Pink's three factors)

Personalize from the user's own context (job, business, family — pulled from `confidence.md` and any earlier conversation).

| Factor | Template | Example for the originator |
|---|---|---|
| Autonomy | "I am learning this on my own terms, on my own schedule, because I chose to." | "I'm learning CV because I chose to extend my AI engineering — no university or boss told me to." |
| Mastery | "Every week I can verify concrete improvement against my plan." | "Each week, my day job AI work benefits from sharper vision understanding." |
| Purpose | "This contributes to something beyond me." | "This becomes a Quanta AI Labs offering and a QuantaSkool module." |

Three sentences, specific to this user. Not generic.

### Step 7 — Apply evidence-calibrated technique defaults

Reference [Dunlosky utility table](./references/dunlosky_utility_table.md). Mark each plank with the **default technique** evidence supports:

- **Practice testing** (HIGH utility) — for any plank with verifiable problems (math, code, language drills)
- **Distributed practice** (HIGH utility) — applied as the 1d/3d/7d/21d cadence in `franklin-scheduler`
- **Self-explanation / Feynman** (g≈0.55) — weekly check per plank
- **Elaborative interrogation** — the question bank above

Do **not** default any plank to highlighting, rereading, or pure summarization.

### Step 8 — Surface external-feedback requirements

If `classification.md` flagged `external_feedback_required: true`, integrate the user's plan for getting feedback — critique group, mentor, peer reviewer, public posting — into the plan. Do not leave this as homework.

### Step 9 — Write the artifacts (HTML + markdown + JSON)

Produce three outputs.

**HTML plan dashboard (user-facing).** Use template at [references/learning_plan_html_template.html](./references/learning_plan_html_template.html). Substitute: `TOPIC`, `SMART_GOAL_STATEMENT`, `DEADLINE_DATE`, `WEEKLY_HOURS`, `TOTAL_HOURS`, `SUITABILITY`, per-plank card content (`PLANK_N_NAME`, `PLANK_N_TOPICS`, `PLANK_N_TECH_N`), per-survey-item (`SURVEY_N_TITLE`, `SURVEY_N_DATE`, `SURVEY_N_TASK`), per-resource entry (`RES_N_URL`, `RES_N_TITLE`, `RES_N_FRESHNESS` with class `freshness-current` / `freshness-aging` / `freshness-stale`), each `QUESTION_N`, motivation anchors (`AUTONOMY_ANCHOR`, `MASTERY_ANCHOR`, `PURPOSE_ANCHOR`), `EXTERNAL_FEEDBACK_BLOCK` (omit the whole div if `external_feedback_required` is false), `GENERATED_DATE`.

Emit as fenced HTML code block in chat — renders as the visual plan dashboard in artifact-supporting clients. Also save to `~/.self-learning-os/<topic>/plan.html` if file-write is available.

**Markdown + JSON state.** Save to `~/.self-learning-os/<topic>/plan.md`. Long form, with the JSON artifact at the bottom:

```markdown
# Learning Plan: <topic>

## Planks (rotating focus areas)
1. **<Plank name>** — <description, default technique>
2. ...

## Week 1 — Survey phase
Resources to survey only (no deep engagement yet):
- **<URL>** — <verified current YYYY-MM-DD> — <specific survey task>
- ...

## Resource library (full plan, by plank)
### Plank 1: <name>
- <URL> — <verified date> — <level: elementary/inspectional/analytical/syntopical>
- ...

## Question bank
1. <why/how question>
...

## Motivation anchors
- **Autonomy:** <user-specific>
- **Mastery:** <user-specific>
- **Purpose:** <user-specific>

## External feedback plan (if required)
<community, mentor, critique loop>

---

```json
{
  "planks": [
    {"name": "...", "topics": [...], "default_techniques": ["practice_testing", "self_explanation"], "weight": 0.2}
  ],
  "week_1_survey": [
    {"url": "...", "verified_date": "YYYY-MM-DD", "task": "...", "level": "inspectional"}
  ],
  "resource_library": [
    {"plank": "...", "url": "...", "verified_date": "YYYY-MM-DD", "level": "analytical", "freshness_flag": "current|aging|stale"}
  ],
  "question_bank": ["..."],
  "motivation_anchors": {"autonomy": "...", "mastery": "...", "purpose": "..."},
  "external_feedback_plan": "..."
}
```
```

### Step 10 — Hand back

Confirm. Coordinator routes to `franklin-scheduler`.

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **heavy use, mandatory**.

This is the skill where stale recommendations cause the most damage. Web checks are not optional.

- **Before drafting any resource list:** survey the current ecosystem with 5–10 targeted queries.
- **Before including any specific URL:** `WebFetch` to confirm it's alive and recently updated.
- **Tag every resource** with a `verified_date` (today's date) and a `freshness_flag` (current / aging / stale).
- **For arXiv papers:** prefer 2023+ in fast-moving fields. Older work is fine for foundational concepts.
- **For GitHub repos:** check stars + last commit date. Avoid abandoned forks.

If web tools are unavailable in this client, **flag this in the artifact**:

```json
{
  "_web_tools_available": false,
  "_warning": "Resources are from training data through <cutoff>; verify URLs and currency before committing time."
}
```

Then proceed with the best training-data resources you can list, clearly labeled.

## What to avoid

- **Generic resource lists** ("read the textbook"). Name specific URLs.
- **Skipping the survey phase.** Week 1 is mapping, not deep work — do not let the user start "real" learning week 1.
- **More than 6 planks.** Cognitive overload. Compress.
- **Too few planks** (1–2). Insufficient rotation. Expand or split.
- **Generic motivation anchors.** Must reference the user's specific job, business, family situation.
- **Forgetting the Dunlosky defaults.** Each plank gets evidence-backed technique tags.

## What "good" looks like

A completed `plan.md` should make the user say: *"I know what I'm doing each week, where to find the materials, what questions I'm hunting for, and exactly why this matters to me."*

## References

- *The Science of Self-Learning*, Ch.2 (SQ3R Survey + question generation), Ch.3 (Adler four levels), Ch.4 (researching from scratch)
- [references/dunlosky_utility_table.md](./references/dunlosky_utility_table.md) — evidence-based technique defaults
- [EVIDENCE.md](../../EVIDENCE.md) — full citations
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-006 (evidence-calibrated defaults), ADR-009 (web search)
