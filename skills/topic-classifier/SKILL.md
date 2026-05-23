---
name: topic-classifier
description: Use this skill when the user has just named what they want to learn and the coach needs to classify the topic as art, science, or hybrid — before any confidence work, planning, or scheduling. Triggers when ~/.self-learning-os/<topic>/classification.md does not exist for the named topic. Classifies by whether the topic has objective right/wrong answers (science — programming, math, physics, languages-grammar) or interpretive subjective dimensions (art — painting, music performance, creative writing, design taste), or hybrid (music theory + performance, design systems + visual taste). Outputs the implications for how downstream skills (SQ3R, Cornell, Feynman) must be adapted. Do NOT use for topics already classified (check state first), for helping the user decide what to learn, or for general topic overviews.
---

# Topic Classifier (Phase A — Diagnostic)

You classify the user's chosen learning topic on the **art vs science** spectrum, because this determines how every downstream skill operates.

## Why this matters (book + research)

*The Science of Self-Learning* (Ch.1) makes a strong claim: art topics have no absolute right/wrong answers — the teacher's interpretation is indivisible from the subject. Science topics have objective, verifiable facts that exist regardless of who teaches them. **Sciences are more reliably self-taught** because the learner can verify progress objectively; arts require small adjustments to the standard self-learning toolkit.

This is not a value judgment. Arts are absolutely self-teachable. But the **method must adapt**:

| Dimension | Science topics | Art topics |
|---|---|---|
| Recite step (SQ3R) | Solve problems / write code / run experiments | Produce work + get critique |
| Feynman check | Explain mechanism with falsifiable predictions | Explain choices + show alternatives + defend taste |
| Progress markers | Passing tests, problems solved, projects shipped | Portfolio growth, critique cycles, peer recognition |
| Resources | Textbooks, problem sets, official docs | Master works to study, critique groups, mentor feedback |
| Confusion endurance | Specific concept gaps you can name | Aesthetic uncertainty that doesn't resolve cleanly |

## When to invoke

The coordinator (`self-learning-coach`) calls you when `~/.self-learning-os/<topic>/classification.md` does not exist. You should not be invoked otherwise.

## Workflow

### Step 1 — Confirm the topic scope

The user said "I want to learn X." But X may be ambiguous:

- "Music" — performance? composition? theory? production?
- "Photography" — gear/technique (science-leaning) or aesthetic eye (art-leaning)?
- "Trading" — technical analysis (science) or discretionary decision-making (hybrid)?
- "Writing" — grammar/structure (science) or voice/craft (art)?
- "Computer Vision" — algorithms (science) or applied to creative work (hybrid)?

If the topic is ambiguous, ask **one** clarifying question. Then proceed.

### Step 2 — Classify

Choose **one** primary type. Use this rubric:

- **Science** — There are facts that are true regardless of the teacher. Performance is verifiable against an objective standard. Examples: programming languages, mathematics, physics, chemistry, language grammar, music theory, statistics, electronics, mechanical repair.
- **Art** — Performance is judged subjectively. Multiple "correct" answers exist depending on context, taste, or culture. Examples: painting, music performance, creative writing, fashion design, photography composition, public speaking style, comedy writing.
- **Hybrid** — Has substantial portions of both. The user will need to switch modes. Examples: software architecture (science of correctness + art of design), trading (science of statistics + art of discretion), product management, UX design, journalism.

Do not allow more than one primary type. If the user resists ("but it's both!"), ask which side they want to lead with for the first 3 months — they can revisit later.

### Step 3 — Identify implications

For each downstream skill, state how it will adapt:

| Skill | Adaptation |
|---|---|
| `learning-planner` | If science → textbooks + problem sets dominate. If art → portfolio of master works + critique. If hybrid → split planks. |
| `franklin-scheduler` | Science: practice-testing blocks with verifiable solutions. Art: production blocks ending in shareable artifacts. |
| `sq3r-session` (v1.1) | Science: Recite = solve. Art: Recite = produce + compare. |
| `cornell-notes` (v1.1) | Science: cues are testable. Art: cues are stylistic decisions + tradeoffs. |
| `feynman-checker` | Science: explain mechanism. Art: explain choices and tradeoffs to a critic. |

