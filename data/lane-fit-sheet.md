# Lane Fit Sheet — Calibration Record

This data sheet documents the seeded samples, per-language lane counts, advisor dial readings, and the builder's drift rulings that calibrate the token cost + fit checker.

---

## Seeded Samples

### Sample 1: German Insurance Ticket
```
Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze.
```
**Language lane:** German  
**Character count:** 94  
**Estimated token count (cl100k_base):** ~18–22 tokens  
**Notable features:** Compound noun "Beitragsbemessungsgrenze" (contribution assessment limit) — a single word that may split into 4–6 subword tokens depending on tokenizer vocabulary coverage.

### Sample 2: Turkish Insurance Inquiry
```
Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?
```
**Language lane:** Turkish  
**Character count:** 56  
**Estimated token count (cl100k_base):** ~14–18 tokens  
**Notable features:** Agglutinative morphology — "Sigortalılığınızın" contains multiple suffixes that may each become separate tokens.

---

## Per-Language Lane Counts

Based on traffic source: 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

| Lane | Traffic Share | Token Efficiency Risk | Notes |
|------|---------------|----------------------|-------|
| German | 38% | Medium-High | Compound nouns inflate token counts |
| Turkish | 22% | High | Agglutinative structure, poor vocabulary fit |
| English | 19% | Low | Best vocabulary coverage in most tokenizers |
| Thai | ~7% | Very High | Script may lack dedicated vocabulary |
| Arabic | ~7% | High | Right-to-left, diacritics handling varies |
| Mandarin | ~7% | Variable | Character-based, depends on tokenizer training |

---

## Advisor Dial Strips

The advisor evaluated both seeded samples against the five dials:

### German Sample Dial Strip
| Dial | Score | Advisor Notes |
|------|-------|---------------|
| special_token_handling | 2 | Standard punctuation, em-dash present |
| vocabulary_fit | 2 | Compound nouns split heavily |
| merge_economy | 2 | Poor merge efficiency on domain terms |
| how_it_splits | 2 | "Beitragsbemessungsgrenze" → multiple pieces |
| edge_case_survival | 2 | Insurance terminology may be undertrained |

### Turkish Sample Dial Strip
| Dial | Score | Advisor Notes |
|------|-------|---------------|
| special_token_handling | 2 | Turkish-specific characters (ı, ş, ğ) |
| vocabulary_fit | 2 | Agglutinative suffixes poorly merged |
| merge_economy | 2 | Each suffix often becomes separate token |
| how_it_splits | 2 | Long words fragment extensively |
| edge_case_survival | 2 | Low-resource language in most tokenizers |

---

## Builder's Drift Ruling

**Advisor run verdict from builder:**  
I dont have this information on me right now so I cant provide.

**Drift assessment:** Unable to complete — builder did not provide comparison between advisor readings and their own dial scores.

**Weakest dial identified by builder:** vocabulary_fit

---

## Stance Line Added

Based on the advisor stance defined by the builder:

> It can listen to events from CRM for new enteries and it reads the text files for language to translate, and it uploads the translated data to text files on CRM. But it refuses emojis and blacklisted words.

**Refusal line added to calibration:**  
The advisor refuses to process emojis and blacklisted words — these inputs should be flagged rather than scored.

---

## Calibration Status

| Component | Status |
|-----------|--------|
| Seeded samples | ✓ Documented |
| Per-language lanes | ✓ Mapped from traffic source |
| Advisor dial strips | ✓ Generated |
| Builder drift ruling | ✗ Not provided |
| Stance refusal | ✓ Documented |

---

*This sheet serves as the calibration anchor for the token cost + fit checker. Re-run when traffic mix shifts or new language lanes are added.*
