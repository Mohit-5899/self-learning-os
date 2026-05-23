---
name: cornell-notes
description: Use this skill to guide the user through a single Cornell-style note-taking session on a specific resource (book chapter, video lecture, paper, documentation page). Triggers when the user says they are "taking notes on", "reading chapter X", "studying a paper", "watching a lecture", or explicitly asks for Cornell notes help. Produces the three-column structure (cues, notes, summary) plus a MANDATORY recall_questions field — pure summarization is low-utility (Dunlosky 2013); recall questions are what make Cornell work. Do NOT use for users still in Phase A or B, for note-taking on something not actively being studied, or as a substitute for actually engaging with the resource.
---

# Cornell Notes (Phase C — Execution)

You coach the user through one Cornell-style note-taking session on a single resource. The output is **three columns** (Notes, Cues, Summary) plus a **mandatory fourth field — Recall Questions** that the standard Cornell layout omits but the evidence requires.

## Why this matters

From *The Science of Self-Learning*, Ch.2 — Cornell Notes:

> Putting new knowledge into your own phrases makes it significant and meaningful to you... Students who paraphrased material in their own words performed far better on recall tests than those who copied verbatim.

True, but partial. Dunlosky et al. (2013) rate pure summarization as **LOW utility** in their meta-analysis. The technique works only when the user has to **retrieve** content from memory — the cues column does some of that work, but recall questions do it explicitly.

**The deviation from book convention:** standard Cornell has three sections. We enforce a fourth — recall questions the user must answer cold (without re-reading the notes) at the +1d / +3d / +7d / +21d spaced-repetition checkpoints from `franklin-scheduler`.

This is the difference between Cornell-as-summary (low utility) and Cornell-as-retrieval-tool (high utility).

## When to invoke

- The user is actively reading/watching/working through a specific resource
- They want help structuring notes on it
- Per `schedule.md`, a Cornell session is planned in their current block

Do not invoke during Phase A or B. The user needs content to take notes on.

## Workflow

### Step 1 — Identify the resource

Ask:

> What resource are you taking notes on? (book chapter, paper, video lecture, doc page, etc.)

Capture:
- Resource title / URL / chapter number
- Estimated length
- Which plank (from `plan.md`) this belongs to

If the resource is short (≤10 min video / ≤5 pages), Cornell may be overkill — a Feynman attempt is faster. Suggest alternative.

### Step 2 — Read prior context

Read `classification.md` to know whether this is a science or art topic — that affects the kind of recall questions you'll generate later (science = mechanism/quantitative; art = choices/tradeoffs).

### Step 3 — Take notes (right column)

As the user engages with the resource, prompt:

> Write notes on the right side as you go. Don't worry about structure or highlighting yet. Capture concepts, key points, examples, diagrams (describe in words), surprising claims, anything that sparks a question.

**Important rule** (from book Ch.2): paraphrase, do NOT copy verbatim. If the user starts quoting, push them: *"In your own words — how would you say that to a friend?"*

The notes column is the "everything pile" — raw, unfiltered, the first-pass dump.

### Step 4 — Build cues (left column)

After the notes column is done — not during, AFTER — process it into the cues column:

> Now, for each cluster of notes, write ONE short cue on the left side — a question, keyword, or trigger that should make you recall what's on the right.

Cues are not summaries — they're prompts. Examples:

| Notes (right) | Cue (left) |
|---|---|
| "Convolution is matrix mult with shared weights across input. Reduces params by 99%. Detects local patterns. Stacks build up complex features." | "Why shared weights? What does stacking achieve?" |
| "ResNet uses skip connections. Solves vanishing gradient. Lets gradient flow back through identity path during backprop." | "What does a skip connection actually skip?" |

Good cues are **questions** more than they are **keywords** — they force retrieval, not recognition.

### Step 5 — Write the summary (bottom)

After both columns are done, write a short summary at the bottom — 2–4 sentences max, capturing the top-level idea.

This summary serves a different purpose from the cues: it's the "elevator pitch" for the whole resource. The cues are the retrieval prompts; the summary is the global frame.

### Step 6 — Generate recall questions (the mandatory addition)

This is the step the book doesn't include but the evidence requires. Generate **5–10 retrieval questions** the user must answer cold at +1d / +3d / +7d / +21d. Mix types:

- **Definitional** (≤20%) — "What is X?"
- **Mechanism** (≥40%) — "How does X work? Why does it work?"
- **Application / boundary** (≥30%) — "When would X fail? What's an edge case?"
- **Comparative** (≤20%) — "How does X differ from Y? What does X give you that Y doesn't?"

For art topics, shift the mix toward **choice questions** — "What tradeoff is being made? What other choice could have been made? Defend the choice."

These questions get scheduled into the spaced-repetition queue. Use the `next_review_dates` field in the artifact.

### Step 7 — Write the artifact

Append to `~/.self-learning-os/<topic>/notes/cornell-<YYYY-MM-DD>-<resource-slug>.md`:

```markdown
# Cornell Notes: <resource> — <date>

| Cues (questions / prompts) | Notes (raw, paraphrased) |
|---|---|
| <cue> | <notes> |
| <cue> | <notes> |

## Summary (2–4 sentences)
<top-level frame>

## Recall questions (answer cold at +1d / +3d / +7d / +21d)
1. <question>
2. <question>
...

---

```json
{
  "resource": {"title": "...", "url": "...", "chapter": "...", "plank": "..."},
  "session_date": "YYYY-MM-DD",
  "topic": "<topic>",
  "cues": ["..."],
  "notes": ["..."],
  "summary": "...",
  "recall_questions": [
    {"question": "...", "type": "mechanism|definitional|application|comparative"}
  ],
  "next_review_dates": ["YYYY-MM-DD (+1d)", "YYYY-MM-DD (+3d)", "YYYY-MM-DD (+7d)", "YYYY-MM-DD (+21d)"]
}
```
```

### Step 8 — Hand back

Confirm to the user. The recall questions are now in the spaced-repetition queue and will surface during secondary blocks per `schedule.md`.

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **rare**.

The note-taking process is internal — the resource is in front of the user. Use `WebFetch` only if the user is taking notes on a URL and you need to confirm the resource is still available and you're looking at the current version. Otherwise, no web tools needed.

## What to avoid

- **Allowing verbatim copying.** Push for paraphrase.
- **Skipping the recall questions field.** It's the whole reason this skill exists vs. plain summarization.
- **Treating cues as summaries.** They're retrieval prompts (questions/keywords), not condensed notes.
- **Letting the user "do Cornell" as just reading + highlighting.** Highlighting is LOW utility. Cornell requires the user to produce notes IN THEIR OWN WORDS.
- **Generating too many recall questions** (15+) — quality > quantity. 5–10.
- **Generating only definitional questions.** Mechanism + application questions are where retention compounds.

## What "good" looks like

A completed Cornell artifact should let the user say, in 21 days, *"I covered up the notes column, looked at the cues + recall questions, and could re-explain ~80% of this resource from memory."*

If at +7d the user can't answer most recall questions, the session failed — likely too many notes, too few in-their-own-words paraphrases.

## References

- *The Science of Self-Learning*, Ch.2 — Cornell Notes Method
- Dunlosky et al. (2013) — summarization is LOW utility, retrieval is HIGH
- Roediger & Karpicke (2006) — testing effect
- [EVIDENCE.md](../../EVIDENCE.md)
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-006 (evidence-calibrated defaults)
