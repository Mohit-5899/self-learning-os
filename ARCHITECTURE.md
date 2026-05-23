# Architecture Decision Records

> Each ADR documents one architectural choice: the **context**, the **decision**, the **consequences**, and the **evidence** behind it. ADRs are append-only — if a decision changes, write a new ADR superseding the old one.

---

## ADR-001 — Ship as a plugin bundle, not a single mega-skill

**Context.** We could write the entire framework into one giant `SKILL.md`. It would be one install, one description-match target, one file to maintain.

**Decision.** Ship as a Claude Code plugin bundling **eleven** distinct skills (eight in v1, three in v1.1) under `skills/` + a `.claude-plugin/plugin.json` manifest.

**Consequences.**
- ✅ Each skill stays under the 500-line / 5000-token soft limit recommended by `agentskills.io/specification`
- ✅ The coordinator (`self-learning-coach`) can compose specialists deterministically
- ✅ Updating one framework (e.g., Cornell) doesn't risk breaking another (e.g., Feynman)
- ❌ More description-match targets means more risk of trigger collisions — mitigated by ADR-005 (coordinator gating)
- ❌ More files to install — mitigated by plugin packaging

**Evidence.** The canonical `anthropics/skills` repository splits document handling into four separate skills (`docx`, `pdf`, `pptx`, `xlsx`) rather than one "office-files" mega-skill, because each has distinct libraries and gotchas. The agentskills best-practices doc states: *"A skill for querying a database and formatting the results may be one coherent unit, while a skill that also covers database administration is probably trying to do too much."*

---

## ADR-002 — One skill per learning framework, not per micro-task

**Context.** Within the bundle, granularity could go further: one skill per micro-task (e.g., `survey-step`, `question-step`, `read-step`, `recite-step`, `review-step` for SQ3R alone). Or it could go coarser: one skill per phase (`phase-a`, `phase-b`, `phase-c`).

**Decision.** Granularity = **one framework per skill**. SQ3R is one skill. Cornell is one skill. Feynman is one skill. Franklin scheduling is one skill.

**Consequences.**
- ✅ Maps cleanly to the book's chapters and the user's mental model
- ✅ Each skill has a coherent JSON artifact schema (see ADR-005)
- ✅ Sub-steps within a framework live inside the same SKILL.md or its `references/`
- ❌ Some skills will approach the 500-line limit — at that point split into framework + `references/<framework>_deep.md`

**Evidence.** Same source as ADR-001 — split by tool/library/format boundary, unify by user intent. A learning framework is a coherent unit; a single retrieval step is not.

---

## ADR-003 — State lives in plain markdown files, not an MCP server

**Context.** Learning is inherently stateful. A user studies CV for 6 weeks; the coach on week 6 must know what happened in weeks 1–5. Three options were considered:

1. **Stateless** — regenerate the plan every session. Rejected: defeats the whole point.
2. **MCP server with SQLite** — robust, queryable, supports future integrations (Anki, calendar).
3. **Plain markdown files** in a known per-user directory.

**Decision.** State as plain markdown under `~/.self-learning-os/<topic>/`. Skills read and write these files via the model's existing file tools (Read/Write/Edit).

**Consequences.**
- ✅ Zero infrastructure for v1 — users own their data as portable files
- ✅ Inspectable / editable by the user without any tooling
- ✅ Trivially backed up to git, iCloud, Dropbox
- ✅ Works in any Claude client that has file access (Claude Code, claude.ai with Files, Codex CLI)
- ❌ No queryable index — fine for v1 (one topic at a time), revisit at multi-topic scale
- ❌ No automatic spaced-repetition due-queue — deferred to v1.2 MCP

**Evidence.** From the architecture-research agent's findings, paraphrasing Anthropic docs: *"Skills teach the model how. MCP gives the model new tools. Subagents give the model another worker."* Local state files are not "new tools" — they're the model's existing capability. MCP would be over-engineered.

---

