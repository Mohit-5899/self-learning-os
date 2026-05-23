# Confidence Audit: computer-vision

## Existing knowledge that connects
- Python (daily use, AI engineering)
- Deep learning fundamentals (CNNs conceptually familiar from AI work)
- PyTorch (intermediate — used in production AI work)
- LangGraph / multi-agent systems (parallel architecture pattern to transformers)
- RAG pipelines (embedding spaces overlap with vision embeddings)
- Linear algebra (rusty but recoverable)
- 2 Vizuara videos already completed:
  1. Computer Vision from Scratch — course launch (28 min)
  2. Lecture 1: Introduction to Computer Vision (52 min)

## Past learning wins
- **AI engineering / multi-agent systems** — taught self the LangGraph + multi-agent stack at day job; result = shipping production RAG and agent systems. Same self-learning process applies here.
- **Self-Learning OS plugin scaffold** — built a 19-file open-source plugin over one weekend (12 skills + ADRs + research-backed evidence file) by combining a book + research papers + GitHub patterns. Proof that disciplined self-learning produces real artifacts.

## Anticipated fears (named, addressed)
- **Time scarcity given day job + Quanta + family** → addressed by self-management Block 2 (18h/week budget locked)
- **Getting lost in 71 videos** → addressed by plank-based rotation; v1 focuses on videos 1-30 only
- **Math gaps (linear algebra, calculus)** → addressed by math foundation in week-1 survey + just-in-time review
- **Falling behind weekly schedule** → addressed by fallback minimums in scheduler + confusion-endurance skill on standby

## Confidence anchors (re-read when motivation dips)
1. "You learned LangGraph + multi-agent orchestration from scratch in your AI engineering work. Vision is the same playbook with different architectures — you already know how to learn this kind of material."
2. "If you put 6 hours/week in for 4 weeks and feel zero progress, re-evaluate scope. Until then, the only valid comparison is your own week-over-week."
3. "The Vizuara series is sequenced — Sanjay/Raj have already done the curriculum work. You don't have to invent the syllabus; you have to execute it."
4. "Even on a wrecked day, the fallback minimum (20 min, one Feynman attempt) keeps the streak alive. Streaks compound; perfection doesn't."

## Progress markers (3-month vision)
- **By week 4:** trained one image classifier end-to-end on a non-MNIST dataset (real images, multiple classes) in PyTorch
- **By week 8:** implemented YOLO-style object detection on a self-chosen dataset AND understand the ViT mechanism well enough to Feynman-explain it
- **By week 12:** one shipped CV demo — deployed, has a UI, can be shown to a Quanta client OR posted as a QuantaSkool case study + one Quanta blog post

---

```json
{
  "existing_knowledge": ["Python", "DL fundamentals", "PyTorch (intermediate)", "LangGraph", "RAG", "Linear algebra (rusty)"],
  "videos_pre_watched": [
    {"order": 1, "title": "Computer Vision from Scratch course launch", "duration_min": 28},
    {"order": 2, "title": "Lecture 1: Introduction to CV", "duration_min": 52}
  ],
  "past_wins": [
    {"learned": "AI engineering / multi-agent systems", "method": "self-taught while working at day job"},
    {"learned": "Self-Learning OS plugin scaffold (12 skills)", "duration_months": 0.25, "method": "book + research papers + GitHub patterns"}
  ],
  "anticipated_fears": [
    {"fear": "Time scarcity", "addressed_by": "self-management 18h/week budget"},
    {"fear": "Getting lost in 71 videos", "addressed_by": "plank-rotation + v1 scope = videos 1-30 only"},
    {"fear": "Math gaps", "addressed_by": "week-1 math foundation + JIT review"},
    {"fear": "Falling behind", "addressed_by": "fallback minimums + confusion-endurance"}
  ],
  "confidence_anchors": ["LangGraph parallel", "4-week kill-switch", "Vizuara is sequenced", "streaks compound"],
  "progress_markers_3_month": [
    "Week 4: non-MNIST classifier trained",
    "Week 8: YOLO detector + ViT mechanism mastered",
    "Week 12: CV demo deployed + blog post"
  ]
}
```
