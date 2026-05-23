# Learning Plan: computer-vision

## Planks (rotating focus areas)

1. **Math + Image Processing foundation** (Mon)
   - Topics: linear algebra refresher, convolution math, filters, edge detection, image pipelines
   - Resources: Vizuara videos 2-3 (already partially done — video 2 complete, video 3 = filters & convolution)
   - Default techniques: practice testing (code implementations of filters), self-explanation

2. **CNN Era — Simple NN to AlexNet through Historical CNNs** (Tue)
   - Topics: simple NN for vision, image classification, hyperparameter tuning (W&B), regularization, transfer learning, AlexNet, VGG, GoogleNet, SqueezeNet, ResNet, MobileNet, DenseNet, EfficientNet, NASNet
   - Resources: Vizuara videos 4-16
   - Default techniques: practice testing (PyTorch impl), elaborative interrogation, distributed practice

3. **Vision Transformers** (Wed)
   - Topics: ViT intuition, ViT from scratch coding, attention mechanism for vision
   - Resources: Vizuara videos 17-19
   - Default techniques: self-explanation (highest priority), practice testing, Feynman weekly

4. **Hands-on: Object Detection + Segmentation** (Thu)
   - Topics: hands-on bootcamp day 1, OpenCV + YOLO object tracking, R-CNN/Fast/Faster R-CNN, Mask R-CNN, UNet + UNet family
   - Resources: Vizuara videos 20-25
   - Default techniques: practice testing (code along), elaborative interrogation

5. **Project + Deployment** (Fri)
   - Topics: YOLO beginner, LPR project, Roboflow pipeline, Streamlit deployment
   - Resources: Vizuara videos 26-30 + own dataset + Roboflow + Streamlit
   - Default techniques: production-grade implementation, Quanta blog drafting

6. **Review + Integration** (Sat weekend session + Fri evening Feynman)
   - Topics: cross-plank synthesis, weekly Feynman check, project advancement
   - Resources: own notes + recall_questions from Cornell + Feynman skill
   - Default techniques: spaced repetition (1d/3d/7d/21d), self-explanation, weekly-review

## Week 1 — Survey phase (orientation only, no deep engagement yet)

- **Vizuara channel + this CV playlist** — verify channel is current and active. Re-watch videos 1-2 at 1.5× for refresh. *(verified 2026-05-23 via user-provided list)*
- **Video 3 — Filters and Convolution** — survey only: read the description, skim the first 5 min and last 5 min. Do not take notes yet. Identify what questions you'll hunt for in Week 2.
- **PyTorch official tutorials index** — pytorch.org/tutorials — browse the top-level structure. Identify which tutorials map to which plank.
- **Hugging Face vision models hub** — huggingface.co/models?pipeline_tag=image-classification — browse top models. Note which architectures appear repeatedly.
- **arXiv survey paper search** — search "computer vision survey 2025" — pick one survey (e.g., a recent foundation-models-for-vision review) and read only abstract + section headings.

> **Web verification note (per ADR-009):** In production, learning-planner uses WebSearch/WebFetch to confirm every URL is alive and verify Vizuara playlist currency. For this acceptance test, resources are drawn from the user-provided playlist + training data; user should spot-check URLs before committing time.

## Resource library (by plank)

### Plank 1 — Math + Image Processing
- Vizuara videos 2-3 (primary)
- 3Blue1Brown "Essence of Linear Algebra" (supplement, if rusty)
- PyTorch tutorial: "What is PyTorch?" + Tensor basics

### Plank 2 — CNN Era
- Vizuara videos 4-16 (primary, sequenced)
- PyTorch torchvision.models documentation
- Hugging Face transformers vision model cards (ResNet, ViT cards)

### Plank 3 — Vision Transformers
- Vizuara videos 17-19 (primary)
- "An Image is Worth 16×16 Words" — Dosovitskiy et al. 2020 — arXiv:2010.11929 (read alongside Vizuara video 17)

### Plank 4 — Detection + Segmentation
- Vizuara videos 20-25 (primary)
- Ultralytics YOLO documentation
- Detectron2 model zoo (reference)

### Plank 5 — Project + Deployment
- Vizuara videos 26-30 (primary)
- Roboflow documentation
- Streamlit documentation

### Plank 6 — Review + Integration
- Own Cornell notes from `~/.self-learning-os/computer-vision/notes/`
- Own SQ3R reviews from `~/.self-learning-os/computer-vision/reviews/`

## Question bank (hunt for answers across the plan)

