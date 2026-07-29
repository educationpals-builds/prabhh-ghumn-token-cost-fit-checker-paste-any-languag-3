# Token Cost + Fit Checker

A conversational checker that evaluates multilingual text samples for tokenizer fit and cost implications. Built for teams choosing vocabulary tables and billing models for on-device assistants.

---

## How This Checker Was Built

This checker was calibrated against a real support queue containing German, Turkish, English, Thai, Arabic, and Mandarin tickets. The builder ran their own sample through the five-dial framework, recorded the verdict, and encoded that calibration into the system instructions.

The five dials:
1. **special_token_handling** — how the tokenizer treats special tokens
2. **vocabulary_fit** — coverage of the target language's vocabulary
3. **merge_economy** — efficiency of subword merges
4. **how_it_splits** — observable tokenization behavior
5. **edge_case_survival** — handling of edge cases (emoji, code, mixed scripts)

---

## Worked Example: The Builder's Sample + Verdict

### Pinned Sample (verbatim)

> "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

### Traffic Source

14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

### What This Decides

Picks the vocabulary for the on-device assistant — the embedding table is capped and inference is billed per token

### Decision Deadline

Thursday's architecture review

### Weakest Dial

**vocabulary_fit**

### Verdict

The vocabulary accuracy metrics should be more than 90% accurate for it to be acceptable.

### Flip Condition

If the vocabulary accuracy metrics is lower than 60%, it would be unacceptable.

### Sharpest Test

Run 100 samples and it meets the bare min 90% accuracy.

---

## One-Paste Rebuild Block

To rebuild this checker from scratch:

1. Copy the system instructions from `blueprints/token-fit-checker.md`
2. Paste into any chat model that supports system prompts
3. Load the calibration from `data/lane-fit-sheet.md`
4. Run verification using `VERIFY.md` protocol
5. Confirm against `tests/probe-board.md`

```
blueprints/token-fit-checker.md   → system instructions
data/lane-fit-sheet.md            → calibration record
skills/token-fit-advisor.skill.md → portable skill file
prompts/token-fit-pack.md         → standalone prompt pack
tests/probes.jsonl                → machine-readable probes
```

---

## Repository Structure

| Path | Purpose |
|------|---------|
| `charter.md` | Full builder run: sample, dials, verdict, flip condition |
| `blueprints/token-fit-checker.md` | One-paste system instructions |
| `prompts/token-fit-pack.md` | 5 standalone prompts, one per dial |
| `METHOD.md` | The framework definition |
| `VERIFY.md` | Stranger verification protocol |
| `skills/token-fit-advisor.skill.md` | Portable skill file |
| `data/lane-fit-sheet.md` | Calibration record with seeded samples |
| `tests/probe-board.md` | All probes with results grid |
| `tests/pass-gate.md` | Gate definition and rulings |
| `tests/probes.jsonl` | Machine-readable probe export |
| `tests/run-local.md` | Run-anywhere guide |
| `STORY.md` | Builder's first-person story |

---

## Quick Start

1. Read `charter.md` to understand the calibration sample
2. Paste the system instructions from `blueprints/token-fit-checker.md` into your model
3. Test with the German+Turkish sample above
4. Verify per-lane counts are reported for each language

---

## Stream

We have CRM at salesforce where we have record of all the text interactions with the customer support.

---

## License

This checker and its calibration are provided as-is for evaluation purposes.

<!-- educationpals-build-verified -->
