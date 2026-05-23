# SMART Goal: computer-vision

## Goal statement

> By **2026-08-22** (12 weeks from 2026-05-23), I will have:
> (1) completed Vizuara CV series videos 1–30 (CNN era + Vision Transformer foundations + hands-on bootcamp through Roboflow deployment),
> (2) implemented from scratch and trained one custom image classifier on a non-MNIST dataset in PyTorch,
> (3) implemented one YOLO-based object detector on a self-chosen dataset,
> (4) shipped one CV demo deployed for the Quanta AI Labs portfolio + one Quanta blog post documenting the journey
> — investing **18 hours/week** because this directly extends my day job AI engineering work, creates a Quanta AI Labs service offering, and seeds a QuantaSkool module.

## SMART breakdown
- **Specific:** Vizuara videos 1–30 + custom classifier + YOLO detector + deployed demo + blog post — five concrete deliverables.
- **Measurable:** Five binary outcomes. Each either exists on 2026-08-22 or it doesn't. No ambiguity.
- **Achievable:** 18h/week × 12 weeks = **216h budget**. Video time for 1–30 ≈ 30h. Implementation + Cornell + Feynman ≈ 4× video time = 120h. Project work ≈ 50h. Review + buffer ≈ 16h. Math works.
- **Relevant:** Three independent payoffs — day job role uplift, Quanta offering, QuantaSkool curriculum. The skill compounds across all three.
- **Time-bound:** 2026-08-22 hard deadline. 12 weeks. Weekly checkpoint via `weekly-review` skill.

## Milestones
- **Week 4 (by 2026-06-20):** Vizuara videos 1–9 complete (math + image processing + simple NN + classifier + hyperparam tuning + regularization + transfer learning + AlexNet). **Verification:** custom classifier on non-MNIST dataset trains to reasonable accuracy in PyTorch.
- **Week 8 (by 2026-07-18):** Vizuara videos 10–22 complete (VGG → ResNet → MobileNet → ViT intro + coding + hands-on bootcamp day 1 + YOLO + R-CNN family). **Verification:** YOLO detector working on a self-chosen dataset; Feynman-explain ViT mechanism in plain language.
- **Week 12 (by 2026-08-22):** Vizuara videos 23–30 complete (Mask R-CNN + UNet + UNet family + YOLO LPR project + Roboflow + fall detection deployment). **Verification:** CV demo live at a URL + Quanta blog post published.

## Pivot rule
If by **Week 4** I have not completed Vizuara videos 1–9 AND have not trained a working classifier on a non-MNIST dataset, **scope down**: drop the YOLO + Roboflow deployment, keep only Vizuara videos 1–19 (CNN era + ViT) + a single ViT implementation from scratch + the Quanta blog post. This preserves the highest-utility learning even if life intervenes.

If a milestone is missed because the topic isn't what was expected: re-run `topic-classifier` (rare for CV).

---

```json
{
  "specific": "Vizuara videos 1-30 + custom classifier + YOLO detector + deployed demo + blog post",
  "measurable": "Five concrete binary deliverables on 2026-08-22",
  "achievable": "216h budget vs estimated 216h needed — exact fit; assumes no major slip",
  "relevant": "day job role uplift + Quanta AI Labs offering + QuantaSkool curriculum",
  "time_bound": "2026-08-22 deadline, 12 weeks, weekly-review checkpoints",
  "deadline_date": "2026-08-22",
  "weekly_hours_committed": 18,
  "total_hour_budget": 216,
  "milestones": [
    {"week": 4, "deadline": "2026-06-20", "milestone": "Vizuara 1-9 + custom non-MNIST classifier trained", "verification": "Classifier achieves reasonable accuracy in PyTorch"},
    {"week": 8, "deadline": "2026-07-18", "milestone": "Vizuara 10-22 + YOLO detector + ViT understood", "verification": "YOLO on chosen dataset + Feynman-explain ViT"},
    {"week": 12, "deadline": "2026-08-22", "milestone": "Vizuara 23-30 + deployed CV demo + Quanta blog post", "verification": "Demo live at URL + post published"}
  ],
  "pivot_rule": "If week 4 milestone missed: scope down to Vizuara 1-19 + ViT impl + blog post only; drop YOLO deployment"
}
```
