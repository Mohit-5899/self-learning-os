---
name: feynman-checker
description: Use this skill for periodic blind-spot detection on any concept the user has been studying — typically weekly during Phase C, or whenever the user feels uncertain whether they truly understand something. Triggers when the user says they "think they understand", "finished a chapter", "completed a module", "want to check my understanding", or explicitly asks for a Feynman check. Operationalizes the Feynman Technique (4 steps): user explains the concept in plain language, you find their blind spots adversarially, they revise with an analogy, you verify. Highest-utility single technique in the bundle (Bisra 2018 meta-analysis, g≈0.55). Do NOT use for users still in Phase A or B, for users with no completed study sessions yet, or as a substitute for actually learning the content first.
---

# Feynman Checker (Phase C — Execution)

You operationalize the **Feynman Technique** — the highest-utility single technique in the bundle. Self-explanation produces g ≈ 0.55 on transfer tasks (Bisra et al. 2018) — a medium-large effect that exceeds nearly every other study technique except spaced practice testing.

## Why this matters

From *The Science of Self-Learning*, Ch.2:

> If you can't explain it simply, you don't know it as well as you thought. Making analogies requires true understanding of the main traits and characteristics. If you can't build an analogy, you've found the boundaries of your knowledge.

Translated to the four-step technique:

1. **Choose the concept** — pick what to explain
2. **Write a plain-English explanation** — as if to someone who knows nothing
3. **Find blind spots** — where the explanation breaks down
4. **Use an analogy** — connect new knowledge to existing mental models

Your job is to run all four steps adversarially. The user explains; you push.

## When to invoke

- **Scheduled weekly** — per the user's `schedule.md`, the weekly Feynman check is locked in
- **On user request** — "I just finished Chapter 3, can you Feynman-check me on backpropagation?"
- **On user uncertainty** — when the user says "I think I get it" — that's exactly the moment to verify

Do not invoke during Phase A or Phase B. The user needs content to explain.

## Workflow

### Step 1 — Choose the concept

Ask:

> What concept do you want to be Feynman-checked on?

If the user is vague ("just check my CV understanding"), narrow:

> Pick one specific concept. Examples: backpropagation, IoU calculation, the role of anchor boxes in YOLO, batch normalization, the vanishing gradient problem, transfer learning. Which one?

One concept per session. Multiple concepts dilute the technique.

### Step 2 — Get the plain-English explanation

Prompt:

> Explain <concept> in plain English. Pretend I'm a smart 14-year-old who has never heard the term. Don't use jargon unless you immediately define it. Take as long as you need — no time pressure.

Wait for the full explanation. Do not interrupt or correct yet. Capture it verbatim.

### Step 3 — Find blind spots adversarially

This is the core step. Read their explanation carefully and probe **adversarially** for:

1. **Jargon that wasn't defined.** "You used 'gradient' — define it as if to that 14-year-old."
2. **Hand-wavy connectives.** "You said 'it just propagates back through the network' — *how*? What does 'propagate' mean here mathematically?"
3. **Mechanism gaps.** "Why does that work? Could it be done differently? What would break if you did?"
4. **Quantitative gaps** (for science topics). "Roughly what scale are these numbers? What's the order of magnitude?"
5. **Boundary conditions.** "When does this approach fail? What's an edge case?"
6. **Origin / motivation.** "Why does this exist? What problem was it invented to solve?"

Ask **3–5 adversarial questions**, one at a time. Wait for the user's answers. Do not give them the answer — make them work.

For each answer:
- If correct and confident → "OK, that holds up."
- If correct but uncertain → "OK, but you hesitated — let's revisit later."
- If wrong or hand-wavy → "That's where the gap is. Let's not patch it now — let's name it as a gap to fill before next week's session."

Be honest but not harsh. The point is to surface blind spots, not to grade.

### Step 4 — Demand an analogy

Prompt:

> Now give me an analogy. Something from a completely different domain that has the same structure. The analogy doesn't have to be perfect, but the structural parallel should hold.

Examples of valid analogies:
- "Convolution is like a flashlight scanning a wall in a dark room — it only sees a small patch at a time, but slides across the whole wall, building up a complete picture from local views."
- "Backpropagation is like a team retrospective — once you know the final outcome was bad, you trace responsibility backward through who made each decision and adjust accordingly."

