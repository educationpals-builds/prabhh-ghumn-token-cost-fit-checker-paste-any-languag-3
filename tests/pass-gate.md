# Pass Gate

The acceptance gate this checker must hold before shipping.

---

## Metric

**Learner-defined gate:**

> I dont have it. I dont have it. I dont have it

---

## Threshold

*Not specified by builder — see learner note above.*

---

## Re-run Cadence

*Not specified by builder — see learner note above.*

---

## Contested-Call Rulings

When the checker and a human reviewer disagree on a dial score, the ruling is logged here with Atlas's opposing case preserved.

| Probe | Dial | Checker Score | Human Score | Ruling | Atlas's Opposing Case |
|-------|------|---------------|-------------|--------|----------------------|
| *(no contested calls recorded — pass_gate not defined)* | — | — | — | — | — |

---

## Gate Status

**INCOMPLETE** — The builder did not provide a metric, threshold, or re-run trigger. Before this checker can be certified, the pass gate must specify:

1. A concrete metric (e.g., "vocabulary_fit dial accuracy across probe board")
2. A numeric threshold (e.g., "≥ 7 of 8 probes return expected dial behavior")
3. A re-run trigger (e.g., "weekly, or on any prompt/stance change")

---

## Notes

This gate file will be updated when the builder supplies the required pass_gate definition. Until then, the checker operates without a formal acceptance threshold.
