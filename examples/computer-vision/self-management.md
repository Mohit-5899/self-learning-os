# Self-Management Setup: computer-vision

## Weekly time audit
| Time | Mon-Fri | Sat | Sun |
|---|---|---|---|
| 06:30–08:30 | ✅ FREE — primary block (deep) | ✅ Weekend long session | ✅ FREE (buffer) |
| 08:30–10:00 | Get ready / commute | (free) | (free) |
| 10:00–13:00 | ❌ day job | (free) | (free) |
| 13:00–13:30 | 💡 Lunch — micro reinforcement (10 min flashcards or 1-line Feynman) | | |
| 13:30–19:00 | ❌ day job | (free) | (free) |
| 19:00–20:30 | 🚫 NON-NEGOTIABLE — family time | 🚫 family | 🚫 family |
| 20:30–21:00 | 🟡 Quanta AI Labs (urgent only) | (Quanta) | (Quanta) |
| 21:00–22:15 | ✅ Secondary block — light reinforcement | (review) | (review) |
| 22:30+ | 🚫 Sleep | | |

## Learning blocks
- **Primary block:** 06:30–08:30, Mon–Fri, deep intensity. Structure: 50min focused study + 10min break + 50min practice (PyTorch implementation) + 10min Cornell notes.
- **Secondary block:** 21:00–22:15, Mon–Fri, light intensity. Structure: 15min spaced-repetition review + 30min next-day prep + 30min light watching/reading.
- **Weekend session:** Sat 09:00–11:00, 2 hours, cross-plank integration + project building.
- **Sunday:** unstructured buffer — catch up missed blocks OR weekly-review skill OR rest.

## Non-negotiables (scheduler must NEVER overlap)
- 19:00–20:30 family time — every day
- 22:30–06:00 sleep — every day
- 10:00–19:00 day job — Mon–Fri

## Ego-depletion risks
- After heavy day job meeting days, evening secondary block drops to ultra-light (5-min spaced review only)
- Weekend session may be sacrificed if Quanta AI Labs has urgent client work — fallback is 30-min Feynman check Sunday morning

## Routines
- **Start cue:** [TBD by user — proposed: tea + open the Vizuara playlist tab + open PyTorch in VS Code]
- **End cue:** [TBD by user — proposed: write one-line summary in `~/.self-learning-os/computer-vision/log.md` + close laptop]
- **Fallback minimum:** "20 minutes — one Feynman attempt on the most recent concept OR re-watch one Vizuara video at 1.5× without notes"

## Weekly learning hours available
- Primary (Mon-Fri): 5 × 2.0h = **10.0h** (deep)
- Secondary (Mon-Fri): 5 × 1.25h = **6.25h** (light)
- Weekend (Sat): **2.0h**
- Lunch micro-reinforcement (Mon-Fri): 5 × 10min ≈ **0.8h** (optional)
- **Total: ~18h/week**

This is a comfortable budget. The Vizuara playlist totals ~250h of video (not counting implementation). At 18h/week, full playlist = ~14 weeks of video alone, 5-7 months including implementation + review + Feynman cadence.

**Decision locked: v1 scope = Vizuara videos 1-30 (CNN era + Vision Transformer + hands-on bootcamp + Roboflow deployment). Videos 31-71 deferred to v2.**

---

```json
{
  "time_audit_summary": "9-hour day job + 1.5h family + 30min Quanta + 1.25h evening study + 2h Saturday",
  "primary_block": {"time": "06:30-08:30", "intensity": "deep", "days": ["Mon","Tue","Wed","Thu","Fri"], "duration_min": 120},
  "secondary_block": {"time": "21:00-22:15", "intensity": "light", "days": ["Mon","Tue","Wed","Thu","Fri"], "duration_min": 75},
  "weekend_session": {"day": "Sat", "time": "09:00-11:00", "duration_min": 120},
  "non_negotiables": [
    {"label": "family time", "time": "19:00-20:30", "days": "every day"},
    {"label": "day job", "time": "10:00-19:00", "days": ["Mon","Tue","Wed","Thu","Fri"]},
    {"label": "sleep", "time": "22:30-06:00", "days": "every day"}
  ],
  "ego_depletion_risks": ["heavy meeting days reduce evening to ultra-light only"],
  "routines": {
    "start_cue": "tea + Vizuara playlist tab + PyTorch in VS Code",
    "end_cue": "one-line summary in log.md + close laptop",
    "fallback_minimum": "20 min — one Feynman attempt OR one video at 1.5x"
  },
  "weekly_learning_hours": 18,
  "v1_scope_decision": "Vizuara videos 1-30 (CNN era + ViT + hands-on bootcamp + deployment)"
}
```
