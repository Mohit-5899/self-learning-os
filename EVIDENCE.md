# Evidence Base

> This document records the cognitive-science and AI-systems research that backs Self-Learning OS's design. Every default in the bundle that overrides the book traces back to a citation here.

## Why this document exists

*The Science of Self-Learning* by Peter Hollins is an excellent overview but is not itself peer-reviewed. Where the book and the published evidence diverge, we side with the evidence — and we record both the original book claim and the empirical correction so anyone can audit the deviation.

This file is the audit trail.

---

## Part 1 — Cognitive Science of Learning

### Dunlosky et al. (2013) — the technique-utility hierarchy

**Citation.** Dunlosky, J., Rawson, K. A., Marsh, E. J., Nathan, M. J., & Willingham, D. T. (2013). *Improving students' learning with effective learning techniques: Promising directions from cognitive and educational psychology.* **Psychological Science in the Public Interest**, 14(1), 4–58.

**What it is.** The canonical meta-analytic appraisal of ten common study techniques, rating each by "utility" — meaning combined effect size, breadth of conditions where it works, and ease of adoption.

**Verdict (summary table).**

| Technique | Utility rating |
|---|---|
| **Practice testing** (retrieval practice, self-quizzing) | **HIGH** |
| **Distributed practice** (spaced repetition) | **HIGH** |
| Elaborative interrogation | MEDIUM |
| Self-explanation | MEDIUM |
| Interleaved practice | MEDIUM |
| Summarization | LOW |
| Highlighting / underlining | LOW |
| Keyword mnemonic | LOW |
| Imagery for text | LOW |
| Rereading | LOW |

**How it shapes Self-Learning OS.**
- `feynman-checker` (self-explanation) and `franklin-scheduler` (spaced repetition + practice testing) implement the HIGH-utility techniques as v1 defaults.
- The Cornell skill (v1.1) requires a `recall_questions` field — without retrieval, Cornell collapses to summarization (LOW utility).
- We do not promote highlighting or rereading anywhere in the bundle.

### Roediger & Karpicke (2006) — the testing effect

**Citation.** Roediger, H. L., & Karpicke, J. D. (2006). *Test-enhanced learning: Taking memory tests improves long-term retention.* **Psychological Science**, 17(3), 249–255.

**What it is.** Demonstrates that testing (retrieval practice) produces better long-term retention than equivalent time spent restudying — sometimes by 50% or more on delayed tests.

**How it shapes Self-Learning OS.**
- The `franklin-scheduler` builds practice-testing blocks into the weekly schedule, not just "study" blocks.
- The `feynman-checker` is itself a form of retrieval practice.
- The Cornell skill's `recall_questions` field exists for this reason.

### Cepeda et al. (2006) — distributed practice / the spacing effect

**Citation.** Cepeda, N. J., Pashler, H., Vul, E., Wixted, J. T., & Rohrer, D. (2006). *Distributed practice in verbal recall tasks: A review and quantitative synthesis.* **Psychological Bulletin**, 132(3), 354–380.

**What it is.** Meta-analysis of 184 studies on the spacing effect: review schedules spread across time produce better retention than massed practice, with d ≈ 0.40 on delayed tests. Optimal interspaced gap is roughly 10–20% of the desired retention interval.

**How it shapes Self-Learning OS.**
- `franklin-scheduler` defaults review cadence to **1 day / 3 days / 7 days / 21 days** for any new concept. This matches FSRS-class schedulers used by Anki and SuperMemo.
- Skills do not let the user pick arbitrary review intervals without surfacing the spacing-effect rationale.

### Chi et al. (1994) + Bisra et al. (2018) — self-explanation

**Citations.**
- Chi, M. T. H., De Leeuw, N., Chiu, M.-H., & LaVancher, C. (1994). *Eliciting self-explanations improves understanding.* **Cognitive Science**, 18(3), 439–477.
- Bisra, K., Liu, Q., Nesbit, J. C., Salimi, F., & Winne, P. H. (2018). *Inducing self-explanation: A meta-analysis.* **Educational Psychology Review**, 30(3), 703–725.

**What it is.** Chi's original studies showed that prompting learners to explain content to themselves while studying produces measurable gains on transfer tasks. Bisra's 2018 meta-analysis quantifies the effect at **g ≈ 0.55** — a medium-large effect.

**How it shapes Self-Learning OS.**
- `feynman-checker` is the single most-emphasized skill in the bundle. The Feynman Technique is operationalized self-explanation.
- Default cadence: weekly Feynman check per active topic.
- The book treats Feynman as one technique among many; we elevate it to the highest-priority Phase-C skill based on this evidence.

### Carlston (2011); Hartlep & Forsyth (2000) — SQ3R efficacy

**Citations.**
- Hartlep, K. L., & Forsyth, G. A. (2000). *The Effect of Note Taking on Memory of Lecture Material*. Teaching of Psychology.
- Carlston, D. L. (2011). *Benefits of student-generated note packets: A preliminary investigation*. **Teaching of Psychology**, 38(3), 142–146.

**What it is.** Evaluations of structured study methods including SQ3R. Key finding: the SQ3R method outperforms passive reading **because of** the Recite/Review steps (retrieval) — not because of the Survey step.

