# Implementation Plan — Self-Learning OS

> This is the working planning document. It captures the **discussion**, **decisions**, and **rationale** behind every architectural choice — not just the spec. New contributors should read this before touching any SKILL.md.

## Why we're building this

### The user pain

A real conversation with a Claude user (the project originator) surfaced a common pattern:

- Wants to learn Computer Vision
- Holds a 9-hour AI engineering job (10 AM – 7 PM)
- Runs a side business in parallel
- Has family commitments
- Has tried "learn X in 30 days" plans before and bounced off

This is not a Computer Vision problem. It's a **learning-systems** problem — the same one Jorge has in Chapter 2 of the book. People with strong intent and capable minds fail at self-learning because they skip the foundation and pick low-utility techniques.

### The hypothesis

A **structured, gated, evidence-backed coaching loop** delivered through Claude can solve this for anyone with any topic — because the constraint is not subject-matter expertise (Claude has plenty) but **adherence to a learning process**.

### Why a skill bundle and not a SaaS product

- **Distribution:** SKILL.md is now an open standard adopted by 40+ AI clients. Build once, run in Claude Code, claude.ai, OpenAI Codex CLI, Cursor, etc.
- **No backend cost:** state lives in the user's local files. Zero infra.
- **Forkable:** anyone can adapt the bundle to their own teaching philosophy.
- **Quanta AI Labs strategic fit:** becomes a portfolio piece, a QuantaSkool course, and an open-source credibility play simultaneously.

## The book's framework (what we're encoding)

From Peter Hollins, *The Science of Self-Learning*:

### Chapter 1 — Principles
- Art vs science classification → topic-classifier skill
- The Learning Success Pyramid (3 blocks) → confidence-audit + self-management-setup + (gating)
- Intrinsic motivation: Autonomy / Mastery / Purpose → learning-planner motivation anchors

### Chapter 2 — Interaction with Information
- SQ3R method → sq3r-session skill (v1.1)
- Cornell Notes → cornell-notes skill (v1.1)
- Self-explanation / elaborative interrogation → embedded in feynman-checker
- Feynman Technique → feynman-checker skill (v1)

### Chapter 3 — Read Faster and Retain More
- Skimming, eye training, focus → embedded in sq3r-session guidance
- Mortimer Adler's four levels → embedded in learning-planner survey phase

### Chapter 4 — Skills and Habits
- Benjamin Franklin's 13 virtues and schedule → franklin-scheduler skill
- SMART goals → smart-goal-setter skill
- Researching from scratch (5 steps) → embedded in learning-planner
- Self-discipline + confusion endurance → confusion-endurance skill (v1.1)

## What the research evidence changes

Several findings cut against parts of the book — we side with the evidence.

See [EVIDENCE.md](./EVIDENCE.md) for full citations. Key implications:

1. **Highlighting, rereading, and pure summarization are low-utility** (Dunlosky 2013). The book's Cornell notes are valuable only because of the recall-question column, not the summary. Our Cornell skill (v1.1) will enforce a `recall_questions` field.
2. **Spaced repetition intervals matter.** Default to 1d / 3d / 7d / 21d (Cepeda 2006, FSRS-style). Don't let the user invent arbitrary review cadences.
3. **Self-explanation (Feynman) has g ≈ 0.55** (Bisra 2018 meta-analysis). This is the **highest-value single skill** in our bundle — promote it to v1 even though it's a "Chapter 2 technique" not a foundational one.
4. **SQ3R's effect comes from Recite + Review, not Survey.** Our SQ3R skill (v1.1) must enforce the retrieval steps; surveying alone is low-utility opt-in.
5. **Structured multi-agent handoffs reduce hallucination** (MetaGPT, ICLR 2024). Each skill emits a strict JSON artifact, validated by the coordinator before chaining.

## Architectural decisions (summary)

Full ADRs live in [ARCHITECTURE.md](./ARCHITECTURE.md). Headlines:

| # | Decision | Rationale |
|---|---|---|
| 001 | Ship as a **plugin bundle**, not a single mega-skill | Matches anthropics/skills pattern (`docx`, `pdf`, `pptx`, `xlsx` are split) |
| 002 | **One skill per learning framework**, not per micro-task | Split by domain boundary, unify by user intent |
| 003 | **State lives in plain markdown files** under `~/.self-learning-os/<topic>/` | MCP is for external integrations, not local state |
| 004 | **Gated phase progression** (A → B → C, no skipping) | The book's central claim; pyramid blocks must be addressed in order |
| 005 | **JSON artifacts** for inter-skill handoff | MetaGPT-style structured composition |
| 006 | **Evidence-calibrated defaults** | Defer to Dunlosky 2013 when the book and the meta-analyses disagree |
| 007 | **Progressive disclosure** — SKILL.md ≤500 lines, push detail to `references/` and logic to `scripts/` | anthropics/skills authoring rule |

## v1 scope (12 hours of work, weekends 1–4)

Ship eight files:

1. `skills/self-learning-coach/SKILL.md` — coordinator + state router
2. `skills/topic-classifier/SKILL.md` — art vs science
3. `skills/confidence-audit/SKILL.md` — Pyramid Block 1
4. `skills/self-management-setup/SKILL.md` — Pyramid Block 2
5. `skills/smart-goal-setter/SKILL.md`
6. `skills/learning-planner/SKILL.md` + `references/dunlosky_utility_table.md`
7. `skills/franklin-scheduler/SKILL.md`
8. `skills/feynman-checker/SKILL.md`

**Acceptance test:** Run the originator's CV prompt end-to-end:
> "I want to learn computer vision. I work 10–7, have family time 7–8:30, run a side business."
>
> Expected: coach routes through topic-classifier → confidence-audit → self-management-setup → smart-goal-setter → learning-planner → franklin-scheduler. Produces a plan equivalent to or better than the hand-written one in the original Claude conversation, with persistence to `~/.self-learning-os/computer-vision/`.

## v1.1 scope (10 hours, weekends 5–8)

- `skills/sq3r-session/SKILL.md` (with enforced Recite + Review)
- `skills/cornell-notes/SKILL.md` (with mandatory `recall_questions`)
- `skills/weekly-review/SKILL.md`
- `skills/confusion-endurance/SKILL.md`
- Description-hardening eval loop (port `skill-creator/run_loop.py` for each skill)

## v1.2 scope (when there are real users)

- Optional MCP server for Anki / Obsidian export
- Optional `scripts/spaced_intervals.py` integration with FSRS via py-fsrs
- Localization (book is English-only)

## Risks and mitigations

| Risk | Mitigation |
|---|---|
| Description-trigger collisions (e.g., `sq3r-session` and `cornell-notes` fire on same prompt) | Run skill-creator-style eval (20 trigger + 20 negative queries) per skill before shipping |
| Generic output (every session feels new) | State persistence in `~/.self-learning-os/<topic>/`; coordinator reads state first |
| Scope creep from the book (12+ frameworks) | Ship 8 in v1; defer the rest |
| The book is one source — readers may push back on the evidence-based deviations | EVIDENCE.md is a defensible explanation; positioning is "calibrated against the science of learning" |
| User abandons mid-plan | Confusion-endurance skill (v1.1) is the on-demand intervention; weekly-review keeps cadence |

## Open questions

- **Subagent vs skill for `feynman-checker`?** Currently a skill. If it becomes context-heavy (deep adversarial questioning over 20+ turns), promote to subagent in v1.1.
- **Should `topic-classifier` block on ambiguous topics?** ("I want to learn music" — performance is art, theory is science.) Lean: surface the ambiguity, let the user split into multiple topics.
- **Hard-coded English vs i18n from day one?** Lean: English-only for v1, add language field to state.md so v1.2 i18n is non-breaking.
- **Public skill marketplace listings?** anthropics/skills accepts PRs only for canonical-quality skills. Better path: skillsmp.com, tonsofskills.com, hesreallyhim/awesome-claude-code.

## Timeline (target)

| Week | Deliverable | Hours |
|---|---|---|
| 1 (this) | Scaffold (this PR): README, plan, architecture, evidence, plugin.json, 8 v1 SKILL.md files | ~5 |
| 2 | End-to-end test with CV prompt; iterate on coordinator routing | ~3 |
| 3 | Description hardening + 20-query eval per skill | ~4 |
| 4 | v1 release + Quanta blog post + QuantaSkool module 1 draft | ~2 |
| 5–8 | v1.1 (cornell, sq3r, weekly-review, confusion-endurance) | ~10 |

Total: ~24h across 8 weeks. Fits the originator's 9 PM weekday windows + Sunday review sessions without touching job or family time.
