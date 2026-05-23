# Topic Classification: computer-vision

## Summary
- **Primary type:** science
- **Self-learning suitability:** high
- **Scope confirmation:** Modern deep-learning-based computer vision — CNNs → Vision Transformers → VLMs → Diffusion → JEPA. Applied focus (build + deploy), not pure research. Primary resource: Vizuara YouTube playlist (71 videos).

## Rationale
The Vizuara playlist is heavily code-grounded — every architecture (AlexNet, ResNet, ViT, DETR, SAM, DDPM) is implemented from scratch in PyTorch. Performance is verifiable against objective baselines (ImageNet accuracy, IoU, mIoU, FID). Topic is directly adjacent to the user's existing AI engineering work at day job.

## Adjustments for downstream skills
- **learning-planner:** dominant resource = Vizuara playlist (sequenced). Supplement only with PyTorch official docs, Hugging Face vision models, arXiv for dissected papers.
- **franklin-scheduler:** practice-testing = implement each architecture in PyTorch end-to-end. Production blocks = working code + dataset.
- **sq3r-session:** Recite = re-implement from notes without looking.
- **cornell-notes:** cues are mechanism questions. Recall questions testable against code execution.
- **feynman-checker:** explain mechanism + predict failure modes + give analogies from existing multi-agent / RAG work.

## Critique/community requirements
None mandatory. Optional: post implementations to GitHub for self-imposed feedback loop. Quanta AI Labs blog/QuantaSkool serves as accountability channel.

---

```json
{
  "topic": "computer-vision",
  "primary_type": "science",
  "self_learning_suitability": "high",
  "scope": "Modern deep-learning CV: CNNs through ViT, VLMs, Diffusion, JEPA — applied focus",
  "adjustments": {
    "learning_planner": "Vizuara playlist sequenced; supplements: PyTorch docs, HF models, arXiv",
    "franklin_scheduler": "practice-testing = PyTorch implementation",
    "sq3r_session": "Recite = re-implement from notes",
    "cornell_notes": "cues = mechanism questions; recall = testable against code",
    "feynman_checker": "explain mechanism + failure modes + analogies from multi-agent work"
  },
  "external_feedback_required": false,
  "external_feedback_plan": "Optional GitHub posting + Quanta blog accountability"
}
```
