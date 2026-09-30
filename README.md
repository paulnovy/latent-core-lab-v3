# Latent Core Lab v3

**More useful computation before the answer.**

An independent research project investigating whether frozen language models can gain accuracy from additional internal computation, without retraining the full backbone. Early experiments show strong depth generalization on controlled synthetic tasks and a smaller, independently held-out gain on mathematical questions.

**Further validation requires more tests and additional compute.**

## Promising results

### 1. Extra inference computation improved a fixed checkpoint

On two synthetic graph-traversal families, a 0.6B model trained on depths 1–4 was evaluated on unseen training depths 5–8. Increasing inference computation from four to eight iterations, with **the same trained weights**, produced:

| Task family | Four iterations | Eight iterations | Paired gain | 95% item interval |
|---|---:|---:|---:|---:|
| Merge | 77.05% | **95.70%** | **+18.65 pp** | +16.32 to +20.98 pp |
| Cycle | 67.87% | **93.26%** | **+25.39 pp** | +22.95 to +27.83 pp |

Each comparison uses 1,024 identical evaluation examples. These are development evaluations at greater task depth, with one training seed per family; they are not a multi-seed general reasoning benchmark.

![Measured accuracy with more inference computation and stronger training controls](assets/results.png)

### 2. The advantage survived a stronger training-budget control

Fresh one-pass controls were trained for approximately the same measured training wall time. The eight-iteration models reached **95.70% vs 47.46%** on merge and **93.26% vs 55.66%** on cycle: gains of **48.24** and **37.60 percentage points**. Both controls reached 100% accuracy on in-distribution evaluation depths.

On reserved template variants at depths 5–8, the advantage remained: **90.23% vs 44.14%** and **83.01% vs 53.52%**, with 512 paired examples per family. This comparison involves different trained models; it is separate from the fixed-checkpoint result above. Training time was approximately matched, not independently measured FLOPs.

### 3. An early 8B math signal

A small learned intervention in a **frozen Qwen3-8B** improved direct-answer accuracy by **3.74 percentage points** on **935 GSM8K test questions held apart from configuration selection**: 35 net additional correct answers, with a paired 95% bootstrap interval of **+1.82 to +5.88 pp**.

All compared arms used the same constrained numerical-answer protocol with thinking disabled. This is a task-specific pilot, not a standard unconstrained GSM8K score or a comparison against explicit chain-of-thought reasoning. The condition was selected on a separate discovery subset; its confirmation interval is conditional on that selection and checkpoint.

[Aggregate data](data/results.csv) · [Evidence and limitations](docs/evidence.md) · [Vector figure](assets/results.svg)

## What remains open

- The strongest gains are on synthetic tasks, with one seed per family. Item intervals do not establish reliability across training seeds.
- Harder multi-input composition did not meet the predeclared competence and transfer criteria. Separate active-mode language diagnostics also showed a measurable regression.
- A later natural-thinking benchmark pilot stopped at its budget limit and remains inconclusive.
- General reasoning gains, internal causal mechanisms, production utility, and an efficiency advantage over strong baselines remain to be demonstrated.

## Public scope and collaboration

This repository contains a selected research summary, aggregate results, and figures. Implementation details, training recipes, checkpoints, and raw benchmark traces remain private. The public package supports inspection of the stated results; it is not a release of the full experimental system.

For a research collaboration, [open a GitHub issue](https://github.com/paulnovy/latent-core-lab-v3/issues) or contact [paulnovy on GitHub](https://github.com/paulnovy).

Public evidence snapshot: **30 September 2026**.
