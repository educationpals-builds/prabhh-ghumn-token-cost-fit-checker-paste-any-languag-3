# METHOD.md

## The TFLEC Framework

This checker is built on the **TFLEC** framework — five dials that measure whether a tokenizer fits a given text stream.

### The Five Dials

| Dial | What It Measures |
|------|------------------|
| **T** — Special **T**oken Handling | How the tokenizer treats special tokens (BOS, EOS, PAD, UNK) and whether they leak into or corrupt the output |
| **F** — Vocabulary **F**it | Whether the tokenizer's vocabulary covers the language mix in the stream without excessive UNK fallback |
| **L** — Merge Economy (token **L**ength) | How efficiently the tokenizer merges characters — fewer tokens per semantic unit means lower cost |
| **E** — How It Splits (**E**xplosion) | Whether compound words, technical terms, or script boundaries cause token explosion |
| **C** — Edge **C**ase Survival | How the tokenizer handles emoji, code-switching, mixed scripts, URLs, and other edge traffic |

### Scoring

Each dial scores 0–4:

- **0** — Fails outright; unusable for this stream
- **1** — Severe issues; would require major workarounds
- **2** — Marginal; works but with known cost or quality penalties
- **3** — Acceptable; minor issues that don't block deployment
- **4** — Strong fit; no concerns for this dial

### Using the Framework

1. **Pin a sample** — real bytes from the stream you'll process
2. **Score each dial** — with per-language lane counts where the stream is multilingual
3. **Identify the weakest dial** — this is your decision point
4. **Call the verdict** — with the cost of being wrong
5. **Name the flip condition** — what measurement would reverse your call

The framework forces you to name what you're not measuring. A cost estimate without a per-language breakdown is incomplete. A verdict without a failure cost is opinion.

---

*This is the only file in this repository where the TFLEC acronym appears. All other files reference the dials by their full names.*
