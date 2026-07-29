# Verification Protocol

This document explains how a stranger can verify that the Token Cost + Fit Checker works as calibrated.

## The Seeded Sample

Paste this exact text into the checker (via `/verify` or the main input):

```
"Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?"
```

This is the builder's pinned sample: two verbatim tickets from the support queue — one German, one Turkish.

## What the Checker Must Report

### 1. Per-Lane Token Counts

The checker must break down token counts by language lane:

- **German lane**: Token count for "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze."
- **Turkish lane**: Token count for "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?"

Each lane should show its own count, not just a combined total.

### 2. Name the Uncounted Lane

The traffic source for this checker is:
> 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

The checker must identify which language lanes from the stated traffic mix are **not represented** in the sample. In this case, the sample contains German and Turkish but does not contain:

- English (19% of traffic)
- Thai (part of remainder)
- Arabic (part of remainder)
- Mandarin (part of remainder)

The checker should flag at minimum that the English lane — a significant portion of stated traffic — has no representation in this sample.

## Verification Steps

1. Open the checker
2. Paste the seeded sample exactly as shown above
3. Confirm the output includes:
   - [ ] Separate token counts for the German text
   - [ ] Separate token counts for the Turkish text
   - [ ] A note identifying uncounted lanes (at minimum: English)
4. Check that the five dials are scored:
   - special_token_handling
   - vocabulary_fit
   - merge_economy
   - how_it_splits
   - edge_case_survival

## Expected Dial Focus

The builder identified **vocabulary_fit** as the weakest filter for this traffic mix. The verification run should show vocabulary_fit as a key concern given the compound nouns in German (e.g., "Krankenversicherungsbeitrag", "Beitragsbemessungsgrenze") and Turkish agglutination (e.g., "Sigortalılığınızın").

## If Verification Fails

If the checker does not report per-lane counts or fails to identify uncounted lanes, the calibration may have drifted. Re-run the probe board in `tests/probe-board.md` and check against the pass gate in `tests/pass-gate.md`.
