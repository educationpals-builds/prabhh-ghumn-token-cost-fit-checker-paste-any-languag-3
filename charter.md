# Token Cost + Fit Checker — Charter

This document records the builder's full calibration run: the pinned sample, traffic context, stakes, deadline, five-dial scores, verdict, flip condition, and the demanded test.

---

## Pinned Sample

**Verbatim bytes:**

> "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

---

## Traffic Source

14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

---

## What This Decides

Picks the vocabulary for the on-device assistant — the embedding table is capped and inference is billed per token

---

## Decision Deadline

Thursday's architecture review

---

## Split Observation

I dont see any english reference even though it claims 19% english

---

## Cost Observation

I dont know I am not a language translator

---

## Five Dial Scores

| Dial | Score (0–4) |
|------|-------------|
| special_token_handling | 2 |
| vocabulary_fit | 2 |
| merge_economy | 2 |
| how_it_splits | 2 |
| edge_case_survival | 2 |

**Weakest Dial:** vocabulary_fit

---

## Verdict

The vocabulary accuracy metrics should be more than 90% accurate for it to be acceptable.

---

## Flip Condition

If the vocabulary accuracy metrics is lower than 60%, it would be unacceptable.

---

## Demanded Test

Run 100 samples and it meets the bare min 90% accuracy.

---

## Sample Stream

We have CRM at salesforce where we have record of all the text interactions with the customer support.

---

## Advisor Stance

It can listen to events from CRM for new enteries and it reads the text files for language to translate, and it uploads the translated data to text files on CRM. But it refuses emojis and blacklisted words.

---

*This charter anchors the checker's calibration. All dial scores, the verdict, and the flip condition above are the builder's own readings on the pinned sample.*
