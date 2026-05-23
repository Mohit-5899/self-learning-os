---
name: sq3r-session
description: Use this skill to walk the user through a full SQ3R session (Survey, Question, Read, Recite, Review) on a specific resource — book chapter, paper, video lecture, or documentation. Triggers when the user says they want to "study a chapter", "work through a paper", "go through a tutorial properly", "read this carefully", or asks for an SQ3R guide. Enforces all five steps with MANDATORY Recite and Review — surveying alone is low-utility (Carlston 2011, Hartlep & Forsyth 2000); the retrieval steps are what make SQ3R effective. Schedules the next spaced-repetition reviews at +1d / +3d / +7d / +21d. Do NOT use for users still in Phase A or B, for general reading without learning intent, for resources shorter than ~15 minutes of engagement, or as a substitute for the underlying resource.
---

# SQ3R Session (Phase C — Execution)

You walk the user through one complete **SQ3R session** (Survey → Question → Read → Recite → Review) on a specific resource. The session has **five steps and you enforce all of them** — surveying alone is the most common failure mode.

## Why this matters

From *The Science of Self-Learning*, Ch.2:

> Five-step method to make reading active, dynamic, and information-retaining... The biggest difference between reading to learn and reading for entertainment is the Recite step.

Carlston (2011) and Hartlep & Forsyth (2000) evaluated structured reading methods. Verdict: **SQ3R works because of Recite and Review** — the retrieval steps — not because of Survey or Question. Users who do Survey + Read and skip Recite + Review do not retain materially better than people who just read.

So your job is **enforce Recite and Review**. Skip them and the technique collapses to "reading with extra steps."

## When to invoke

- The user is about to engage with a substantial resource (≥15 min of focused engagement)
- They want to do it *properly*, not just skim
- Per `schedule.md`, an SQ3R session is in their primary block

For short resources (<15 min), a Cornell session or direct Feynman attempt is faster.

## Workflow

### Step 1 — Identify the resource

Ask:

> What resource are you working through? (chapter / paper / video / doc page) And how long do you estimate it'll take?

Capture: title, URL/source, estimated length, plank.

### Step 2 — Read prior context

Read `classification.md`. Adapt the workflow:

- **Science topic:** Recite = solve / implement / verify against an objective answer
- **Art topic:** Recite = produce + compare against the master work
- **Hybrid:** alternate per resource

### Step 3 — SURVEY (~10% of total time)

Prompt:

> Don't dive in yet. First, survey it — like looking at a map before a road trip. Look at the title, headings, subheadings, intro and conclusion, any diagrams or figures, the index if it's a book. Tell me three things: (1) what's the structure, (2) what does the author seem to be claiming, (3) what's it NOT going to cover.

Capture the user's three observations. If they're skipping this and want to read straight away, push back: surveying is what makes the reading focused. But cap at 10% of the time budget — surveying is **not** the goal.

### Step 4 — QUESTION (~10% of total time)

Prompt:

> Now turn the headings into questions. For each major section, rewrite the heading as a question you'll hunt for the answer to while reading. Examples: "Backpropagation" → "How does backpropagation actually work mechanically?" / "What are the steps in order?"

The user generates **5–10 questions**. These become the reading targets. Without them, reading drifts; with them, the user is hunting.

### Step 5 — READ (~50% of total time, the largest block)

Prompt:

> Now read. Slow down — this is not entertainment reading. When you hit a passage that's unclear, stop and re-read from the section start. Take notes if you want, but don't highlight. Stay in your own words.

The user reads. You don't interrupt — but if they're reading too fast (per the book Ch.3 — slow down on dense material), surface it.

If the resource is technical and the user is implementing along (e.g., a tutorial), the implementation counts as part of Read — they're testing comprehension by execution.

### Step 6 — RECITE (~20% of total time, the critical step)

This is the step most users skip. Do not let them.

Prompt:

> Close the resource. Without looking. Try to answer each of the questions you wrote in Step 4 — out loud, or by writing. If you can't answer one, that's a gap.

For each question, capture:
- The user's answer (verbatim or paraphrase)
- Your assessment: **solid / partial / gap**

For gaps:
- Briefly note what was missed
- Schedule a re-read of that specific section in the +1d slot, not the full resource

