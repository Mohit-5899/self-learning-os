---
name: franklin-scheduler
description: Use this skill after learning-planner, to convert the user's planks, weekly learning hours, and primary/secondary blocks into a concrete weekly schedule with Franklin-style daily/weekly cadence. Triggers when ~/.self-learning-os/<topic>/plan.md exists but schedule.md does not. Produces a weekly grid mapping each day's blocks to specific planks, overlays an evidence-backed spaced-repetition review cadence (1d / 3d / 7d / 21d per new concept) plus a weekly Feynman check, and respects all non-negotiables locked in self-management.md. Last skill of Phase B — after this, the user enters Phase C execution. Do NOT use for one-off study sessions, for general calendar advice, for users still in Phase A, or for users without a completed learning plan.
---

# Franklin Scheduler (Phase B — Planning, final step)

You produce the **concrete weekly schedule** the user will execute. After this skill, Phase B is complete and the user enters daily/weekly execution (Phase C).

## Why this matters

From *The Science of Self-Learning*, Ch.4 — Benjamin Franklin's daily schedule:

> Once decisions are removed with a detailed schedule, you're far more likely to follow through. Simply having something to refer to with premade decisions provides guidance that wouldn't otherwise exist.

The schedule does two jobs:

1. **Removes decisions** — the user shouldn't be choosing "what to study" at 6:30 AM with low willpower. The schedule has already chosen.
2. **Enforces evidence-backed cadence** — spaced repetition intervals (1d / 3d / 7d / 21d) per Cepeda 2006; weekly Feynman check per Bisra 2018; rotating planks per Dunlosky's interleaving recommendation.

You inherit Franklin's design philosophy: **scheduled, not improvised**. But you do *not* replicate Franklin's 5 AM rise — the schedule reflects the user's own time audit, not an idealized polymath's day.

## When to invoke

The coordinator calls you when `plan.md` exists but `schedule.md` does not.

## Workflow

### Step 1 — Read prior context

Read all prior artifacts:
- `classification.md` — topic type, adjustments needed (science = practice-testing blocks; art = production blocks)
- `self-management.md` — primary block, secondary block, weekend session, non-negotiables, ego-depletion risks, weekly hours
- `plan.md` — planks list, week-1 survey list, default techniques per plank, motivation anchors

### Step 2 — Map planks to days (rotation)

Assign one **primary plank per weekday**. Rotate across 5–6 planks so each gets ~1 day per week.

Example for Computer Vision (5 planks):

| Day | Primary plank |
|---|---|
| Mon | Math foundation |
| Tue | Image processing |
| Wed | Deep learning for vision |
| Thu | Object detection |
| Fri | Hands-on project |
| Sat | Review + integration (cross-plank) |
| Sun | Optional buffer / catch-up |

This is **interleaved practice** (Dunlosky MEDIUM utility) — not block practice. The user is not allowed to do "math all week, then CV all week." Interleaving wins on transfer.

### Step 3 — Build the daily block structure

For each weekday, allocate the user's `primary_block` and `secondary_block` from `self-management.md`:

**Primary block (deep work, e.g., 06:30–08:30):**
- 00–50 min: Active study on the day's plank (SQ3R / problem-solving / production)
- 50–60 min: 10-min break (per book Ch.3 — 50 min focus, then break)
- 60–90 min: Practice testing on the same plank (problems, code, drills)
- 90–120 min: Cornell-style notes (v1.1) OR review of yesterday's concepts (retrieval practice)

**Secondary block (light, e.g., 21:00–21:30):**
- 00–15 min: Spaced-repetition review of due cards (concepts at 1d / 3d / 7d / 21d intervals)
- 15–30 min: Plan tomorrow's primary block (pick the exact resource, problem, or task)

Adjust block lengths to the user's actual availability. If their primary block is only 60 minutes (not 120), compress: 40 min active + 5 min break + 15 min practice testing.

### Step 4 — Overlay spaced-repetition intervals

For every new concept introduced in the plan, schedule reviews at:

- **+1 day** — quick check, can it be recalled at all?
- **+3 days** — deeper test, can it be applied?
- **+7 days** — Feynman-style explain-back
- **+21 days** — full integration check (cross-plank if relevant)

These reviews live in the secondary block. The user does not need to track this manually — the scheduler produces the review queue per week.

This default cadence comes from Cepeda et al. (2006) — meta-analysis of 184 studies on the spacing effect. Don't let the user pick arbitrary intervals.

### Step 5 — Lock the weekly Feynman check

Per Bisra et al. (2018) — self-explanation produces g ≈ 0.55 (medium-large effect). Schedule a **weekly Feynman check** at a specific recurring slot. Default: Friday 21:00 or Sunday morning weekend session, ~30 minutes.

The Feynman check picks **one concept** from the week and runs the `feynman-checker` skill against it. This is non-negotiable in the schedule — if the user skips it, surface it next week.

### Step 6 — Mark the weekend session

Saturday or Sunday (per `self-management.md`) is the longer session — 90–120 minutes. Use it for:

- **Cross-plank integration** — work that combines two or more planks
- **Project-building** — the SMART-goal deliverable advances here
- **Weekly review** (v1.1) — what worked, what didn't, what to adjust

Do not schedule the weekend session as "more of the same weekday work." It should be qualitatively different — broader, builder-oriented.

### Step 7 — Respect non-negotiables and ego-depletion

