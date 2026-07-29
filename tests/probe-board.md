# Probe Board — Token Cost + Fit Checker

This board contains all 8 probes used to verify the checker's calibration: 6 pre-generated probes covering the five dials, plus 2 learner-authored probes.

---

## The Five Dials

| Dial | Description |
|------|-------------|
| special_token_handling | How the tokenizer handles special tokens, control characters, BOM markers |
| vocabulary_fit | Whether the vocabulary covers the language mix without excessive unknown tokens |
| merge_economy | How efficiently the tokenizer merges common sequences |
| how_it_splits | The actual piece count for compound words and phrases |
| edge_case_survival | Handling of emoji, code-switching, mixed scripts, rare Unicode |

---

## Pre-Generated Probes (1–6)

### Probe 1: German Compound Noun

**Input (pasteable):**
```
Krankenversicherungsbeitrag
```

**Targeted Dial:** how_it_splits

**Expected Behavior:** Reports piece count for this compound; a vocabulary-fit tokenizer should produce fewer than 8 pieces; a poor fit produces 10+.

**Per-Lane Counts:**
- German lane: 1 word, token count varies by tokenizer

---

### Probe 2: Turkish Agglutination

**Input (pasteable):**
```
Sigortalılığınızın
```

**Targeted Dial:** vocabulary_fit

**Expected Behavior:** Reports whether this agglutinated form is in vocabulary or split into morphemes; flags if split exceeds 5 pieces.

**Per-Lane Counts:**
- Turkish lane: 1 word, token count varies by tokenizer

---

### Probe 3: Mixed Script Sentence

**Input (pasteable):**
```
Der Kunde sagte "Teşekkürler" und ging.
```

**Targeted Dial:** edge_case_survival

**Expected Behavior:** Reports per-lane counts for German and Turkish within the same sentence; flags any unknown token markers.

**Per-Lane Counts:**
- German lane: 5 words
- Turkish lane: 1 word

---

### Probe 4: Special Token Boundary

**Input (pasteable):**
```
<|endoftext|>Beitragsbemessungsgrenze
```

**Targeted Dial:** special_token_handling

**Expected Behavior:** Reports whether the special token is recognized and isolated; the German word should tokenize normally after it.

**Per-Lane Counts:**
- Special tokens: 1
- German lane: 1 word

---

### Probe 5: Thai Script Sample

**Input (pasteable):**
```
ขอบคุณครับ
```

**Targeted Dial:** vocabulary_fit

**Expected Behavior:** Reports piece count; flags if tokenizer falls back to byte-level encoding (piece count > 10 for this short phrase).

**Per-Lane Counts:**
- Thai lane: 1 phrase

---

### Probe 6: Arabic with Diacritics

**Input (pasteable):**
```
شُكْراً جَزِيلاً
```

**Targeted Dial:** edge_case_survival

**Expected Behavior:** Reports whether diacritics are preserved or stripped; flags if diacritics cause token explosion.

**Per-Lane Counts:**
- Arabic lane: 2 words

---

## Learner-Authored Probes (7–8)

### Probe 7

**Input (pasteable):**
```
I dont have it
```

**Targeted Dial:** (not specified)

**Expected Behavior:** (not specified)

**Per-Lane Counts:** (not available)

---

### Probe 8

**Input (pasteable):**
```
I dont have it
```

**Targeted Dial:** (not specified)

**Expected Behavior:** (not specified)

**Per-Lane Counts:** (not available)

---

## Results Grid — This Run

| Probe | Input Summary | Targeted Dial | Expected | Actual | Per-Lane Counts | Pass/Fail |
|-------|---------------|---------------|----------|--------|-----------------|-----------|
| 1 | Krankenversicherungsbeitrag | how_it_splits | <8 pieces | — | German: 1 word | — |
| 2 | Sigortalılığınızın | vocabulary_fit | <5 pieces | — | Turkish: 1 word | — |
| 3 | Mixed DE+TR sentence | edge_case_survival | No UNK tokens | — | DE: 5, TR: 1 | — |
| 4 | Special token + German | special_token_handling | Token isolated | — | Special: 1, DE: 1 | — |
| 5 | Thai phrase | vocabulary_fit | <10 pieces | — | Thai: 1 phrase | — |
| 6 | Arabic with diacritics | edge_case_survival | Diacritics preserved | — | Arabic: 2 words | — |
| 7 | I dont have it | — | — | — | — | — |
| 8 | I dont have it | — | — | — | — | — |

---

## Board Reading

> I dont have it. I dont have it. I dont have it

**Note:** The learner did not provide board reading results. The weakest dial across all probes and the direction of failure (generous or harsh) cannot be determined from this run.

---

## Calibration Anchor

The pinned sample for this checker:

> "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

Traffic source: 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

Weakest filter identified: vocabulary_fit

---

## How to Use This Board

1. Paste each probe input into the checker
2. Record the dial scores and per-lane token counts
3. Compare against expected behavior
4. Mark pass/fail in the grid
5. Identify the weakest dial across all 8 probes
6. Note direction of failure: generous (scores too high) or harsh (scores too low)

The board re-runs whenever the prompt or stance changes. See `tests/run-local.md` for automation options.
