# Token Fit Prompt Pack

Five standalone prompts for evaluating token cost and vocabulary fit. Each prompt targets one dial and can be used in any chat model. Paste the sample text where indicated.

---

## 1. Special Token Handling (0–4)

```
You are a tokenizer analyst. Evaluate how well a tokenizer handles special tokens in the following text.

TEXT TO ANALYZE:
[paste sample here]

Score from 0–4:
- 0: Special tokens (BOS, EOS, PAD, UNK) are mishandled or corrupt output
- 1: Special tokens present but inconsistently applied
- 2: Special tokens work but edge cases (nested quotes, control chars) fail
- 3: Handles most special token scenarios correctly
- 4: Robust special token handling across all edge cases

Report:
1. List any special tokens detected or needed
2. Note any corruption or mishandling
3. Give your score with one-sentence justification
```

---

## 2. Vocabulary Fit (0–4)

```
You are a vocabulary coverage analyst. Evaluate how well a tokenizer's vocabulary fits the following multilingual text.

TEXT TO ANALYZE:
[paste sample here]

Score from 0–4:
- 0: Majority of words fall back to byte-level or UNK tokens
- 1: Many domain terms and non-English words fragment excessively
- 2: Common words tokenize well; domain-specific and compound words fragment
- 3: Good coverage; only rare terms fragment
- 4: Excellent vocabulary fit; minimal fragmentation across all languages

Report:
1. Identify language lanes present (e.g., German, Turkish, English)
2. For each lane, note words that fragment vs. words that tokenize cleanly
3. Flag any domain-specific terms (e.g., "Krankenversicherungsbeitrag", "Beitragsbemessungsgrenze", "Sigortalılığınızın")
4. Give your score with one-sentence justification
```

---

## 3. Merge Economy (0–4)

```
You are a tokenization efficiency analyst. Evaluate the merge economy — how efficiently the tokenizer combines subwords — for the following text.

TEXT TO ANALYZE:
[paste sample here]

Score from 0–4:
- 0: Excessive fragmentation; token count far exceeds word count
- 1: Poor merge economy; many unnecessary splits
- 2: Moderate efficiency; some wasteful splits on common patterns
- 3: Good merge economy; most merges are sensible
- 4: Excellent efficiency; near-optimal token-to-word ratio

Report:
1. Estimate token count vs. word count ratio
2. Identify any words that split into 4+ tokens
3. Note whether compound words (German, Turkish) merge efficiently or fragment
4. Give your score with one-sentence justification
```

---

## 4. How It Splits (0–4)

```
You are a tokenization boundary analyst. Evaluate where the tokenizer places split boundaries in the following text.

TEXT TO ANALYZE:
[paste sample here]

Score from 0–4:
- 0: Splits at arbitrary byte positions; no linguistic awareness
- 1: Splits often break morphemes or meaningful units
- 2: Splits are mostly at morpheme boundaries but inconsistent
- 3: Splits respect morphemes; occasional odd breaks
- 4: Splits align with linguistic structure across all languages

Report:
1. Show example splits for 2–3 complex words
2. Note whether splits preserve meaning (e.g., "Kranken|versicherungs|beitrag" vs. "Krank|enversich|erungsbei|trag")
3. Check if Turkish agglutinative suffixes split sensibly
4. Give your score with one-sentence justification
```

---

## 5. Edge Case Survival (0–4)

```
You are a tokenizer stress-test analyst. Evaluate how the tokenizer handles edge cases in the following text.

TEXT TO ANALYZE:
[paste sample here]

Score from 0–4:
- 0: Crashes, hangs, or produces garbage on edge cases
- 1: Handles basic text but fails on mixed scripts, punctuation clusters, or special chars
- 2: Survives most edge cases but output is inconsistent
- 3: Robust on common edge cases; rare failures
- 4: Handles all edge cases gracefully including mixed scripts, emoji, URLs, code

Report:
1. Test for: mixed-language boundaries, punctuation (em-dash, quotes), special characters
2. Note any characters that produce UNK or unexpected tokens
3. Check behavior on the German umlaut (ü, ö, ä) and Turkish dotted/dotless i (ı, İ)
4. Give your score with one-sentence justification
```

---

## Usage Notes

- **Calibration anchor**: The builder's pinned sample for this checker is a German+Turkish support ticket pair:
  > "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?"

- **Traffic context**: 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

- **Weakest dial for this domain**: vocabulary_fit — the deciding factor for on-device assistant vocabulary selection

- **Per-lane reporting**: When analyzing multilingual text, always report token counts and scores per language lane, not just aggregate

- **What these prompts do not cover**: Actual cost calculation (billing per token), sequence-length ceiling checks, or embedding table capacity — those require runtime instrumentation beyond prompt-based analysis