## ADR-004 — Gated phase progression (A → B → C, no skipping)

**Context.** The book's central claim (Ch.1) is that information first hits the brain's emotional center, then the prefrontal cortex, then the hippocampus. Skipping the lower blocks (confidence + self-management) makes learning neurologically impossible — even with great content.

**Decision.** The coordinator skill (`self-learning-coach`) refuses to route to a Phase-B or Phase-C skill until Phase A is complete. State files record which phase the user is in.

**Phase gates:**

| Phase | Gate (must exist in state) | Skills available |
|---|---|---|
| A | (entry — nothing required) | topic-classifier, confidence-audit, self-management-setup |
| B | `classification.md`, `confidence.md`, `self-management.md` all present | smart-goal-setter, learning-planner, franklin-scheduler |
| C | `plan.md`, `schedule.md` present | feynman-checker, sq3r-session, cornell-notes, weekly-review |
| On-demand | (any time) | confusion-endurance |

**Consequences.**
- ✅ Enforces the book's most important claim
- ✅ Users physically cannot skip blocks — system architecture enforces pedagogy
- ❌ Determined users will try to skip — the coach must redirect gracefully, not punish
- ❌ Some users have already done Phase A work (e.g., they already have a calendar/system) — the skill must allow them to record this as "completed" without re-doing it

**Evidence.** *The Science of Self-Learning*, Ch.1, "The Learning Success Pyramid" (Susan Kruger). Brain-chemistry rationale: stress diverts neurotransmitters to fight-or-flight; ego depletion exhausts the prefrontal cortex; the hippocampus only gets to encode memory when the first two are quiet.

---

## ADR-005 — JSON artifacts for inter-skill handoff

**Context.** Skills could hand off via free-text conversation ("here's what we figured out about your time…") or via structured artifacts. Free text is easier to write but harder to validate and chain.

**Decision.** Each skill produces a strict JSON artifact persisted to disk. The coordinator validates the artifact's shape before allowing the next skill to read it.

**Artifact schemas (overview):**

```jsonc
// topic-classifier → classification.md (with JSON block)
{
  "topic": "computer vision",
  "primary_type": "science" | "art" | "hybrid",
  "rationale": "...",
  "self_learning_suitability": "high" | "medium" | "low",
  "adjustments": ["..."]
}

// confidence-audit → confidence.md
{
  "existing_knowledge": ["..."],
  "past_wins": [{"learned": "...", "duration_months": N, "method": "..."}],
  "anticipated_fears": ["..."],
  "confidence_anchors": ["..."]
}

// self-management-setup → self-management.md
{
  "time_audit": [{"block": "06:30-08:30", "available": true, "intensity": "deep"}],
  "weekly_learning_hours": N,
  "primary_block": "...",
  "secondary_block": "...",
  "non_negotiables": ["family 7-8:30 PM", "job 10-7"]
}

// smart-goal-setter → smart-goal.md
{
  "specific": "...", "measurable": "...", "achievable": "...",
  "relevant": "...", "time_bound": "...",
  "deadline": "YYYY-MM-DD",
  "milestones": [{"week": N, "checkpoint": "..."}]
}

// learning-planner → plan.md
{
  "planks": [{"name": "...", "topics": [...], "weight": 0.0}],
  "week_1_survey": [{"resource": "...", "task": "..."}],
  "question_bank": ["Why...", "How..."],
  "motivation_anchors": {"autonomy": "...", "mastery": "...", "purpose": "..."}
}

// franklin-scheduler → schedule.md
{
  "weekly": [{"day": "Mon", "plank": "...", "blocks": [...]}],
  "review_cadence": {"intervals_days": [1, 3, 7, 21], "feynman": "Friday 21:00"}
}
```

**Consequences.**
- ✅ Coordinator can validate before chaining — catches bad outputs immediately
- ✅ State is machine-readable across sessions
- ✅ Future tools (visualizers, MCP servers, mobile clients) can consume the artifacts
- ❌ More structure to write for each skill — mitigated by JSON-in-markdown pattern (human-readable + machine-parseable)

