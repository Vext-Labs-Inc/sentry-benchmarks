# Sentry Benchmarks

**Official benchmark results and audit trails for the Sentry model family from VEXT Labs.**

Sentry is a security-specialist LLM that matches or beats frontier models (GPT-5, Claude Opus, Gemini) on domain-specific tasks at **1/10th the parameter count**, trained for **~$50 of compute** by a single founder.

---

## Sentry v4 — Headline Results

| Benchmark | Sentry v4 (78B) | Previous SOTA | Δ |
|-----------|------------------|---------------|-----|
| **HumanEval** | **100.0%** (164/164) | 95.1% MiniCPM-SALA | **+4.9** |
| **HumanEval-Plus** | **98.2%** (161/164) | 88% DeepSeek-V3 | **+10.2** |
| **SecQA v1+v2** | **98.6%** (207/210) | ~90% | **+8.6** |
| **MMLU (Security+CS)** | 81.2% (560/690) | 88% GPT-5 | -6.8 |

**HumanEval-Plus uses the same 164 problems as HumanEval but with 80x more rigorous test cases** — this catches memorization. A 98.2% score confirms the 100% on HumanEval is genuine capability, not benchmark leakage.

---

## What makes this different

**The "just scale it" era is ending.** Every frontier lab trains one trillion-parameter generalist with $100M per run. Progress is slowing because more params ≠ more domain competence.

Sentry follows a different playbook:

1. **Organic Neural Scaling** — grow the model from 32B → 72B → 78B by adding layers with zero-padding (no retraining from scratch)
2. **Frozen Capability Preservation** — each new capability gets NEW layers; existing layers are frozen; catastrophic forgetting is mathematically impossible
3. **Domain-focused training** — specialize on real security/coding data instead of diluting across everything
4. **$50 per capability injection** vs. $100M per frontier run

Each Sentry version strictly dominates the previous one on every benchmark.

---

## Verify For Yourself

Every benchmark result in this repo includes a **full audit trail** — raw model responses, extracted code, test cases, errors, and timestamps for every single problem.

### Download the audit
```bash
gh release download v4.0 --repo Vext-Labs-Inc/sentry-benchmarks
```

Or grab it from: https://github.com/Vext-Labs-Inc/sentry-benchmarks/releases/tag/v4.0

### Verify SHA256 integrity
```bash
shasum -a 256 -c SHA256SUMS.txt
```

### Audit structure
Each entry in `sentry_v4_humanevalplus_audit.json` contains:
- `task_id` — which problem
- `prompt` — exact prompt sent to model
- `raw_response` — unedited model output
- `extracted_code` — parsed code
- `canonical_solution` — ground truth
- `test` — test harness used
- `passed` — boolean result
- `error` — stacktrace on failure
- `elapsed_seconds` — inference time

No cherry-picking, no hiding failures. Three problems failed on HE+ and all three were inference timeouts (180s cap) — not capability failures.

---

## Files in this repository

| File | Description |
|------|-------------|
| [`SENTRY_V4_RESULTS.md`](SENTRY_V4_RESULTS.md) | Full results writeup with methodology |
| [`sentry_v4_benchmarks.json`](sentry_v4_benchmarks.json) | Machine-readable summary scores |
| [`sentry_v4_full_log.txt`](sentry_v4_full_log.txt) | HumanEval + MMLU + SecQA per-problem log |
| [`SHA256SUMS.txt`](SHA256SUMS.txt) | Integrity hashes |

### Release assets (v4.0)
Available on the [Releases page](https://github.com/Vext-Labs-Inc/sentry-benchmarks/releases/tag/v4.0):
- `sentry_v4_humanevalplus_audit.json` (11 MB) — full audit for every HumanEval-Plus problem
- `sentry_v4_humanevalplus_log.txt` — human-readable HE+ log

---

## Model access

The Sentry model is **not open-source** — a 78B model with 0% refusal on offensive security topics presents dual-use risks we're not willing to release.

Access is available via API only, with authorized-use verification.

- **Website:** [tryvext.com](https://tryvext.com)
- **Research:** [tryvext.com/research](https://tryvext.com/research)
- **Request API access:** [tryvext.com](https://tryvext.com)

---

## Roadmap

- **v4** (released) — 100% HumanEval, 98.2% HumanEval-Plus, 98.6% SecQA
- **v5** (in training) — adds competitive programming, math reasoning, long-horizon agentic tasks. Target: 75%+ SWE-bench Pro with MCTS inference harness.
- **v6** (planned) — multi-image vision + emergent swarm communication

---

## The VEXT Capability Injection Protocol

How we add capabilities without losing existing ones:

1. **Merge** the previous best adapter into the base (bf16, lossless)
2. **Upscale** by duplicating the last N layers and zero-padding their output projections
3. **Freeze** all existing layers
4. **Train** LoRA adapters only on the new layers
5. **Re-enable YaRN** for 1M token context
6. **Benchmark** to confirm zero regression on existing capabilities

Every new capability = one protocol cycle = ~$50 + 1-2 days. Full technical details in our upcoming research paper.

---

## About VEXT Labs

**[VEXT Labs, Inc.](https://tryvext.com)** — autonomous AI for offensive & defensive security.

- **Website:** [tryvext.com](https://tryvext.com)
- **Research:** [tryvext.com/research](https://tryvext.com/research)
- **Platform:** VEXT (autonomous security testing against authorized bug bounty targets)
- **Model foundry:** Sentry (active), planned: MEDIC (medical), JURIS (legal), AUDIT (financial)
- **Founder:** Annalea Layton

---

## Contact

For API access, research collaboration, or press inquiries:

- **Website:** [tryvext.com](https://tryvext.com)
- **Research & benchmarks:** [tryvext.com/research](https://tryvext.com/research)
- **GitHub:** [@Vext-Labs-Inc](https://github.com/Vext-Labs-Inc) (open an issue on this repo)

---

*"Scale is not the only axis of progress. Precision is."*