Read `self-management.md` carefully:

- Non-negotiables (family dinner, sleep) — schedule must never overlap. Period.
- Ego-depletion risks — on days flagged as high-depletion-risk, the secondary block becomes ultra-light (5-minute Feynman attempt as the fallback minimum, no new content).

If the user's audit said "Wednesday evenings are dead after work meetings," the Wednesday secondary block is "5-min spaced review only."

### Step 8 — Build the kill-switch / fallback minimum

For every block, define the **fallback minimum** — the smallest version of this block that still counts. From `self-management.md` step 7. Examples:

- **Primary block fallback:** "20 minutes of just one Feynman attempt on a recent concept" (preserves the habit even on a hard day)
- **Secondary block fallback:** "Read one flashcard. That's it." (preserves the streak)

This makes the schedule **robust to bad days** rather than collapsing when life intervenes.

### Step 9 — Write the artifact

Save to `~/.self-learning-os/<topic>/schedule.md`:

```markdown
# Weekly Schedule: <topic>

## Plank rotation
| Day | Primary plank | Secondary focus |
|---|---|---|
| Mon | <plank> | spaced review |
| ... | | |

## Daily block structure
### Primary block (<time>)
- 00–50 min: <activity, current plank>
- 50–60 min: 10-min break
- 60–<end>: <activity>

### Secondary block (<time>)
- 00–15 min: spaced review (1d/3d/7d/21d due queue)
- 15–<end>: plan tomorrow

## Spaced-repetition review queue (sample week)
- Day 1 (Mon): no reviews yet (first day)
- Day 2 (Tue): review Monday's concepts (+1d)
- Day 4 (Thu): review Monday's concepts (+3d)
- Day 8 (Mon, next week): review Monday's concepts (+7d)
- Day 22: review Monday's concepts (+21d)

## Weekly Feynman check
- **<day, time>** — pick one concept from the week, run feynman-checker

## Weekend session
- **<day, time, length>** — cross-plank integration / project / review

## Fallback minimums (when life intervenes)
- Primary block fallback: <minimum>
- Secondary block fallback: <minimum>

## Non-negotiables (scheduler MUST never overlap)
- <list from self-management.md>

## Ego-depletion adjustments
- <day-specific lightening>

---

```json
{
  "weekly_grid": [
    {
      "day": "Mon",
      "primary_plank": "...",
      "blocks": [
        {"time": "06:30-07:20", "activity": "active study", "technique": "sq3r_or_problems", "plank": "..."},
        {"time": "07:20-07:30", "activity": "break", "technique": "break"},
        {"time": "07:30-08:00", "activity": "practice testing", "technique": "practice_testing", "plank": "..."},
        {"time": "08:00-08:30", "activity": "notes", "technique": "cornell_with_recall"},
        {"time": "21:00-21:15", "activity": "spaced review", "technique": "distributed_practice"},
        {"time": "21:15-21:30", "activity": "plan tomorrow", "technique": "planning"}
      ]
    }
  ],
  "review_cadence": {
    "intervals_days": [1, 3, 7, 21],
    "weekly_feynman": {"day": "Fri", "time": "21:00", "duration_min": 30}
  },
  "weekend_session": {"day": "Sat", "time": "10:00-11:30", "purpose": "cross_plank_integration"},
  "fallback_minimums": {
    "primary_block": "20 min Feynman attempt on a recent concept",
    "secondary_block": "one flashcard"
  },
  "non_negotiables_respected": ["..."],
  "ego_depletion_adjustments": {"Wed_evening": "ultra-light only"}
}
```
```

### Step 10 — Hand back

Confirm completion. Coordinator marks Phase B complete. The user is now ready for Phase C execution — daily blocks per the schedule, plus weekly Feynman checks.

Suggest the next step:

> Your schedule is locked in. Tomorrow morning at <primary block time>, start with <plank>. On <Feynman day> at <time>, run the feynman-checker on the most important concept of the week. Want me to help you with the first session now?

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **never**.

The scheduling math (spacing intervals, block structure, plank rotation) is fixed by evidence. No external information is needed at scheduling time. Resources were already verified by `learning-planner`.

## What to avoid

- **Inventing review intervals.** Always 1/3/7/21 unless explicit override by the user with rationale.
- **Scheduling deep work in a known low-energy block.** If the user's audit flagged late evenings as ego-depleted, do not schedule new concept introduction there.
- **Letting the user skip the weekly Feynman check.** It's the highest-utility single technique.
- **Block-practicing one plank for a week.** Interleave or rotate; don't ever go 5 days on the same plank.
- **Overlapping non-negotiables.** Re-read `self-management.md`. The scheduler is gated by those commitments.
- **Producing a perfect schedule with no fallback minimums.** Real life will hit. Plan for it.

## What "good" looks like

A completed `schedule.md` should make the user say: *"I know exactly what to do at 6:30 AM tomorrow, I know what to do if life kicks me in the teeth on Thursday, and I know what's on Friday at 21:00."*

## References

- *The Science of Self-Learning*, Ch.4 (Franklin schedule, virtues-style rotation, recreation respected)
- Cepeda et al. (2006) — spacing effect; review intervals
- Bisra et al. (2018) — self-explanation; weekly Feynman cadence
- Dunlosky et al. (2013) — interleaved practice; technique defaults
- [EVIDENCE.md](../../EVIDENCE.md)
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-006 (evidence-calibrated defaults)
