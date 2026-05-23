---
name: smart-goal-setter
description: Use this skill after self-management-setup, to convert the user's 3-month progress markers (from confidence.md) and weekly time budget (from self-management.md) into one rigorous SMART goal — Specific, Measurable, Achievable, Relevant, Time-bound — with weekly milestones. Triggers when ~/.self-learning-os/<topic>/self-management.md exists but smart-goal.md does not. Required before learning-planner. The skill rejects vague goals ("learn computer vision") and pushes the user toward measurable outcomes ("train and deploy a custom YOLO object detector on a self-chosen dataset by week 12"). Do NOT use for users still in Phase A, for non-learning goals, or for already-set goals (check state first).
---

# SMART Goal Setter (Phase B — Planning)

You convert the user's vague aspiration into **one rigorous SMART goal** with measurable weekly milestones.

## Why this matters

From *The Science of Self-Learning*, Ch.4:

> Goals should be realistically achievable but not so easy that achieving them feels meaningless. Goals should be just a tiny bit higher than what you think you can do — not impossibly high.

A SMART goal turns "I want to learn X" into something you can win or lose at on a specific date. Without that, every plan dissolves into vague effort — which is what the book's Chapter-2 Jorge story is about.

The SMART framework:

- **Specific** — Unambiguous about what exists at the end
- **Measurable** — Pass/fail check, not "feeling like I know it"
- **Achievable** — Realistic given the time budget already locked in `self-management.md`
- **Relevant** — Personally significant to the user (ties back to their `confidence.md` progress markers)
- **Time-bound** — Concrete deadline (default 3 months, but reconcile with weekly hours)

## When to invoke

The coordinator calls you when `self-management.md` exists but `smart-goal.md` does not.

## Workflow

### Step 1 — Read prior context

Read:
- `classification.md` — topic type and self-learning suitability
- `confidence.md` — especially the `progress_markers_3_month` list
- `self-management.md` — especially `weekly_learning_hours`

These are your inputs. The progress markers describe *what success looks like*; the weekly hours describe *what's possible*. The SMART goal lives at the intersection.

### Step 2 — Reality-check the time budget

Multiply the weekly hours by 12 (a 3-month default) to get a total budget.

| Weekly hours | 3-month total | Realistic scope |
|---|---|---|
| <3 | <36 hours | Surface-level exposure; one micro-project. Scope down. |
| 3–5 | 36–60 hours | One small project + foundation reading. Aggressive. |
| 6–10 | 72–120 hours | One real working project + multiple core concepts. Realistic. |
| 10–15 | 120–180 hours | Comfortable mastery of fundamentals + 2+ projects. |
| 15+ | 180+ hours | Approaching full-time student level. |

If the user's progress markers (from `confidence.md`) imply a scope that doesn't fit their hour budget, surface this **before** writing the goal. Two options:

- **Reduce scope:** "Mastering CV in 3 months at 4 hours/week is not realistic. Want to shrink scope to 'image classification + one project' for this 3 months, with object detection as a v2 goal?"
- **Extend timeline:** "Or extend the deadline to 6 months."

Do not let the user set an impossible goal. The book is clear: impossible goals destroy confidence.

### Step 3 — Draft the SMART goal

Write a single sentence covering all five SMART dimensions. Examples:

**Bad:** "I will learn computer vision."

**SMART:** "By <date 12 weeks from today>, I will have trained a custom YOLO object detector on a dataset I assembled myself, deployed it behind a simple web demo, and written one technical blog post explaining the architecture — investing 8 hours per week of focused study, because this directly extends my Hoichoi AI engineering work and creates a portfolio piece for Quanta AI Labs."

The structure:
- **Specific:** "trained a custom YOLO object detector on a dataset I assembled myself, deployed it behind a simple web demo, and written one technical blog post"
- **Measurable:** the three deliverables exist or they don't
- **Achievable:** 8 hours/week × 12 weeks = 96 hours, sufficient for this scope
- **Relevant:** the user's own "why" (job + business)
- **Time-bound:** specific 12-week deadline

### Step 4 — Decompose into milestones

Break the goal into ~4 monthly checkpoints (or ~12 weekly checkpoints for tighter cadence):

| Week | Milestone |
|---|---|
| 4 | Understand image processing fundamentals + reproduce 3 OpenCV examples end-to-end |
| 8 | Train a working image classifier on a public dataset (MNIST/CIFAR-10 minimum) |
| 12 | Custom YOLO detector + deployed demo + blog post live |

Each milestone is itself SMART — testable, dated, achievable.

### Step 5 — Surface the kill-switch

Ask:

> If by week 4 you have not hit Milestone 1, what will you do? Continue, adjust scope, or stop?

The book emphasizes that failure should redirect, not destroy. Lock the **pivot rule** now, while motivation is high. Options:

- "If week 4 milestone missed by >50%, scope down (drop the YOLO detector, keep image classification)."
- "If milestone missed because life intervened, extend deadline by 4 weeks and continue."
- "If milestone missed because the topic isn't what I expected, reclassify and restart."

Capture this explicitly.

### Step 6 — Write the artifact

Save to `~/.self-learning-os/<topic>/smart-goal.md`:

```markdown
# SMART Goal: <topic>

## Goal statement
<single SMART sentence>

## SMART breakdown
- **Specific:** <what exactly>
- **Measurable:** <pass/fail criteria>
- **Achievable:** <hour math + scope justification>
- **Relevant:** <user's why>
- **Time-bound:** <deadline date>

## Milestones
- Week 4: <milestone>
- Week 8: <milestone>
- Week 12: <milestone>

## Pivot rule
<what to do if a milestone is missed>

---

```json
{
  "specific": "...",
  "measurable": "...",
  "achievable": "...",
  "relevant": "...",
  "time_bound": "...",
  "deadline_date": "YYYY-MM-DD",
  "weekly_hours_committed": <N>,
  "total_hour_budget": <N>,
  "milestones": [
    {"week": 4, "milestone": "...", "verification": "..."},
    {"week": 8, "milestone": "...", "verification": "..."},
    {"week": 12, "milestone": "...", "verification": "..."}
  ],
  "pivot_rule": "..."
}
```
```

### Step 7 — Hand back

Confirm completion. Coordinator routes to `learning-planner`.

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **rare**.

Use `WebSearch` only when the goal references a fixed external deadline you need to verify:

- A certification exam date ("By the next OpenCV certification exam date")
- A specific conference or hackathon ("Submit a project by NeurIPS 2026 deadline")
- A course completion deadline ("Finish fast.ai Practical Deep Learning Part 1 by its next cohort end")

In those cases, one targeted query to confirm the exact date. Otherwise, the goal-setting process is purely internal.

## What to avoid

- **Vague goals.** "I will be good at X" is not SMART. Push back.
- **Goals that exceed the hour budget.** Math is non-negotiable.
- **Goals with no measurable artifact.** "I will understand CV" has no pass/fail. Force a deliverable.
- **Setting one giant goal with no milestones.** A 12-week goal with no week-4 checkpoint is unaccountable.
- **Skipping the pivot rule.** Most users hit week 4 and freeze when behind. Pre-commit to a response.

## What "good" looks like

A completed `smart-goal.md` should make the user say: *"I know exactly what I'm building, when it's due, what the checkpoints are, and what to do if I'm behind."*

## References

- *The Science of Self-Learning*, Ch.4 — SMART Goals Framework
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-004 (gating), ADR-005 (artifacts)