**Evidence.** MetaGPT (Hong et al., ICLR 2024) demonstrates that structured artifact handoffs between specialized roles reduce hallucination and improve task completion vs. free-text agent-to-agent communication.

---

## ADR-006 — Evidence-calibrated defaults override the book where they conflict

**Context.** The book is excellent on framework selection and motivation, but it's not a peer-reviewed source on technique efficacy. Several techniques the book promotes (especially summarization without recall) are rated **low-utility** by the largest meta-analysis in the field (Dunlosky et al., 2013).

**Decision.** When the book and the cognitive-science evidence disagree, **defer to the evidence**. Document the deviation in EVIDENCE.md and the relevant SKILL.md.

**Specific deviations:**

| Book emphasis | Evidence verdict | Our default |
|---|---|---|
| Cornell summaries | Low-utility without recall (Dunlosky 2013) | Cornell skill enforces `recall_questions` field |
| SQ3R surveying | Low-utility alone; Recite/Review carry the effect | SQ3R skill front-loads Recite + Review |
| "Read it, then highlight it" | Low-utility (Dunlosky 2013) | Not in any skill |
| Feynman as "occasional check" | g ≈ 0.55 — highest single technique (Bisra 2018) | Feynman is a v1 skill; weekly cadence default |
| Vague review timing | Spacing effect d ≈ 0.40 (Cepeda 2006); FSRS-class schedulers exist | Default cadence: 1d / 3d / 7d / 21d |

**Consequences.**
- ✅ Bundle is genuinely better than "just ask Claude for a study plan" — calibrated against science
- ✅ Defensible positioning: "Hollins's frameworks, Dunlosky's hierarchy"
- ❌ Book readers may push back — EVIDENCE.md is the response
- ❌ Adds research-citation burden to maintenance — accept as the cost of credibility

**Evidence.** See EVIDENCE.md.

---

## ADR-007 — Progressive disclosure: ≤500 lines / ≤5000 tokens per SKILL.md

**Context.** SKILL.md files are loaded into context on every activation. Bloated SKILL.md = wasted context budget every session.

**Decision.** Each SKILL.md stays under **500 lines / 5000 tokens** in its body (frontmatter excluded). Anything beyond → push to:

- `references/<topic>.md` — domain detail loaded on demand by the model
- `scripts/<task>.py` — deterministic logic the model invokes via Bash

**Pattern enforced:**

```
skills/learning-planner/
├── SKILL.md                          ← ≤500 lines: the workflow
├── references/
│   ├── dunlosky_utility_table.md     ← loaded when the model needs technique selection
│   └── motivation_anchors_examples.md
└── scripts/
    └── plank_weight_calculator.py    ← deterministic math
```

**Consequences.**
- ✅ Skill catalog (tier-1 description loading) costs ~100 tokens per skill
- ✅ Skill instruction (tier-2 SKILL.md loading) costs ≤5000 tokens
- ✅ Skill resources (tier-3 references/scripts) cost zero unless needed
- ❌ More files to maintain — mitigated by the rule that any single SKILL.md only references what it actually needs

**Evidence.** `agentskills.io/specification` recommends <500 lines / <5000 tokens. Real examples in `anthropics/skills/pptx/` push detail into `editing.md` and `pptxgenjs.md`. The `docx` skill bundles `scripts/office/unpack.py`, `pack.py`, `validate.py` for repeated deterministic work.

---

## ADR-008 — Description hardening via skill-creator eval loop (deferred to v1.0)

**Context.** Descriptions are the single highest-leverage authoring choice — they determine whether Claude picks the right skill for a given user prompt. Hand-writing descriptions tends to undertrigger (skill never fires) or overtrigger (wrong skill fires).