This is **retrieval practice** (Dunlosky HIGH utility). The struggle is the point. If the user produces fluent answers without effort, push harder — ask follow-ups.

### Step 7 — REVIEW (~10% of total time)

Prompt:

> Re-open the resource. For each question you answered partially or with a gap, go to the relevant section and verify. Don't re-read the whole thing — just the gap sections. Then write a 3-sentence summary of the whole resource.

This is the per-resource Review (different from `weekly-review`, which is cross-resource).

Schedule the artifact for spaced re-engagement at +1d / +3d / +7d / +21d.

### Step 8 — Write the artifact

Save to `~/.self-learning-os/<topic>/reviews/sq3r-<YYYY-MM-DD>-<resource-slug>.md`:

```markdown
# SQ3R Session: <resource> — <date>

## Survey observations
- Structure: <user's note>
- Author's claim: <user's note>
- What's NOT covered: <user's note>

## Questions generated
1. <question>
2. <question>
...

## Reading notes
<key notes, paraphrased, no highlighting>

## Recite attempt (cold, without looking)
| Question | User's answer | Assessment |
|---|---|---|
| Q1 | ... | solid / partial / gap |

## Gaps to revisit
- Section X — re-engage at +1d
- ...

## Review summary (3 sentences)
<global frame>

## Spaced review schedule
- +1d: <date> — re-read gap sections, re-attempt failed questions
- +3d: <date> — re-attempt all questions cold
- +7d: <date> — Feynman-check one concept from this resource
- +21d: <date> — full integration check

---

```json
{
  "resource": {"title": "...", "url": "...", "chapter": "...", "plank": "..."},
  "session_date": "YYYY-MM-DD",
  "time_minutes_actual": <N>,
  "survey": {"structure": "...", "claim": "...", "not_covered": "..."},
  "questions": ["..."],
  "reading_notes": "...",
  "recite_attempt": [
    {"question": "...", "answer": "...", "assessment": "solid|partial|gap"}
  ],
  "gaps_to_revisit": [{"section": "...", "revisit_at": "+1d"}],
  "review_summary": "...",
  "next_review_dates": ["YYYY-MM-DD (+1d)", "YYYY-MM-DD (+3d)", "YYYY-MM-DD (+7d)", "YYYY-MM-DD (+21d)"]
}
```
```

### Step 9 — Hand back

Confirm completion. The gaps and review dates are now in the spaced-repetition queue.

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **occasional**.

Use `WebFetch` only when:
- The user is reading an online resource (URL) — fetch to confirm currency and (if needed) extract sections you'll quote
- The user references a related paper / spec during Recite and you need to verify their claim

Most SQ3R sessions are on a single resource already in the user's hands. No web tools needed.

## What to avoid

- **Skipping Recite.** This is the most common SQ3R failure. Do not let the user end the session at Read.
- **Over-surveying.** Cap at 10% of total time. Survey is orientation, not learning.
- **Letting Recite turn into re-reading.** Recite is COLD — book closed, eyes off the screen. If the user can't answer without looking, that's a gap to log, not a hint to re-read.
- **Highlighting during Read.** Per Dunlosky 2013 — LOW utility. Push toward paraphrased notes if anything.
- **Stuffing too many questions into Question step.** 5–10 is the sweet spot; 15+ becomes unfocused.
- **Generic summary.** "It was about X" is useless. Specific top-level claim + key supporting moves.

## What "good" looks like

A completed SQ3R artifact should let the user say: *"I now have 5–10 questions about this resource that I can answer cold, a list of 1–2 specific gaps I need to revisit tomorrow, and a 3-sentence frame of the whole thing."*

If after the session the user remembers "the gist" but can't answer any specific question, the Recite step was skipped or fake — re-do it.

## References

- *The Science of Self-Learning*, Ch.2 — SQ3R Method (Francis P. Robinson)
- Carlston (2011) — Benefits of student-generated note packets
- Hartlep & Forsyth (2000) — Note Taking and Memory
- Dunlosky et al. (2013) — retrieval HIGH, highlighting LOW
- [EVIDENCE.md](../../EVIDENCE.md)
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-006 (evidence-calibrated defaults)
