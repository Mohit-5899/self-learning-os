# Dunlosky 2013 — Technique Utility Reference

> **Source.** Dunlosky, J., Rawson, K. A., Marsh, E. J., Nathan, M. J., & Willingham, D. T. (2013). *Improving students' learning with effective learning techniques: Promising directions from cognitive and educational psychology.* **Psychological Science in the Public Interest**, 14(1), 4–58.
>
> This reference is loaded on demand by `learning-planner` when assigning default techniques to a plank. Also referenced by `franklin-scheduler`.

## The hierarchy

| Technique | Utility | Use as default? | Notes |
|---|---|---|---|
| **Practice testing** (retrieval practice, self-quizzing) | HIGH | ✅ YES | Strongest evidence base. Default for any plank with verifiable problems. |
| **Distributed practice** (spaced repetition) | HIGH | ✅ YES | Applied as 1d/3d/7d/21d cadence. See Cepeda 2006. |
| **Elaborative interrogation** ("why" questions) | MEDIUM | ✅ YES | Operationalized in the question bank. |
| **Self-explanation** (Feynman-style) | MEDIUM-HIGH (g≈0.55 per Bisra 2018) | ✅ YES | Highest single-technique effect size. Weekly cadence default. |
| **Interleaved practice** | MEDIUM | ✅ YES | Built into the rotating-planks design. |
| **Summarization** | LOW | ⚠️ ONLY with recall questions attached | Pure summaries don't transfer. The cues + recall column in Cornell is what does the work. |
| **Highlighting / underlining** | LOW | ❌ NO | Never default. |
| **Keyword mnemonic** | LOW | ❌ NO (for general use) | Domain-specific use only (e.g., language vocab in early stages). |
| **Imagery for text** | LOW | ❌ NO | Inconsistent benefits. |
| **Rereading** | LOW | ❌ NO | The biggest illusion-of-knowing trap. Replace with retrieval. |

## How to apply when assigning plank defaults

For each plank in the plan:

1. **Identify the verifiable activity.** Can the user solve a problem, write code, produce an artifact, recite a translation? If yes → practice testing is the default.
2. **Identify the conceptual gap.** Is the user trying to understand a mechanism, theory, or relationship? If yes → self-explanation (Feynman) is the default.
3. **Add elaborative interrogation.** Every plank should have ≥3 "why" or "how" questions in the question bank.
4. **Schedule distributed practice.** Every new concept gets 1d/3d/7d/21d review checkpoints in `franklin-scheduler`.
5. **Interleave planks across the week.** Do not do the same plank 5 days in a row — rotate.
6. **Never assign highlighting, rereading, or pure summarization as a primary technique.**

## What to say if the user objects

If the user says "but I learn best by rereading the textbook":

> The largest meta-analysis in this field (Dunlosky et al. 2013, PSPI) found that rereading is in the lowest-utility tier — most students who do it feel like they're learning but score significantly worse on delayed tests. The illusion of fluency from rereading is well-documented. Want to try replacing rereading with self-quizzing for week 1 and see if it changes your retention?

If they want to override, log it explicitly in the plan artifact under `user_overrides`. Don't fight forever — the user owns the system. But document the override so they can revisit.

## What to say if the user wants more techniques

The book mentions techniques the meta-analysis doesn't strongly endorse (e.g., elaborate mnemonics for non-vocabulary use, extensive imagery). Defer to the evidence: include them only if the user explicitly requests, never as default.

## Future updates

This reference is versioned. If a newer meta-analysis updates the rankings, write a new entry in `EVIDENCE.md` and update this file with a `last_reviewed` date.

**Last reviewed:** 2026-05-23. Current as of the Bisra 2018 meta-analytic extension for self-explanation. No newer comprehensive meta-analysis supersedes Dunlosky 2013 as of this date — verify with `WebSearch` if more than 2 years have passed.
