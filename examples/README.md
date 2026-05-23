# Worked Examples

Real outputs from running the Self-Learning OS bundle end-to-end on actual learning journeys. These are not synthetic — they're the exact artifacts the bundle produced for a real user, copied verbatim from their `~/.self-learning-os/<topic>/` directory.

## `computer-vision/` — AI engineer learning modern computer vision

**Learner profile:** AI engineer at day job (10 AM – 7 PM), runs Quanta AI Labs as a side business, family time 7 PM – 8:30 PM. Already has Python + LangGraph + RAG + PyTorch experience.

**Goal:** Master modern deep-learning-based CV (CNNs through Vision Transformers through object detection) in 12 weeks, using the [Vizuara YouTube CV playlist](https://www.youtube.com/@Vizuara) as the primary resource. Output target: trained custom classifier + YOLO detector + deployed demo + Quanta blog post.

**Time budget:** 18 hours/week (06:30–08:30 weekday morning primary + 21:00–22:15 weekday secondary + 09:00–11:00 Saturday weekend session).

**What's in this example:**

| File | Phase | What it contains |
|---|---|---|
| `state.md` | meta | Current phase tracker + activity log |
| `classification.md` | A | Science vs art classification (verdict: science, high suitability) |
| `confidence.md` | A | Existing knowledge, past wins, fears, anchors, 3-month markers |
| `self-management.md` | A | Weekly time audit, primary/secondary blocks, non-negotiables, fallback minimums |
| `smart-goal.md` | B | 12-week SMART goal with 3 milestones + pivot rule |
| `smart-goal.html` | B | **Visual goal card** (open in browser) |
| `plan.md` | B | 6 planks, week-1 survey list, 12-question bank, motivation anchors |
| `plan.html` | B | **Visual plan dashboard** (open in browser) |
| `schedule.md` | B | Weekly grid + 1d/3d/7d/21d review cadence + Fri Feynman + Sat weekend session |
| `schedule.html` | B | **Visual weekly calendar grid** (open in browser) |

**How this example was generated:**

A real Claude session running this bundle's `self-learning-coach` skill. The session walked through:
1. `topic-classifier` → classification.md
2. `confidence-audit` → confidence.md
3. `self-management-setup` → self-management.md
4. `smart-goal-setter` → smart-goal.md + smart-goal.html
5. `learning-planner` → plan.md + plan.html
6. `franklin-scheduler` → schedule.md + schedule.html

Total time from "I want to learn CV" to "Monday morning 06:30 AM I know exactly what to do" was approximately 20 minutes of interactive Q&A.

**Caveats noticed during this acceptance test** (tracked as GitHub issues for v1.0):
- The `learning-planner` did not actually invoke WebSearch/WebFetch for resource verification (per ADR-009 it should — fix tracked in issue #1)
- The `franklin-scheduler` produced plank-rotation but didn't map specific Vizuara videos to specific days for Week 1 (fix tracked in issue #2)
- The `confidence.md` captured "2 videos pre-watched" but the planner didn't read this field to skip already-watched resources (fix tracked in issue #3)

## Reproducing

To run the same flow yourself:

```
I want to learn <topic>. Here's my context: [job hours, family time, side projects].
[paste your resource list if you have one]
```

The `self-learning-coach` skill will route you through Phase A → Phase B → Phase C, writing artifacts to `~/.self-learning-os/<topic>/` along the way.

## Contributing examples

If you run the bundle on your own learning journey and want to contribute the output as a worked example, please:
1. Sanitize any genuinely sensitive personal data (real names of family members, employer-confidential project names)
2. Open a PR adding your `<topic>/` directory under `examples/`
3. Add a row to the table above
