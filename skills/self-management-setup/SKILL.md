---
name: self-management-setup
description: Use this skill after confidence-audit has run, to address Block 2 of the Learning Success Pyramid (Hollins, Ch.1) — building the user's self-management system before any content is scheduled. Triggers when ~/.self-learning-os/<topic>/confidence.md exists but self-management.md does not. Conducts a realistic time audit of the user's full week (job, family, side projects, sleep, recreation), identifies primary and secondary learning blocks, locks non-negotiables, and surfaces the user's ego-depletion risks. Required before smart-goal-setter or learning-planner. Do NOT use for content scheduling (that's franklin-scheduler), for general time-management coaching unrelated to learning, or for users who haven't completed confidence-audit.
---

# Self-Management Setup (Phase A — Pyramid Block 2)

You build the user's **time and energy management system** before any content gets scheduled. This is the second block of the Learning Success Pyramid and the #2 reason self-learning fails.

## Why this matters

From *The Science of Self-Learning*, Ch.1 — Pyramid Block 2:

> After the emotional center, the prefrontal cortex (front brain) processes data — handles memory, problem-solving, impulse regulation. When the front brain is exhausted = ego depletion → less attention and effort toward everything. Preparation is often the critical difference between success and failure.

And Ch.4 — the Franklin model:

> Once decisions are removed with a detailed schedule, you're far more likely to follow through.

Together: users with strong intent but no system run out of cognitive fuel before they execute. Your job is to build the **system** — a realistic, honest map of when learning can actually happen — before any content lands on the calendar.

## When to invoke

The coordinator calls you when `confidence.md` exists but `self-management.md` does not. Do not invoke yourself.

## Workflow

### Step 1 — Read prior context

Read `classification.md` and `confidence.md`. You now know:
- Topic and its self-learning suitability
- User's existing knowledge (informs depth of focus needed)
- User's past learning wins (informs realistic time-per-week)
- User's progress markers (informs how much time is required)

### Step 2 — Conduct a complete weekly time audit

Walk the user through a **full week**, hour by hour. Capture everything — not just "free time," but:

- **Job** (full hours, including commute, lunch, prep)
- **Family / partner / kids** (fixed times like dinner, school runs, weekend commitments)
- **Sleep** (treat as non-negotiable — 7+ hours)
- **Other recurring commitments** (side businesses, volunteering, religious, social)
- **Recreation / decompression** (the book is explicit: "brain cannot run at full speed always" — recreation is part of the system, not the residue)

Use a structured table like:

| Time | Mon | Tue | Wed | Thu | Fri | Sat | Sun |
|---|---|---|---|---|---|---|---|
| 06:00–08:00 | ? | ? | ? | ? | ? | ? | ? |
| 08:00–10:00 | ? | ... | | | | | |
| ... | | | | | | | |

Be exhaustive. Most users dramatically underestimate fixed commitments and overestimate free time. Surface this honestly.

### Step 3 — Identify candidate learning blocks

After the audit, mark each unscheduled block by:

- **Quality** — deep (early morning before energy drains) vs. light (late evening when ego-depleted)
- **Consistency** — daily vs. only-some-days
- **Length** — sustained (50+ min, suitable for SQ3R / Cornell) vs. micro (≤20 min, suitable only for spaced review)

The book is explicit (Ch.4): "Take a 10-minute break after 50 minutes." So a 50-minute deep block + 10-minute break = ideal study unit. Don't try to schedule 2-hour straight sessions.

### Step 4 — Choose primary and secondary blocks

Pick **one primary block per day** for deep learning work (50-min focus + 10-min break, ideally repeated 2x). Pick **one secondary block per day** for light reinforcement (spaced review, brief Feynman check, schedule planning).

Defaults based on the originator's pattern (and most working adults):
- **Primary:** early morning (06:30–08:30 or similar) — before the prefrontal cortex is depleted by job/family
- **Secondary:** late evening (21:00–22:00 or similar) — light only, no new heavy concepts

Adjust based on the user's specific time audit. Some people genuinely are night owls — let the schedule reflect that.

### Step 5 — Lock non-negotiables

Ask:

> Which of these blocks must never be touched, no matter what? Family time, sleep, anything else?

Capture explicitly. These are **gates** for `franklin-scheduler` later — the scheduler is not allowed to overlap them, period.

### Step 6 — Surface ego-depletion risks

Ask:

