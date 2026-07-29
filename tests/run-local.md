# Run-Local Guide

Three ways to run the token-fit checker probes locally, from zero-tooling to CI integration.

---

## Rung 1: Manual Paste Protocol

For each probe in `tests/probes.jsonl`, follow this protocol:

### Steps

1. Open your chat interface with the token-fit checker loaded (see `blueprints/token-fit-checker.md` for system instructions)
2. Copy the probe's `input` field **exactly as written** — do not reformat
3. Paste into the checker
4. Compare the checker's output against the probe's `expected` field

### Expected Lines Reference

| Probe ID | Input (first 40 chars) | Expected Behavior |
|----------|------------------------|-------------------|
| pregen-01 | German compound noun sample | Reports per-lane token count, flags vocabulary_fit |
| pregen-02 | Turkish agglutinative sample | Reports merge_economy stress |
| pregen-03 | Mixed German+Turkish ticket | Per-lane counts for both languages |
| pregen-04 | English baseline | Baseline token count for comparison |
| pregen-05 | Thai script sample | Flags edge_case_survival for unseen script |
| pregen-06 | Arabic RTL sample | Reports special_token_handling for directionality |

### Byte-Preservation Warning

**Critical:** When pasting multilingual samples, ensure your clipboard and chat interface preserve the original bytes.

- Do not let autocorrect "fix" German umlauts (ä, ö, ü, ß)
- Do not let the interface normalize Turkish characters (ı, İ, ş, ğ, ç)
- Copy from a plain-text editor, not a rich-text source
- If the checker's token count differs wildly from expected, re-paste from `probes.jsonl` directly

A single byte substitution (e.g., `ü` → `u`) changes the token count and invalidates the probe.

---

## Rung 2: Script Runner (~20 lines)

Embed this runner in your local environment. Requires an API key in your environment variables.

### runner.py

```python
import json
import os
import sys

# Reads API key from environment
API_KEY = os.environ.get("LLM_API_KEY")
if not API_KEY:
    print("Error: Set LLM_API_KEY in environment")
    sys.exit(1)

# Load probes
with open("tests/probes.jsonl", "r", encoding="utf-8") as f:
    probes = [json.loads(line) for line in f if line.strip()]

# Load system prompt
with open("blueprints/token-fit-checker.md", "r", encoding="utf-8") as f:
    system_prompt = f.read()

results = []
for probe in probes:
    # Replace this block with your LLM API call
    # response = call_llm(system_prompt, probe["input"])
    response = "[YOUR_LLM_CALL_HERE]"
    passed = probe["expected"].lower() in response.lower()
    results.append({"id": probe["id"], "passed": passed, "response": response[:100]})

# Print graded grid
print("\n=== PROBE RESULTS ===")
print(f"{'ID':<12} {'PASS':<6} {'RESPONSE (truncated)'}")
print("-" * 50)
for r in results:
    status = "✓" if r["passed"] else "✗"
    print(f"{r['id']:<12} {status:<6} {r['response'][:30]}...")

# Gate verdict
pass_count = sum(1 for r in results if r["passed"])
total = len(results)
print(f"\n=== GATE VERDICT ===")
print(f"Passed: {pass_count}/{total}")
# Note: pass_gate threshold not specified by builder
print("Gate status: [Builder did not specify pass_gate threshold]")
```

### Usage

```bash
export LLM_API_KEY="your-key-here"
python runner.py
```

The script prints a graded grid showing each probe's pass/fail status and a truncated response, followed by the gate verdict.

---

## Rung 3: Eval Tool / CI Integration

Load `tests/probes.jsonl` into any eval runner so the board re-runs automatically when prompts or stance change.

### probes.jsonl Format

Each line is a JSON object:
```json
{"id": "pregen-01", "name": "German compound", "input": "...", "targets": ["vocabulary_fit"], "expected": "...", "invariant": "..."}
```

### Integration Options

**Generic eval framework:**
```yaml
# eval-config.yaml
probes_file: tests/probes.jsonl
system_prompt_file: blueprints/token-fit-checker.md
pass_threshold: 0.75
output_format: grid
```

**CI pipeline (GitHub Actions example):**
```yaml
- name: Run token-fit probes
  run: |
    python runner.py > board-output.txt
    diff board-output.txt tests/certified-board.txt || echo "Board drift detected"
```

**Trigger conditions:**
- On any change to `blueprints/token-fit-checker.md`
- On any change to `skills/token-fit-advisor.skill.md`
- On stance or refusal policy updates
- Weekly scheduled re-run for drift detection

---

## Diffing Against the EP-Certified Board

After running locally, compare your results to the certified board on the EducationPals listing:

### Manual Diff

1. Run the local board: `python runner.py > local-board.txt`
2. Download the certified board from the EP listing page
3. Compare:
   ```bash
   diff local-board.txt ep-certified-board.txt
   ```

### What Drift Means

| Drift Type | Cause | Action |
|------------|-------|--------|
| New failures | Model update changed tokenization | Re-calibrate dials, update expected values |
| New passes | Prompt improvement | Update certified board via re-certification |
| Per-lane count shift | Vocabulary table change | Review vocabulary_fit dial threshold |

### Re-Certification

If local results diverge from the certified board:

1. Document which probes drifted and in which direction
2. Determine if drift is acceptable (model improvement) or regression
3. If acceptable, submit updated `probes.jsonl` with new expected values for re-certification
4. If regression, revert prompt changes and re-run

---

## Notes on This Build

The builder's pass_gate was not fully specified:
> I dont have it. I dont have it. I dont have it

Until a concrete metric, threshold, and re-run trigger are defined, the gate verdict in the script will report "unspecified." Define these in `tests/pass-gate.md` to enable automated pass/fail gating.
