---
name: weekly-review
description: Use this skill for the weekly Sunday consolidation across all active learning topics — what got done, what slipped, what to adjust for next week. Triggers on Sundays, whenever the user says "weekly review", "how is my week looking", "let's reflect on the week", "what's left this week", or when more than 6 days have passed since the last review. Reads state.md and the week's notes/reviews directories, surfaces planks completed vs skipped, confirms the weekly Feynman check ran, flags missed spaced reviews, and produces explicit adjustments for next week's schedule. Do NOT use for daily review (that's the secondary block in franklin-scheduler), for users still in Phase A or B, or for users with no completed study sessions yet in the past week.
---

# Weekly Review (Phase C — Cadence)

You run the user's **Sunday consolidation** — the cross-resource, cross-plank look at the whole week. This is different from the per-resource Review step inside `sq3r-session`. That's per-resource. This is **whole-week, cross-topic**.

## Why this matters

From *The Science of Self-Learning*, Ch.2 — SQ3R Review, generalized:

> Treat this as a continuation of survey — you've now gone deep; step back and make updated, more accurate connections.

And Ch.4 — Franklin's daily schedule:

> 5–10 PM: Reflect, eat supper, consider what good was done.

Both elements combine here. Without a weekly review, three things happen:

1. **Slipped work gets buried.** If Thursday's primary block didn't happen, the user notices on the following Thursday at the earliest — a week wasted.
2. **Spaced reviews drift.** Reviews scheduled at +7d slide past their slots and the schedule rots.
3. **The schedule stays static.** A schedule that's never adjusted becomes either too easy (boring) or too hard (abandoned). Weekly review is the **adjustment point**.

## When to invoke

- On a Sunday (or whichever day the user picked for their longer weekend session)
- Whenever the user explicitly asks for a weekly review
- Whenever `state.md` shows `last_activity` more than 6 days old AND the user has resumed

Do not invoke during Phase A or B. There is nothing to review yet.

## Workflow

### Step 1 — Read state and inventory the week

Read for the user's active topic(s):
- `state.md` — last activity, activity log
- `schedule.md` — what was *planned* for the week
- `notes/` — Cornell sessions completed (count and resources)
- `reviews/` — SQ3R sessions + Feynman checks completed
- `log.md` (if it exists) — confusion-endurance interventions

Build a comparison: **planned vs actual**.

| Day | Planned plank | Did it happen? | Notes |
|---|---|---|---|
| Mon | Math foundation | ✅ | Cornell session done |
| Tue | Image processing | ✅ | SQ3R, 1 gap logged |
| Wed | Deep learning | ❌ | Late meeting, fell back to flashcards |
| Thu | Object detection | ✅ | Cornell done |
| Fri | Project + Feynman check | ⚠️ | Project done, Feynman skipped |
| Sat | Weekend session | (this) | reviewing now |

### Step 2 — Confirm the weekly Feynman check status

The most important single item to verify. From `schedule.md`, the weekly Feynman check should have happened at a specific slot.

- If completed → great. Note which concept was checked.
- If skipped → this is the single highest-priority recovery item. Schedule it before any other adjustment.

Per Bisra 2018, this is the highest-utility single technique. Missing it means missing the biggest retention lever.

### Step 3 — Audit spaced reviews

For each new concept introduced this week, check the spaced-review queue:

- +1d reviews from items 6 days ago → done?
- +3d reviews from items 4 days ago → done?
- +7d reviews from items last week → done?

Surface any missed reviews. Reschedule into next week, **not** retroactively. If a +7d review was missed, push it to +1d in the new week and continue from there (the spacing math approximately holds).

### Step 4 — Surface the wins

This is the part most users skip. Be explicit:

> Before adjustments — what worked this week? Which session felt productive? Where did you have a moment of *"I actually get this now"*?

Capture 2–3 wins. These feed back into the confidence anchors. Per Hollins Ch.4 — *fears and worries feel powerful in the present because the future isn't certain* — surfacing wins is the antidote.

### Step 5 — Name the blockers (without judgment)

Ask:

> Where did things break this week? Be specific. Not "I was lazy" — specifically: which day, which block, what came up, what was the trigger?

Common blockers:
- **External:** unexpected meeting, family emergency, illness
- **Energy:** Wednesday evening exhaustion (ego depletion, already flagged in `self-management.md`?)
- **Schedule:** primary block too early, secondary block too tired
- **Content:** the resource was harder than expected — needs a different approach
- **Motivation:** the topic feels less interesting than it did 3 weeks ago

For each blocker, propose an adjustment:

| Blocker | Adjustment |
|---|---|
| Wednesday evening dead | Move Wednesday primary work to Saturday; do flashcards only Wednesday |
| Object detection chapter too dense | Break into 3 sub-sessions instead of 1 |
| Motivation low on math plank | Revisit motivation anchors; integrate math into project work |

### Step 6 — Adjust next week's schedule

Produce a **concrete delta** to `schedule.md` for next week. Don't rewrite the whole schedule — write the diff.

- "Wednesday primary → Saturday morning. Wednesday becomes flashcards only."
- "Object detection plank: 3 sub-sessions across Tue/Thu/Sat instead of 1 on Thursday."
- "Friday Feynman check moved to Saturday 10:00 (Friday evening has been dead 3 weeks in a row)."

Adjustments must be specific. "Try harder next week" is not an adjustment.

### Step 7 — Check the pivot rule

Re-read `smart-goal.md` — the pivot rule. If you've now missed two weekly milestones in a row, the pivot rule fires. Options:

- **Scope down** (drop part of the SMART goal, keep the rest)
- **Extend timeline** (push the deadline by N weeks)
- **Reclassify** (the topic may not be what the user thought; restart with `topic-classifier`)

Do not let "fall further behind" be the default outcome. The pivot rule exists for this moment.

### Step 8 — Write the artifact

Append to `~/.self-learning-os/<topic>/reviews/weekly-<YYYY-Www>.md`:

```markdown
# Weekly Review: <topic> — Week ending <date>

## Planned vs Actual
| Day | Planned plank | Did it happen? | Notes |
|---|---|---|---|

## Weekly Feynman check status
- **Status:** done | skipped
- **Concept:** <if done>
- **Recovery action:** <if skipped, when it's rescheduled>

## Spaced reviews
- +1d due / done: <count>
- +3d due / done: <count>
- +7d due / done: <count>
- +21d due / done: <count>
- Missed reviews to reschedule: <list>

## Wins
- <specific moment>
- <specific moment>

## Blockers + adjustments
| Blocker | Adjustment |
|---|---|

## Next week's schedule delta
- <specific change to schedule.md>
- <specific change>

## Pivot rule check
- Milestones missed in past 2 weeks: <count>
- Pivot triggered? <yes / no>
- If yes: <scope-down | extend | reclassify> + specifics

---

```json
{
  "week_ending": "YYYY-MM-DD",
  "topic": "<topic>",
  "planned_vs_actual": [
    {"day": "Mon", "planned": "...", "completed": true, "notes": "..."}
  ],
  "weekly_feynman": {"done": true, "concept": "...", "recovery": null},
  "spaced_reviews": {"plus_1d": {"due": 3, "done": 3}, "plus_3d": {"due": 2, "done": 1}},
  "missed_reviews_rescheduled": ["..."],
  "wins": ["..."],
  "blockers_adjustments": [{"blocker": "...", "adjustment": "..."}],
  "schedule_delta": ["..."],
  "pivot_check": {"milestones_missed_2wk": 0, "pivot_triggered": false, "pivot_action": null}
}
```
```

### Step 9 — Hand back

Update `state.md` with the weekly review timestamp and the adjustments. Coordinator surfaces the next week's first session.

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **never**.

Weekly review operates entirely on the user's saved state and own reflection. No external information needed.

## What to avoid

- **Skipping the wins step.** Most users come into a review focused on what went wrong. Force a win surface.
- **Vague adjustments.** "Try harder" is not an adjustment. "Move Wednesday primary to Saturday" is.
- **Pretending nothing was missed.** If the Feynman check was skipped, name it. If three spaced reviews drifted, name them.
- **Ignoring the pivot rule.** If milestones are slipping, fire the pivot. Pretending it'll catch up next week is how 3-month plans become 6-month plans become abandonment.
- **Rewriting the whole schedule.** Write a diff, not a fresh document. The current schedule is mostly working — adjust at the edges.
- **Treating this as therapy.** It's a structured operational review. Emotional support comes from `confusion-endurance` if needed.

## What "good" looks like

A completed weekly review should make the user say: *"I know exactly what worked, what didn't, and what's different about next week. I also remember three specific moments of progress."*

## References

- *The Science of Self-Learning*, Ch.2 (SQ3R Review, generalized) + Ch.4 (Franklin reflection block + pivot rule)
- [EVIDENCE.md](../../EVIDENCE.md)
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-004 (gating)