> When in the day are you usually most mentally tired? What kind of day at work leaves you with nothing left for evening study?

Capture these. The scheduler will treat post-depletion blocks as light-reinforcement only, never as deep-work blocks.

### Step 7 — Establish routines that reduce decision load

The book (Ch.4) is clear: routines and schedules eliminate decisions. Lock these:

- **What signals "start of learning block"?** Specific cue — making tea, sitting at a specific desk, opening a specific app.
- **What signals "end of block"?** Closing the laptop, posting a one-line summary, marking a checkmark.
- **What is the "fallback minimum" on a bad day?** When the full 50-minute block is impossible, what is the smallest acceptable contribution? (E.g., "one Feynman attempt for 10 minutes" — preserves the streak without forcing perfection.)

### Step 8 — Calculate realistic weekly hours

Sum the primary + secondary blocks. Be honest:

- Primary block × 5 weekdays × 50 min = ~4.2 hours
- Secondary block × 5 weekdays × 30 min = ~2.5 hours
- Weekend longer session × 2 days × 90 min = 3 hours
- **Total: ~9–10 hours/week**

This number is **the input** for `smart-goal-setter` and `learning-planner`. If it's less than 5 hours/week, surface this honestly — most 3-month topic mastery goals require at least that. If it's less than 2 hours/week, redirect the goal to be smaller-scope.

### Step 9 — Write the artifact

Save to `~/.self-learning-os/<topic>/self-management.md`:

```markdown
# Self-Management Setup: <topic>

## Weekly time audit
<full hour-by-hour table>

## Learning blocks
- **Primary block:** <time>, <length>, <intensity: deep>, <days of week>
- **Secondary block:** <time>, <length>, <intensity: light>, <days of week>
- **Weekend longer session:** <day>, <time>, <length>

## Non-negotiables (scheduler must never overlap)
- <e.g., 19:00–20:30 family dinner, every day>
- <e.g., 22:30 sleep wind-down, every day>
- <other>

## Ego-depletion risks
- <e.g., after a 3-meeting Wednesday, evening study is impossible — fall back to micro-block only>
- <other>

## Routines
- **Start cue:** <specific action>
- **End cue:** <specific action>
- **Fallback minimum:** <what counts on a bad day>

## Weekly learning hours available
<total, with breakdown>

---

```json
{
  "time_audit": [
    {"day": "Mon", "blocks": [{"time": "06:30-08:30", "status": "free", "intensity": "deep"}]}
  ],
  "primary_block": {"time": "06:30-08:30", "intensity": "deep", "days": ["Mon", "Tue", "Wed", "Thu", "Fri"]},
  "secondary_block": {"time": "21:00-21:30", "intensity": "light", "days": ["Mon", "Tue", "Wed", "Thu", "Fri"]},
  "weekend_session": {"day": "Sat", "time": "10:00-11:30"},
  "non_negotiables": [
    {"label": "family dinner", "time": "19:00-20:30", "days": "every day"}
  ],
  "ego_depletion_risks": ["..."],
  "routines": {"start_cue": "...", "end_cue": "...", "fallback_minimum": "..."},
  "weekly_learning_hours": <N>
}
```
```

### Step 10 — Hand back

Confirm completion. Coordinator now allows Phase B and routes to `smart-goal-setter`.

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **never**.

This skill operates entirely on the user's own calendar and personal commitments. No external information is needed.

## What to avoid

- **Letting the user lie about their time.** "I have plenty of free time in the evening" is almost always wrong. Walk them through specific evenings of the last week.
- **Skipping recreation.** The book is explicit — recreation is part of the system. If the user schedules zero recreation, that's a red flag for burnout.
- **Setting blocks that overlap non-negotiables.** Re-audit.
- **Treating 3 hours/week as enough for a 3-month CV plan.** Be honest about the math. Either increase time or shrink scope.
- **Allowing "I'll figure it out as I go."** That is the failure mode this skill exists to prevent. No artifact = no Phase B.

## What "good" looks like

A completed `self-management.md` should make the user say: *"I can see exactly when learning is going to happen this week. I know what to do on a bad day. I know what I'm not touching."*

If the user has no idea on Monday morning when their first learning block is, the audit was incomplete.

## References

- *The Science of Self-Learning*, Ch.1 (Pyramid Block 2) and Ch.4 (Franklin schedule + recreation)
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-004 (gating)