**How it shapes Self-Learning OS.**
- `sq3r-session` (v1.1) enforces the Recite and Review steps. Surveying alone is opt-in.
- The book's emphasis on Survey is preserved — but framed as "orientation," not "learning."

---

## Part 2 — LLM Agent Architecture

### Schick et al. (2023) — Toolformer

**Citation.** Schick, T., Dwivedi-Yu, J., Dessì, R., Raileanu, R., Lomeli, M., Zettlemoyer, L., Cancedda, N., & Scialom, T. (2023). *Toolformer: Language Models Can Teach Themselves to Use Tools.* **NeurIPS 2023**. arXiv:2302.04761.

**What it is.** Demonstrates an LM can decide when to invoke external tools by self-supervised learning over which tool-calls reduce completion loss.

**Relevance.** Validates the core Claude Skills design pattern: the model decides which skill to activate from description metadata. This is the architectural justification for treating skills as triggerable units rather than always-on system prompts.

### Yao et al. (2023) — ReAct

**Citation.** Yao, S., Zhao, J., Yu, D., Du, N., Shafran, I., Narasimhan, K., & Cao, Y. (2023). *ReAct: Synergizing Reasoning and Acting in Language Models.* **ICLR 2023**. arXiv:2210.03629.

**What it is.** Interleaves chain-of-thought reasoning with action tokens; outperforms reasoning-only and acting-only baselines on HotpotQA, Fever, ALFWorld, WebShop.

**Relevance.** The Self-Learning OS loop — coordinator reads state, decides next skill, invokes it, validates the artifact, updates state — is a direct ReAct pattern.

### Hong et al. (2024) — MetaGPT

**Citation.** Hong, S., Zhuge, M., Chen, J., Zheng, X., Cheng, Y., Wang, J., et al. (2024). *MetaGPT: Meta Programming for a Multi-Agent Collaborative Framework.* **ICLR 2024 (oral)**. arXiv:2308.00352.

**What it is.** Encodes Standard Operating Procedures into role-specialized agents communicating via structured artifacts (not free text). Reduces hallucination by enforcing handoff schemas.

**Relevance.** This paper is the architectural justification for ADR-005 (JSON artifacts). Each Self-Learning OS skill is a "role" with a defined input/output schema; the coordinator validates artifacts before chaining — the MetaGPT pattern.

### Wu et al. (2024) — AutoGen

**Citation.** Wu, Q., Bansal, G., Zhang, J., Wu, Y., Li, B., Zhang, E., Liu, J., Awadallah, A., White, R. W., Burger, D., & Wang, C. (2024). *AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation.* **COLM 2024**. arXiv:2308.08155.

**What it is.** Generalized framework for conversable LLM agents that compose via natural-language messages and tool calls.

**Relevance.** AutoGen's looser style is the alternative we **rejected** for inter-skill handoff (in favor of MetaGPT's stricter schemas). But it informs the coordinator's user-facing conversation style: lightweight, natural, with the underlying coordination still strict.

---

## Part 3 — LLM-Augmented Learning Systems

### Kestin et al. (2024) — AI tutoring outperforms active learning

**Citation.** Kestin, G., Miller, K., Klales, A., Milbourne, T., & Ponti, G. (2024). *AI tutoring outperforms active learning in MIT physics class.* (Working paper / L@S evidence.)

**What it is.** Head-to-head comparison: well-prompted LLM tutoring vs. expert-led active learning in an MIT physics course. LLM tutoring matched or exceeded human-led conditions on retention.

**Relevance.** This is the empirical case for investing in a real tutor-coach pattern, not just a static plan generator. The coordinator + feynman-checker pairing is intended to operate as an LLM tutor, not just a planner.

### Khanmigo / Khan Academy LLM tutors (2023–present)

**Citation.** Multiple working papers and Khan Academy / Khanmigo publications.

**Relevance.** Production evidence at scale that LLM tutors are usable in real learning contexts when paired with explicit pedagogy (not just chat). Our skills encode the pedagogy.

---

## Part 4 — Bloom's "2 sigma problem" (the motivating frame)

**Citation.** Bloom, B. S. (1984). *The 2 Sigma Problem: The Search for Methods of Group Instruction as Effective as One-to-One Tutoring.* **Educational Researcher**, 13(6), 4–16.

**What it is.** Bloom's original finding: one-to-one tutoring produces a 2 standard deviation improvement over conventional classroom instruction. The "search for methods of group instruction as effective as one-to-one tutoring" is unresolved.

**Relevance.** Self-Learning OS is a pragmatic answer: LLM tutoring + evidence-backed pedagogy + structured planning may be the modern path to closing Bloom's gap. The bundle is positioned within this lineage.

---

## How to update this file

When you add a new evidence claim to any SKILL.md:

1. Add the citation here under the relevant part.
2. State the finding in 1–2 sentences.
3. State explicitly how it shapes a skill's default behavior.
4. Link back from the SKILL.md to the relevant section here.

If a future paper overturns one of these findings, do not delete — add a new entry noting the update and reconsider the affected ADR.