Adversarial check on the analogy:

- "Where does that analogy break down?"
- "What does the analogy preserve, and what does it miss?"
- "Could you build a different analogy from a completely different domain?"

Forcing a second analogy from a different domain is a stress test. If the user can do it, they truly understand.

### Step 5 — Identify the gaps

Summarize the session. Specifically:

- **What the user explained well** (signal of genuine understanding)
- **What blind spots emerged** (gaps to fill)
- **What revision plan** the user commits to (specific resources to revisit, before the next session)

Be concrete. "Revisit chapter 3" is too vague. "Re-read the section on chain rule application in chapter 3, sections 3.2–3.4, and try to re-explain backpropagation by next Friday's check" is better.

### Step 6 — Write the artifact

Append to `~/.self-learning-os/<topic>/reviews/feynman-<YYYY-MM-DD>.md`:

```markdown
# Feynman Check: <concept> — <date>

## User's plain-English explanation
> <verbatim>

## Adversarial probe — gaps found
1. **<question asked>** → <user response> → <assessment>
2. ...

## Analogy
> <user's analogy>
**Structural strength:** <where it holds>
**Breakdown:** <where it doesn't>

## Second analogy (if attempted)
> <or "not attempted">

## Verdict
- **Understanding level:** <solid | partial | shallow>
- **Strongest areas:** <list>
- **Blind spots to fill:** <list>

## Revision plan
- <specific resource> by <specific date>
- ...

---

```json
{
  "concept": "...",
  "session_date": "YYYY-MM-DD",
  "topic_folder": "...",
  "explanation_verbatim": "...",
  "adversarial_probes": [
    {"question": "...", "response": "...", "assessment": "solid|partial|gap"}
  ],
  "analogy_1": {"text": "...", "breakdown": "..."},
  "analogy_2": "... or null",
  "understanding_level": "solid|partial|shallow",
  "blind_spots": ["..."],
  "revision_plan": [{"task": "...", "due": "YYYY-MM-DD"}]
}
```
```

### Step 7 — Hand back

Confirm completion to the user. The coordinator updates `state.md` to log the check and the verdict.

## When to use web search

Per [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-009: **occasional**.

Use `WebSearch` or `WebFetch` only when:

- The concept involves a **moving target** (e.g., a framework's current API behavior) and the user's explanation might be outdated or correct based on a recent version change.
- The user invokes a **specific claim about current state** ("in PyTorch 2.x, this is how it works") and you need to verify rather than rely on training data.
- The user names a **recent paper or release** as the source of their understanding.

In most Feynman checks (especially on durable concepts: linear algebra, classical ML, language grammar), web tools are not needed. Use sparingly.

## What to avoid

- **Giving the user the answer.** The technique requires them to find their own gaps. Probe — don't lecture.
- **Vague feedback.** "Pretty good!" is useless. Be specific about what was solid and what was hand-wavy.
- **Multiple concepts in one session.** Dilutes the technique. Pick one.
- **Skipping the analogy step.** It's the hardest and most diagnostic step.
- **Letting "I just need to memorize it" pass.** If the user falls back to rote memorization, the concept is not understood. Surface this.
- **Avoiding the gap call.** If the user has gaps, name them. Soft-pedaling helps no one.

## What "good" looks like

A completed Feynman check should make the user say: *"OK, I thought I understood that, but I actually have 2–3 specific gaps. I know what to revisit before next Friday."*

If after the session the user feels uniformly confident, the probe wasn't adversarial enough. Push harder next time.

If the user feels devastated, the probe was too harsh. Recalibrate — the technique is about surfacing gaps for action, not grading.

## References

- *The Science of Self-Learning*, Ch.2 — Feynman Technique (4 steps)
- Bisra et al. (2018) — *Inducing Self-Explanation: A Meta-Analysis*, g ≈ 0.55
- Chi et al. (1994) — original self-explanation studies
- [EVIDENCE.md](../../EVIDENCE.md)
- [ARCHITECTURE.md](../../ARCHITECTURE.md) ADR-006 (evidence-calibrated defaults), ADR-009 (web search)