### Step 4 — Surface self-learning suitability

Be honest about how hard this will be alone:

- **High suitability** (mostly science topics with great free resources): "You can do this entirely solo."
- **Medium suitability** (mixed, or science topics with steep prerequisites): "You can do this, but you'll need to actively seek a community / critique for one part of the curriculum."
- **Low suitability** (high-art topics requiring mentor calibration): "Self-learning will get you 60%. The last 40% requires a mentor, critique group, or formal feedback loop. Plan for that now."

### Step 5 — Write the artifact

Save to `~/.self-learning-os/<topic>/classification.md`:

```markdown
# Topic Classification: <topic>

## Summary
- **Primary type:** <science | art | hybrid>
- **Self-learning suitability:** <high | medium | low>
- **Scope confirmation:** <user's chosen scope>

## Rationale
<2-3 sentence explanation of why this classification>

## Adjustments for downstream skills
- learning-planner: <what changes>
- franklin-scheduler: <what changes>
- sq3r-session: <what changes>
- cornell-notes: <what changes>
- feynman-checker: <what changes>

## Critique/community requirements
<If suitability is medium or low, what external feedback loop the user must set up>

---

```json
{
  "topic": "<topic>",
  "primary_type": "<science|art|hybrid>",
  "self_learning_suitability": "<high|medium|low>",
  "scope": "<confirmed scope>",
  "adjustments": {
    "learning_planner": "<...>",
    "franklin_scheduler": "<...>",
    "sq3r_session": "<...>",
    "cornell_notes": "<...>",
    "feynman_checker": "<...>"
  },
  "external_feedback_required": <true|false>,
  "external_feedback_plan": "<if true, brief plan>"
}
```
```

### Step 6 — Hand back to coordinator

Confirm completion to the user in one sentence, then hand control back to `self-learning-coach` for routing to `confidence-audit`.

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **rare**.

Classifications are conceptually stable. Use `WebSearch` only when:

- The user names a **brand-new framework or technology** (released after your training cutoff) that you cannot identify confidently. One query to confirm what it is, then classify normally.
- The user names a **domain that has fundamentally shifted** since your training (rare — most topic boundaries are durable).

If unsure between science and art due to recent ecosystem changes (e.g., "vibe coding" — is it engineering or design?), a quick web check is justified. Otherwise, do not use web tools.

## Examples

### Example 1 — Computer Vision
- Confirmed scope: deep-learning–based CV (CNNs, YOLO, segmentation) for applied work
- Primary type: **science**
- Suitability: **high** — abundant high-quality free resources (fast.ai, PyImageSearch, OpenCV docs, Hugging Face)
- Adjustments: Recite = implement in PyTorch; Feynman = explain mechanism and failure modes

### Example 2 — Music Performance (Piano)
- Confirmed scope: classical repertoire, intermediate level
- Primary type: **art**
- Suitability: **medium** — solo practice works for technique but interpretation requires teacher or recital critique
- Adjustments: Recite = perform and self-record; Feynman = explain phrasing choices; **external feedback required** = monthly lesson or peer critique group

### Example 3 — Options Trading
- Confirmed scope: systematic options strategies (not discretionary)
- Primary type: **hybrid** — statistics + risk management (science) + market regime judgment (art)
- Suitability: **medium**
- Adjustments: split planks (statistics plank: textbooks + backtests; market plank: live observation + journaling); Feynman includes both quantitative explanation and qualitative regime reading

## Common failures to avoid

- **Forcing science classification on genuinely subjective topics** because it's "easier to plan." Be honest.
- **Refusing to commit to a primary type** because it's hybrid. Pick the lead, document the secondary.
- **Skipping the suitability honesty.** If the user needs a teacher, tell them now — not month three.
- **Generic adjustments.** Each adjustment must be specific to this topic, not boilerplate.

## References

- *The Science of Self-Learning*, Ch.1, "Arts vs. Sciences in Self-Learning"
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-004 (gating), ADR-005 (artifacts), ADR-009 (web search)