1. Why does a convolution work better than a fully connected layer for images? (mechanism)
2. How do skip connections solve the vanishing gradient problem? (mechanism)
3. Why does ResNet enable training networks 100+ layers deep when AlexNet struggled at 8? (comparative)
4. How does YOLO achieve real-time inference while two-stage detectors (Faster R-CNN) don't? (mechanism)
5. What is an anchor box and why are multiple scales needed? (mechanism)
6. Why does batch normalization improve training stability? (mechanism)
7. What changes when ViT replaces convolutions with self-attention — what is gained, what is lost? (comparative)
8. How does transfer learning work and when does it fail? (boundary)
9. What does data augmentation actually do statistically? (mechanism)
10. Why is IoU the standard detection metric and what are its failure modes? (application)
11. How does UNet's encoder-decoder structure produce per-pixel segmentation? (mechanism)
12. What is the relationship between an attention map in ViT and a feature map in a CNN — are they the same kind of thing? (synthesis)

## Motivation anchors (re-read when motivation dips)

- **Autonomy:** "I am learning CV on my own schedule, on my own terms — because I chose to extend my AI engineering work. No university or employer told me to do this. The decision is mine."
- **Mastery:** "Every day job project I touch after week 4 will benefit from sharper vision understanding. By week 12, I will be able to do things on my team that nobody else can do — and I will know exactly how I got there."
- **Purpose:** "This 3-month journey produces a Quanta AI Labs CV service offering, a QuantaSkool module, and a public blog post. The learning is not just for me — it seeds three downstream wins for Quanta."

## External feedback plan

None mandatory (science topic, high suitability). Self-imposed accountability via:
- Posting each plank's implementation to a public GitHub repo (one repo per plank, or one repo with branches)
- Weekly Quanta AI Labs internal update — sharing the Week N artifact with collaborators
- Final Quanta blog post (week 12 milestone)

---

```json
{
  "planks": [
    {"name": "Math + Image Processing", "topics": ["linear algebra", "convolution", "filters", "edge detection"], "default_techniques": ["practice_testing", "self_explanation"], "weight": 0.15},
    {"name": "CNN Era", "topics": ["simple NN", "AlexNet", "VGG", "GoogleNet", "ResNet", "MobileNet", "DenseNet", "EfficientNet", "NASNet"], "default_techniques": ["practice_testing", "elaborative_interrogation", "distributed_practice"], "weight": 0.25},
    {"name": "Vision Transformers", "topics": ["ViT intuition", "ViT from scratch", "attention for vision"], "default_techniques": ["self_explanation", "practice_testing"], "weight": 0.15},
    {"name": "Detection + Segmentation", "topics": ["YOLO", "R-CNN family", "Mask R-CNN", "UNet"], "default_techniques": ["practice_testing", "elaborative_interrogation"], "weight": 0.20},
    {"name": "Project + Deployment", "topics": ["LPR project", "Roboflow", "Streamlit", "fall detection demo"], "default_techniques": ["production_implementation"], "weight": 0.20},
    {"name": "Review + Integration", "topics": ["cross-plank synthesis", "spaced repetition", "weekly Feynman"], "default_techniques": ["distributed_practice", "self_explanation"], "weight": 0.05}
  ],
  "week_1_survey": [
    {"resource": "Vizuara channel + playlist", "task": "verify channel current; re-watch videos 1-2 at 1.5x", "verified_date": "2026-05-23"},
    {"resource": "Vizuara video 3 (Filters & Convolution)", "task": "survey only — read description, skim first/last 5 min", "verified_date": "2026-05-23"},
    {"resource": "pytorch.org/tutorials", "task": "browse top-level structure; map to planks", "verified_date": "2026-05-23"},
    {"resource": "huggingface.co/models (image-classification)", "task": "browse top models; note repeated architectures", "verified_date": "2026-05-23"},
    {"resource": "arXiv CV survey 2025", "task": "abstract + section headings only", "verified_date": "2026-05-23"}
  ],
  "question_bank_count": 12,
  "motivation_anchors": {
    "autonomy": "Learning CV on my own terms — extending AI engineering by choice",
    "mastery": "By week 12, doing things at day job that nobody else on the team can do",
    "purpose": "Produces Quanta CV offering + QuantaSkool module + public blog post"
  },
  "external_feedback_plan": "GitHub repo + weekly Quanta update + final Quanta blog post (week 12 milestone)",
  "v1_scope": "Vizuara videos 1-30",
  "deferred_to_v2": "Vizuara videos 31-71 (Transformers for Vision LLMs, VLMs, Diffusion, JEPA)"
}
```
