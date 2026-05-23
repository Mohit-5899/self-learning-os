---
name: confidence-audit
description: Use this skill after topic-classifier has run, to address Block 1 of the Learning Success Pyramid (Hollins, Ch.1) — the user's confidence and emotional readiness to learn this specific topic. Triggers when ~/.self-learning-os/<topic>/classification.md exists but confidence.md does not. Surfaces the user's prior learning wins, names anticipated fears about this topic, builds concrete confidence anchors they can return to when motivation dips, and locks a progress-marker plan. Required before smart-goal-setter or learning-planner. Do NOT use for general emotional support, life coaching, or therapy — strictly scoped to confidence-to-learn-this-topic. Do NOT use for users who haven't completed topic-classifier yet.
---

# Confidence Audit (Phase A — Pyramid Block 1)

You address the **emotional readiness** to learn this specific topic, before any time-management work or content planning.

## Why this matters

From *The Science of Self-Learning*, Ch.1 — the Learning Success Pyramid:

> Information first hits the emotional center of the brain, which determines if it's a threat. If a threat is perceived, brain chemicals are diverted to fight-or-flight — leaving nothing for learning. Fear, criticism, depression, or stress literally make learning impossible neurologically.

This is the **first block of the pyramid** for a reason. Users with strong intent but unaddressed fears (about ability, time, failure, peer comparison) will burn out within 2–4 weeks. Your job is to surface those fears before they cause damage, and build durable anchors the user can return to.

This is **not therapy**. It is a structured surfacing of:
1. **Existing knowledge** that connects to the topic
2. **Past learning wins** that prove the user has done hard learning before
3. **Anticipated fears** about this specific journey
4. **Confidence anchors** — specific statements the user can re-read when motivation dips
5. **A progress-marker plan** — how progress will be made visible week to week

## When to invoke

The coordinator calls you when `classification.md` exists but `confidence.md` does not. Do not invoke yourself.

## Workflow

### Step 1 — Read prior context

Read `classification.md`. You now know:
- The topic and its scope
- Whether it's art / science / hybrid
- The self-learning suitability and any external-feedback requirements

This informs what fears are likely realistic. For low-suitability art topics, "I'm worried I'll plateau without a teacher" is a legitimate fear, not catastrophizing.

### Step 2 — Surface existing knowledge

Ask the user:

> Before this journey, what do you already know that connects to **<topic>**? Even loosely — adjacent fields, related hobbies, tools you've used, books you've half-read.

Build a list. The point is to break the "I'm starting from zero" illusion that crushes motivation.

For Computer Vision example, an AI engineer might say:
- Python (daily use)
- Deep learning fundamentals
- LangGraph / multi-agent systems
- Linear algebra (rusty)
- Some PyTorch experience

That's not zero. That's a strong scaffold.

### Step 3 — Surface past learning wins

Ask:

> Name 2–3 things you taught yourself in the past that turned out well. What did they have in common? How long did each take?

Capture for each:
- **What was learned**
- **Roughly how long**
- **Method that worked** (textbook? courses? building? mentors?)

This is high-leverage. The user's *own* track record beats any external pep talk. They have done hard learning before; they will do it again.

### Step 4 — Surface anticipated fears

Ask directly:

> What scares you about this journey? Be specific. "I don't have time" and "I'll lose motivation" and "I'm not smart enough for this" are all valid — name them.

Common fears to probe for if not raised:
- "I won't have time" → will be addressed in Phase A Block 2 (self-management)
- "I'll get bored / lose motivation" → motivation anchors in learning-planner; confusion-endurance skill on standby
- "I'm not smart enough" → past wins are the counter-evidence
- "I'll fall behind people doing this faster" → reframe: the only comparison that matters is your own week-over-week
- "I'll start and quit again like last time" → ask what was different last time; design around that
- (Art topics) "I'll plateau without a teacher" → if suitability ≠ high, surface in external-feedback plan

Do not dismiss any fear. Acknowledge each, then plan around it.

### Step 5 — Build confidence anchors

For each fear, write **one specific statement** the user can re-read in 6 weeks when that fear resurfaces. Anchors must be:

- **Specific to this user, not generic** ("You've learned <X> from scratch in <N> months — same process")
- **Falsifiable** ("If you put in 8h/week for 4 weeks and have made no progress, then re-evaluate")
- **Actionable, not affirmational** ("I am strong" ≠ anchor. "Re-read your Week 1 plan and ask if you've put in the agreed hours" = anchor.)

Aim for 3–5 anchors.

### Step 6 — Lock a progress-marker plan

Ask:

> Three months from now, how would you *know* you've made real progress? What would be visible — to you and to anyone else who looked?

Capture 3–5 markers. Examples:
- Computer Vision: "A working image classifier I built end-to-end, deployed somewhere I can show people"
- Spanish: "A 10-minute unscripted conversation with a native speaker, on a topic I picked"
- Trading: "A backtested system with 6+ months of paper-trading results"

These become inputs to `smart-goal-setter` later. For now, they serve to make the user's success **visible** to themselves.

### Step 7 — Write the artifact

Save to `~/.self-learning-os/<topic>/confidence.md`:

```markdown
# Confidence Audit: <topic>

## Existing knowledge that connects
- <item>
- <item>

## Past learning wins
- **<what>** — <duration>, via <method>
- **<what>** — <duration>, via <method>

## Anticipated fears (named, not dismissed)
- <fear> → addressed by: <anchor or skill>
- <fear> → addressed by: <anchor or skill>

## Confidence anchors (re-read these when motivation dips)
1. <anchor>
2. <anchor>
3. <anchor>

## Progress markers (what success looks like at 3 months)
- <marker>
- <marker>

---

```json
{
  "existing_knowledge": ["..."],
  "past_wins": [
    {"learned": "...", "duration_months": <N>, "method": "..."}
  ],
  "anticipated_fears": [
    {"fear": "...", "addressed_by": "..."}
  ],
  "confidence_anchors": ["..."],
  "progress_markers_3_month": ["..."]
}
```
```

### Step 8 — Hand back

Confirm completion. Coordinator routes to `self-management-setup`.

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **never**.

This skill operates entirely on the user's personal history, fears, and progress vision. No external information is needed. If the user mentions a specific framework or course while describing a past win ("I did the fast.ai course in 2024"), capture it as-is — do not verify whether the course still exists. The point is the user's experience, not current resource availability.

## What to avoid

- **Generic affirmations.** "You can do it!" is not an anchor.
- **Dismissing fears.** Every fear gets named and addressed.
- **Therapy mode.** This is about confidence to learn *this topic*, not life-wide.
- **Skipping past wins.** They are the most important step — most users underestimate their own track record.
- **Letting the user invent unrealistic 3-month markers.** Push back if a marker is unmeasurable or too ambitious for their time budget (the next skill will surface real time available).

## What "good" looks like

A completed `confidence.md` should make the user say: *"Right, I actually have done hard things before. And the things I'm scared of have plans attached. Let's go."*

If after this skill the user is *less* confident, you did it wrong — likely you spent too much time on fears and not enough on past wins.

## References

- *The Science of Self-Learning*, Ch.1, "The Learning Success Pyramid — Block 1: Confidence"
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-004 (gating)
