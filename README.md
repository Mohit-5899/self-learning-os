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

## What's inside

12 skills in total — coordinator + 3 diagnostic + 3 planning + 5 execution/intervention.

| Phase | Skill | Purpose |
|---|---|---|
| Coordinator | `self-learning-coach` | Reads user state, routes to the next ungated skill |
| A — Diagnostic | `topic-classifier` | Art vs science breakdown — changes the whole approach |
| A — Diagnostic | `confidence-audit` | Pyramid Block 1 — surfaces prior wins, names fears |
| A — Diagnostic | `self-management-setup` | Pyramid Block 2 — time audit, schedule, routines |
| B — Planning | `smart-goal-setter` | SMART goal with milestones + pivot rule — **HTML goal card** |
| B — Planning | `learning-planner` | Planks, week-1 survey, question bank, motivation anchors — **HTML plan dashboard** |
| B — Planning | `franklin-scheduler` | Daily/weekly schedule with 1d/3d/7d/21d review cadence — **HTML calendar grid** |
| C — Execution | `sq3r-session` | Guided Survey→Question→Read→Recite→Review pass; mandatory retrieval steps |
| C — Execution | `cornell-notes` | Coach or Assist mode → **HTML notes artifact** + mandatory `recall_questions` field |
| C — Execution | `feynman-checker` | Weekly adversarial blind-spot check (g≈0.55 effect size) |
| C — Cadence | `weekly-review` | Sunday cross-resource consolidation + pivot-rule check — **HTML metrics dashboard** |
| Any phase | `confusion-endurance` | On-demand intervention when the user signals frustration or wanting to quit |

**Visual artifacts.** Five skills render styled, self-contained, print-ready HTML artifacts alongside the markdown state files: `cornell-notes`, `franklin-scheduler`, `learning-planner`, `smart-goal-setter`, `weekly-review`. The user gets a real document to view, save, or print — not just text in a chat window. Templates live in each skill's `references/` directory; CSS is inline so files work offline.

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

## Install — works on every agent that adopts the Agent Skills open standard

Self-Learning OS follows the [Agent Skills open standard](https://agentskills.io) (originally from Anthropic, now used by 26+ AI agents). The same 12 SKILL.md files work as-is across **Claude Code, claude.ai (web/desktop), OpenAI Codex CLI, Gemini CLI, Cursor, GitHub Copilot, VS Code**, and more. No per-client modifications needed.

Pick your client below. Most installs are one command.

---

### Claude Code (CLI)

**Plugin install (recommended — gets all 12 skills atomically):**

```bash
git clone https://github.com/Mohit-5899/self-learning-os ~/.claude/plugins/self-learning-os
```

**Or skills-only install (cherry-pick what you want):**

```bash
git clone https://github.com/Mohit-5899/self-learning-os /tmp/slos
cp -r /tmp/slos/skills/* ~/.claude/skills/      # user scope (all projects)
# OR
cp -r /tmp/slos/skills/* .claude/skills/        # project scope (this repo only)
```

Open a fresh Claude Code session and say `I want to learn <topic>`. The `self-learning-coach` skill auto-routes you through Diagnostic → Planning → Execution.

📖 Reference: [code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills)

---

### claude.ai (Web / Desktop app)

Available on **Free, Pro, Max, Team, and Enterprise plans** (requires the code-execution capability enabled).

1. Clone or download this repo, then zip the `skills/` folder
2. In claude.ai, go to **Settings → Capabilities → Skills → Upload**
3. Upload the ZIP — **important: the ZIP must contain the `skills/` folder at root**, not just the individual `SKILL.md` files

If you only want a specific skill (e.g., just the coordinator), zip only that one skill's directory.

📖 Reference: [support.claude.com — How to create custom Skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)

---

### OpenAI Codex CLI

Skills supported since December 2025.

```bash
# Default Codex skills path
git clone https://github.com/Mohit-5899/self-learning-os ~/.agents/skills/self-learning-os

# Or the alternative path some Codex versions use
git clone https://github.com/Mohit-5899/self-learning-os ~/.codex/skills/self-learning-os
```

Inside Codex CLI: run `/skills` to confirm the bundle is loaded. Codex auto-loads matching skills; you can also explicitly mention a skill with `$<skill-name>`.

📖 Reference: [developers.openai.com/codex/skills](https://developers.openai.com/codex/skills)

---

### Gemini CLI

```bash
# Global (all projects)
git clone https://github.com/Mohit-5899/self-learning-os ~/.gemini/skills/self-learning-os

# Or project-scoped (single repo, shareable via git)
cd your-project
git clone https://github.com/Mohit-5899/self-learning-os .gemini/skills/self-learning-os
```

Start a new Gemini CLI session. Skills activate via description matching.

📖 Reference: [geminicli.com/docs/cli/skills](https://geminicli.com/docs/cli/skills/)

---

### Cursor, GitHub Copilot, VS Code, and 20+ other agents

All consume the same [Agent Skills standard](https://agentskills.io). Follow your client's docs for its local skills directory path, then `git clone` this repo into it. The same 12 SKILL.md files work as-is.

---

### Universal: install once, use everywhere

If you use multiple agents, clone once and symlink the rest. Then a single `git pull` updates every agent at once:

```bash
# Clone once to a neutral location
git clone https://github.com/Mohit-5899/self-learning-os ~/agent-skills/self-learning-os

# Symlink into each client's skills directory
ln -s ~/agent-skills/self-learning-os ~/.claude/plugins/self-learning-os
ln -s ~/agent-skills/self-learning-os ~/.agents/skills/self-learning-os
ln -s ~/agent-skills/self-learning-os ~/.gemini/skills/self-learning-os
```

---

### Verify it's working

In your client, ask:

> *"I want to learn Spanish in 3 months, I have 6 hours per week."*

The `self-learning-coach` skill should immediately route you into Phase A (`topic-classifier` first). If it instead gives you a generic study plan, the skill didn't trigger — most often this is because:

- The skill directory wasn't placed in the right path for your client (check the paths above)
- Your client session needs a restart after install
- The description-trigger didn't match — open `skills/self-learning-coach/SKILL.md` and verify the `description:` field

Your learning state (per-topic) lives at `~/.self-learning-os/<topic>/` — portable across all agents.

## Worked example

See [`examples/computer-vision/`](./examples/computer-vision/) for a real end-to-end run of the bundle — a working professional with a 9-hour day job, side business, and family commitments learning modern computer vision (CNNs → Vision Transformers → object detection) over 12 weeks. Open the `.html` files in a browser to see the rendered artifacts (SMART goal card, plan dashboard, weekly calendar).

## Quick start

After install, just say what you want to learn:

```
I want to learn computer vision. I work 10–7, have family time 7–8:30, 
and run a side business. Where do we start?
```

The coach will route you into `topic-classifier` first, then walk you through the rest in order. Your state lives in `~/.self-learning-os/<topic>/`.

## Status

**v0.1 — In development.** Building in public. PRs, issues, and feedback welcome.

- [x] **v0.1 (2026-05-23):** Phase A + B end-to-end + `feynman-checker` — scaffold written, untested
- [x] **v0.2 (2026-05-23):** `sq3r-session`, `cornell-notes`, `weekly-review`, `confusion-endurance` — scaffold written, untested
- [ ] **v1.0:** End-to-end acceptance test + description-hardening eval loop (port `skill-creator/run_loop.py`)
- [ ] **v1.1:** Real-user validation pass + iteration on description triggers
- [ ] **v1.2:** Optional MCP server for Anki/Obsidian export + `py-fsrs` integration
- [ ] **v2:** Subagent for parallel topic management

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