**Decision.** Before v1.0 release, port `skill-creator/run_loop.py` (from anthropics/skills) and run it against each skill with:
- ~20 should-trigger queries (varied phrasings of the right user intent)
- ~20 should-not-trigger queries (near-miss intents that belong to other skills)
- 60/40 train/validation split to prevent overfitting

**Consequences.**
- ✅ Catches description collisions empirically rather than by hand-reading
- ✅ Documented eval set becomes a regression test for future description tweaks
- ❌ Adds ~1 hour per skill to release — 11 skills × 1 hour = 11 hours. Spread across v1.0 polish week.

**Evidence.** `skill-creator/SKILL.md` lists "missing the eval viewer before iterating" as one of the top authoring mistakes. The optimization technique is documented at `agentskills.io/skill-creation/optimizing-descriptions`.

---

## ADR-009 — Web search and fetch for time-sensitive or out-of-scope information

**Context.** Claude's training data has a cutoff. Self-Learning OS operates on topics where the resource landscape changes constantly: course launches, new GitHub repos, new arXiv papers, framework releases, package versions, market conditions. A skill that hand-waves "I'll suggest resources" without verifying current availability ships stale recommendations on day one.

**Decision.** Every skill that touches current external information **must invoke `WebSearch` and/or `WebFetch`** when:

1. The information is **time-sensitive** (course availability, framework versions, repo activity, paper publication date, market state, prices, deadlines).
2. The information is **post-training-cutoff** for the model executing the skill (new releases, new papers, recent benchmarks, recent news).
3. The information is **ecosystem-specific to the user's stack** (current dependencies, current best-of-breed library for a stack).
4. The user **explicitly asks** for current / latest / now-available resources.

**Per-skill expected usage (v1):**

| Skill | Expected web use |
|---|---|
| `learning-planner` | **Heavy.** Verify every resource URL is alive, check repo star counts and last-commit dates, find papers post-2023, validate course availability. |
| `self-learning-coach` | **Light.** Surfaces the tool to the user when they ask for current/specific resources. |
| `feynman-checker` | **Occasional.** Verify technical claims against current docs when the user's explanation involves a moving target. |
| `topic-classifier` | **Rare.** Classifications are stable. Use only if the topic itself is brand new (e.g., a 2026 framework). |
| `confidence-audit` | None. |
| `self-management-setup` | None. |
| `smart-goal-setter` | **Rare.** Only when the goal references a fixed external deadline (e.g., a specific certification exam date). |
| `franklin-scheduler` | None. The spaced-repetition cadence is fixed by evidence. |

**Skills that use web tools must:**

- **Cite sources** in the artifact. Every URL goes into the JSON output, not just summarized away.
- **Surface freshness** to the user. If a resource is older than 2 years and the field moves fast (e.g., ML, JS frameworks), flag it.
- **Prefer primary sources.** Official docs > blog posts > tutorials.
- **Run web checks before generating the artifact**, not after — so the artifact reflects current reality.

**Consequences.**
- ✅ Resource lists are never stale on day one
- ✅ Users get current-day reality, not snapshots from training
- ❌ Skills take longer to execute (web round-trips) — acceptable for one-time Phase B work
- ❌ Some Claude clients may not have web tools enabled — the skill must degrade gracefully (note "web tools unavailable; resources may be stale" and proceed)

**Evidence.** Pragmatic — every "study plan generator" failure mode in production stems from stale recommendations. Verified by the conversation that motivated this project: the original CV plan referenced fast.ai, PyImageSearch, OpenCV docs by URL; only web checks at generation time can guarantee those are still the best-of-breed.

---

## Open ADRs (not yet decided)

- **ADR-010** — Subagent vs skill for `feynman-checker` in v1.1 (depends on whether deep adversarial questioning blows past skill context)
- **ADR-011** — Multi-topic concurrency (current model: one topic at a time; multi-topic requires a topic-index file and routing logic)
- **ADR-012** — Localization / i18n strategy
- **ADR-013** — Caching policy for web-search results (avoid re-fetching the same URL across skills within one session)
