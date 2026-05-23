# Weekly Schedule: computer-vision

## Plank rotation

| Day | Primary plank (06:30–08:30) | Secondary focus (21:00–22:15) |
|---|---|---|
| Mon | Math + Image Processing | Spaced review + plan tomorrow |
| Tue | CNN Era | Spaced review + plan tomorrow |
| Wed | Vision Transformers | Spaced review + plan tomorrow |
| Thu | Detection + Segmentation | Spaced review + plan tomorrow |
| Fri | Project + Deployment | **Weekly Feynman check** (21:00–21:30) + plan weekend |
| Sat | Weekend session 09:00–11:00 — cross-plank integration + project | (free) |
| Sun | Buffer / catch-up / `weekly-review` skill | (free) |

## Daily block structure

### Primary block — 06:30–08:30 (Mon–Fri, deep work)
- **06:30–07:20** (50 min): Active study on day's plank — watch Vizuara video OR re-watch + take Cornell notes
- **07:20–07:30** (10 min): Break — per Ch.3 "50 min focus, then break"
- **07:30–08:20** (50 min): Practice testing — implement the concept in PyTorch, run, debug
- **08:20–08:30** (10 min): Cornell summary + recall questions for the day's concept

### Secondary block — 21:00–22:15 (Mon–Fri, light)
- **21:00–21:15** (15 min): Spaced-repetition review of due flashcards (+1d/+3d/+7d/+21d queue)
- **21:15–21:45** (30 min): Watch next day's Vizuara video preview OR light reading
- **21:45–22:15** (30 min): Plan tomorrow — open the exact resource, write a one-sentence goal

### Weekend session — Sat 09:00–11:00 (cross-plank, project-oriented)
- **09:00–10:00** (60 min): Cross-plank integration — connect concepts learned in different planks during the week
- **10:00–11:00** (60 min): Project advancement — work on the SMART-goal deliverable (classifier → YOLO → demo)

## Spaced-repetition review queue (default cadence per Cepeda 2006)

For any new concept introduced in a primary block:
- **+1 day:** quick recall check during next-day secondary block
- **+3 days:** deeper retrieval — can you apply it?
- **+7 days:** Feynman-style explain-back
- **+21 days:** full integration check (cross-plank if relevant)

**Sample queue for Week 2 (assuming Week 1 plank-rotation has started):**
- Mon Week 2: review Mon Week 1 (Math+IP) at +7d, review Thu Week 1 (Detection) at +3d
- Tue Week 2: review Tue Week 1 (CNN) at +7d
- ...

The coordinator auto-generates the actual due queue from `notes/` and `reviews/` directories.

## Weekly Feynman check (highest-utility, g≈0.55 — DO NOT SKIP)

- **Friday 21:00–21:30** (30 min) — pick one concept from the week and run `feynman-checker` skill on it
- Replaces the standard Friday secondary-block start
- If skipped, surface as the top priority in `weekly-review` Sunday

## Fallback minimums (when life intervenes)

- **Primary block fallback** (≤20 min): one Feynman attempt on the most recent concept OR re-watch one Vizuara video at 1.5× without notes
- **Secondary block fallback** (≤5 min): review the top 3 due flashcards. Done.

## Ego-depletion adjustments

- **Heavy day job meeting days (Wed?):** secondary block drops to ultra-light (5-min flashcards only), defer planning to Thu morning
- **Quanta urgent client work** (any evening): skip secondary block; pick up the spaced review queue Saturday morning

## Non-negotiables (scheduler must NEVER overlap)

- 19:00–20:30 family time — every day
- 22:30–06:00 sleep — every day
- 10:00–19:00 day job — Mon–Fri

---

```json
{
  "weekly_grid": [
    {"day": "Mon", "primary_plank": "Math + Image Processing", "primary_time": "06:30-08:30", "secondary_time": "21:00-22:15"},
    {"day": "Tue", "primary_plank": "CNN Era", "primary_time": "06:30-08:30", "secondary_time": "21:00-22:15"},
    {"day": "Wed", "primary_plank": "Vision Transformers", "primary_time": "06:30-08:30", "secondary_time": "21:00-22:15"},
    {"day": "Thu", "primary_plank": "Detection + Segmentation", "primary_time": "06:30-08:30", "secondary_time": "21:00-22:15"},
    {"day": "Fri", "primary_plank": "Project + Deployment", "primary_time": "06:30-08:30", "secondary_time": "21:00-22:15 (Feynman 21:00-21:30)"},
    {"day": "Sat", "primary_plank": "Cross-plank integration + project", "primary_time": "09:00-11:00", "secondary_time": null},
    {"day": "Sun", "primary_plank": "Buffer / weekly-review", "primary_time": null, "secondary_time": null}
  ],
  "review_cadence": {
    "intervals_days": [1, 3, 7, 21],
    "weekly_feynman": {"day": "Fri", "time": "21:00-21:30", "duration_min": 30}
  },
  "weekend_session": {"day": "Sat", "time": "09:00-11:00", "purpose": "cross-plank integration + project advancement"},
  "fallback_minimums": {
    "primary_block": "20 min — one Feynman attempt OR one Vizuara video at 1.5x",
    "secondary_block": "5 min — top 3 due flashcards"
  },
  "non_negotiables_respected": ["family 19:00-20:30", "day job 10:00-19:00", "sleep 22:30-06:00"],
  "ego_depletion_adjustments": {"heavy_meeting_days": "ultra-light only", "quanta_urgent": "skip secondary; defer to Sat"}
}
```
