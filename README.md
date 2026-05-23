# Self-Learning OS for Claude

> Turn Claude into a personal learning coach for **any** topic — language, code, trading, music, science — in a way that the science of learning actually backs.

A Claude Skills bundle and plugin that walks any user through a complete, personalized self-directed learning journey. Built on Peter Hollins' *The Science of Self-Learning* and **calibrated against the cognitive-science evidence** of what actually works.

## What this is

Most "learn anything in 30 days" plans fail for the same reason Jorge fails in Chapter 2 of the book: people skip the foundation (confidence + self-management) and jump straight into resources. They also default to the wrong techniques — highlighting, rereading, and pure summarization — which the Dunlosky 2013 meta-analysis ranks as **low-utility**.

This bundle prevents both mistakes. It walks the user through the book's exact order:

1. **Diagnose** the topic and the learner (art-vs-science, confidence, self-management)
2. **Plan** the journey (SMART goal, weekly planks, schedule)
3. **Execute** with **evidence-backed** techniques (Feynman > Cornell-with-recall > SQ3R-with-Recite-Review)

And it persists state, so the coach knows on day 14 what you did on day 3.

## What's inside (v1)

| Phase | Skill | Purpose |
|---|---|---|
| Coordinator | `self-learning-coach` | Reads user state, routes to the next ungated skill |
| A — Diagnostic | `topic-classifier` | Art vs science breakdown — changes the whole approach |
| A — Diagnostic | `confidence-audit` | Pyramid Block 1 — surfaces prior wins, names fears |
| A — Diagnostic | `self-management-setup` | Pyramid Block 2 — time audit, schedule, routines |
| B — Planning | `smart-goal-setter` | One 3-month SMART goal with measurable milestones |
| B — Planning | `learning-planner` | Weekly planks, week-1 survey list, question bank, motivation anchors |
| B — Planning | `franklin-scheduler` | Daily/weekly schedule with evidence-backed review cadence (1d/3d/7d/21d) |
| C — Execution | `feynman-checker` | Weekly adversarial blind-spot check (g≈0.55 effect size) |

Deferred to v1.1: `cornell-notes`, `sq3r-session`, `confusion-endurance`, `weekly-review`.

## Why this order

The book is explicit (Ch.1): information first hits the emotional center of the brain. Under threat (stress, fear, overwhelm), brain chemistry diverts to fight-or-flight and learning becomes neurologically impossible. Self-management exhaustion ("ego depletion") collapses attention. **Only after both blocks are addressed does the hippocampus get to do its job.**

So the coach refuses to skip blocks. You cannot reach `learning-planner` until `self-management-setup` is done. You cannot reach `feynman-checker` until a plan exists.

## Why these techniques (and not others)

| Technique | Dunlosky 2013 utility | Status in v1 |
|---|---|---|
| Practice testing | HIGH | ✅ Default (feynman, scheduler review cadence) |
| Distributed practice | HIGH | ✅ Default (1d/3d/7d/21d intervals) |
| Self-explanation | HIGH | ✅ Default (feynman-checker is the highest-value skill) |
| Elaborative interrogation | MEDIUM | ✅ In learning-planner question bank |
| Interleaved practice | MEDIUM | ✅ In franklin-scheduler weekly rotation |
| Highlighting | LOW | ❌ Not recommended |
| Rereading | LOW | ❌ Not recommended |
| Pure summarization | LOW | ⚠️ Only allowed with recall questions attached |

This is the most honest deviation from the book: Hollins leans hard on Cornell summaries, but the evidence says the **recall column** is what does the work — not the summary. Our Cornell skill (v1.1) will enforce that.

## Install (three paths)

### Path 1 — Claude Code plugin (recommended for developers)

```bash
# Once a marketplace listing exists:
/plugin install self-learning-os@Mohit-5899

# Or directly from this repo:
git clone https://github.com/Mohit-5899/self-learning-os ~/.claude/plugins/self-learning-os
```

### Path 2 — Manual copy (any Claude client)

```bash
git clone https://github.com/Mohit-5899/self-learning-os
cp -r self-learning-os/skills/* ~/.claude/skills/
```

### Path 3 — claude.ai upload

Upload the contents of `skills/` to your claude.ai project's skill panel (paid plans). See Anthropic's [skills documentation](https://docs.claude.com/) for the current flow.

## Quick start

After install, just say what you want to learn:

```
I want to learn computer vision. I work 10–7, have family time 7–8:30, 
and run a side business. Where do we start?
```

The coach will route you into `topic-classifier` first, then walk you through the rest in order. Your state lives in `~/.self-learning-os/<topic>/`.

## Status

**v0.1 — In development.** Building in public. PRs, issues, and feedback welcome.

- [ ] v1: Phase A + B end-to-end + `feynman-checker`
- [ ] v1.1: `cornell-notes`, `sq3r-session`, `weekly-review`, `confusion-endurance`
- [ ] v1.2: Optional MCP server for Anki/Obsidian export
- [ ] v2: Subagent for parallel topic management

## Credits

Built on:

- **Peter Hollins**, *The Science of Self-Learning* — the framework backbone
- **Dunlosky et al. (2013)**, *Psychological Science in the Public Interest* — the technique-utility hierarchy
- **Roediger & Karpicke (2006)**, *Psychological Science* — test-enhanced learning
- **Cepeda et al. (2006)**, *Psychological Bulletin* — distributed practice spacing
- **Chi et al. (1994)** + **Bisra et al. (2018)** — self-explanation effect
- **Anthropic Skills team** — the SKILL.md format and `skills/skill-creator` authoring loop

See [EVIDENCE.md](./EVIDENCE.md) for full citations.

## License

MIT — see [LICENSE](./LICENSE).

---

By [Quanta AI Labs](https://quantaailabs.com).
