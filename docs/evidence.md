# Evidence behind the public results

This page states what the published aggregates measure and how to interpret them. All results are local experimental observations, not external benchmark certifications.

## Synthetic depth generalization

The two task families are synthetic graph-traversal problems called merge and cycle. The frozen backbone is Qwen3-0.6B-Base. Training tasks have depths 1–4; the reported development comparisons cover depths 5–8 with 256 examples per depth. Reserved template comparisons cover the same greater depths with 128 examples per depth.

There is one model seed per family. Family and seed are confounded, so the two families are not two training-seed replications of the same task. Equal example counts per depth make the displayed macro accuracy equal to pooled accuracy.

The public CSV distinguishes three comparisons:

- `fixed_checkpoint`: the same trained model evaluated at four and eight inference iterations. The accuracy change is an inference-time effect at fixed weights.
- `training_time_control`: fresh one-pass controls versus the historical recurrent checkpoints evaluated at eight iterations. This compares different trained models and training schedules.
- `reserved_templates`: the same control/model comparison on reserved template variants. This establishes a bounded robustness result within these synthetic families.

The one-pass controls achieved 97.44% and 98.96% of the reference training wall time, and 96.96% and 97.85% of its summed optimizer intervals. Host kernels differed. Both controls reached 100% on in-distribution evaluation depths. The matching is approximate measured training time, not exact FLOPs or an architecture-only contrast.

Terminal-task 95% intervals are depth-stratified paired normal **item intervals**, conditional on the saved checkpoints. They do not quantify between-seed uncertainty or correct for multiple comparisons.

Input counterfactual checks provided an additional consistency signal: changed-answer accuracy was 91.80% and 91.41%; irrelevant-edit prediction stability was 98.05% and 97.66%, with 256 examples per family. Stability is prediction equality, not accuracy. Input interventions do not demonstrate internal causal mediation.

## Early math pilot

The backbone is frozen Qwen3-8B. The selected lightweight adaptation was assessed using the same constrained numerical-answer decoding as the unchanged baseline, with thinking disabled. The metric is exact numerical-answer correctness under that protocol.

Configuration selection used 384 discovery examples from the 1,319-example GSM8K test split. The headline result uses only the other **935 test examples**. It is therefore held apart from configuration selection, but it is one selected condition, not an independent training-seed replication. The 95% interval is a paired example bootstrap, conditional on the selected condition and checkpoint, without multiplicity correction.

The +3.74 pp result equals **35 net additional correct answers / 935**. The public CSV also supplies both arm totals and paired rescue/regression counts. The full-test descriptive result was 5.16% to 9.55%; it overlaps discovery and is deliberately not the confirmation headline.

This pilot does not establish gains under standard unconstrained generation, superiority to explicit chain-of-thought, or a general increase in computational capacity. A larger follow-up benchmark pilot was stopped at its hard budget before the planned coverage was complete and remains inconclusive.

## Results that bound the claim

A harder synthetic multi-input composition study used two recurrent training seeds. Both failed the predeclared 95% in-distribution competence gate: observed values were 68.85% and 63.09% at four iterations. Increasing to eight iterations reduced native development accuracy by 1.37 and 2.93 pp. Low competence leaves representation and optimization limitations unresolved; it does not prove that recurrent composition is impossible.

On a fixed ordinary-text diagnostic subset for those composition checkpoints, active-mode perplexity rose approximately 10.4% at four iterations and 26% at eight. This covered 128 blocks and 32,640 scored tokens per mode; it is not a full standard language-retention benchmark.

These outcomes argue against scaling the unchanged 0.6B implementation. Further validation requires more tests and additional compute; generalization and scaling remain open research questions.

## Inspecting the aggregates

`data/results.csv` contains only aggregate counts and numerical results. For every row:

```text
baseline_correct = stable_correct + regressed
experimental_correct = stable_correct + rescued
paired_n = stable_correct + stable_wrong + rescued + regressed
gain_pp = 100 * (rescued - regressed) / paired_n
```

Arm accuracy is `100 * correct / paired_n`. The figure uses the four native synthetic comparison rows and a zero-based percentage axis. Confidence intervals on gains are in the README/CSV; the figure displays point accuracies.

The figures and public table were extracted from preserved experimental records and checked against paired counts. Source-record fingerprints are listed below to anchor this snapshot. They are provenance identifiers, not a claim that the private records can be independently reproduced from this public package.

| Private source record | SHA-256 |
|---|---|
| Synthetic paired comparison record | `2209e7f0df01762007649f315293b2cf666656e91eebb9876fb037f5a8ebe49e` |
| Math baseline record | `1700cd388f6038169ae120b77c94e3d7253dbed01a1cc71c91838e3493b3c1a2` |
| Math adapted-condition record | `f7ff1d9a2dde5d6775f449f5007b43cf5650601c3dd634dd5c6cfc620859ce54` |
| Math selection record | `fdbc72c4dbfe395a241f1e9e35cc76179d7e10024a725b67c8e3422206ecc2c0` |
| Math paired interval record | `c9680831d5beb3bc80dad4e2401628609686fd468539b9b9e972ce898682fdd4` |

Full code, exact intervention construction, training recipes, weights, infrastructure manifests, per-question completions, and benchmark question text are outside this public release.
