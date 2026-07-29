# What I Built

I built a token cost and fit checker for multilingual support tickets. The checker evaluates text samples against five dials—special token handling, vocabulary fit, merge economy, how it splits, and edge case survival—to determine whether a tokenizer is suitable for our on-device assistant deployment.

The problem I was solving: we have a 14-day support queue export with 38% German, 22% Turkish, 19% English, and the remainder Thai, Arabic, and Mandarin. The embedding table is capped and inference is billed per token, so vocabulary fit matters for both cost and quality.

## The Probe That Fooled It

I was not able to complete the probe board with my own test cases. When asked for probes, I submitted: "I dont have it" for both entries. This means the checker shipped without learner-authored edge cases, and I cannot quote a probe result that exposed a weakness in my calibration.

This is a failure. A checker without probes that stress its weakest dial—vocabulary fit, in my case—is a checker that has not been tested against the traffic it claims to handle.

## The Fix

The fix remains incomplete. I identified vocabulary_fit as the weakest dial, but I did not supply the concrete measurements needed to calibrate it. My cost note was: "I dont know I am not a language translator." My split note observed: "I dont see any english reference even though it claims 19% english." Neither of these meets the standard for a calibration record.

The advisor stance I defined—listening to Salesforce CRM events, reading text files, uploading translations, refusing emojis and blacklisted words—gives the checker a shape. But without the seeded run comparison (my answer: "I dont have this information on me right now so I cant provide"), the advisor's dial readings remain unverified against my own.

## The Gate It Holds

I was not able to define a proper gate. My submission: "I dont have it. I dont have it. I dont have it."

A checker without a gate—metric, threshold, re-run trigger—is a checker that cannot fail. That means it cannot be trusted.

## Re-Certification Cadence

The architecture review deadline was Thursday. Any re-certification would need to happen before that review, with the Salesforce CRM export as the source of new samples.

## The Domain Lesson

The lesson is simple: a verdict without its failure cost is not a verdict. I wrote that vocabulary accuracy should be more than 90% for acceptability, but I did not name what happens if I am wrong—the bill, the sequence-length ceiling, the retrain cost. I wrote a flip condition (below 60% would be unacceptable) but did not name who produces that measurement or by when.

A checker carries the builder's counting discipline. Mine carried gaps where the discipline should have been. The provenance record shows what was drafted and what was missing. A stranger opening this repo will see exactly where I stopped short.
