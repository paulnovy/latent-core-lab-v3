# Evidence behind the public results

This page states what the published aggregates measure and how to interpret them. All results are local experimental observations, not external benchmark certifications.

## October follow-ups

Public update: 28 October 2026. The latest completed campaign reported here finished on 24 October. The September evidence below remains historical; different models, prompts, populations, and scoring protocols prevent direct comparison of their absolute scores.

### Fixed interventions and adaptive allocation

The backbone was Qwen3-0.6B-Base, frozen throughout training and evaluation. Lightweight interventions acted before answer generation; the frozen language model produced the answer. The public labels A and B identify two different fixed policies without disclosing their construction or training recipe.

The confirmation cohort contains 128 questions sampled from the **GSM8K training split**, disjoint by ID and exact question text from this campaign's training, routing, development, and retention banks. Fixed configurations and the controller were selected before confirmation was opened, according to the saved freeze records. Those records support the recorded ordering; file integrity is not an independent certification of chronological blindness. **34/128 confirmation IDs also appear in older prepared 8B project data**; preparation overlap does not by itself establish training exposure. Project-wide historical exposure and pretraining contamination are not ruled out. This campaign did not evaluate the official GSM8K test split.

All conditions used the same numerical-answer scorer and maximum visible-generation allowance. The scorer required a parseable final numerical answer; truncated generations were counted as incorrect. This is protocol-specific answer correctness, not a standard unrestricted benchmark score.

| Condition | Correct / 128 | Accuracy | Gain vs baseline | Paired 95% interval | Rescued / regressed |
|---|---:|---:|---:|---:|---:|
| Baseline | 42 | 32.81% | — | — | — |
| Fixed policy A | 64 | 50.00% | +17.19 pp | +7.81 to +26.56 pp | 33 / 11 |
| Fixed policy B | 60 | 46.88% | +14.06 pp | +5.47 to +22.66 pp | 27 / 9 |
| Learned adaptive allocation | 56 | 43.75% | +10.94 pp | +2.34 to +20.31 pp | 25 / 11 |
| Matched randomized budget | 59 | 46.09% | +13.28 pp | +4.69 to +22.66 pp | 26 / 9 |
| Swapped budget | 57 | 44.53% | +11.72 pp | +3.12 to +20.31 pp | 24 / 9 |

Intervals in this table and the October policy rows in the CSV come from the saved deployment analysis: 10,000 paired problem-level bootstrap samples, conditional on the selected configurations, one seed, and this cohort, without multiplicity correction. The separate action-matrix analysis used different bootstrap draws; its fixed-A interval is +7.03 to +27.34 pp for the same answers. These are alternative resampling estimates of one comparison, not independent replications.

The adaptive controller did not establish useful question-conditioned allocation. It scored below both fixed policies and both matched budget controls. Against fixed A, its difference was −6.25 pp (95% interval −15.62 to +2.34); against matched randomization, −2.34 pp (−7.03 to +2.34). A controller variant without state updates scored 55/128 versus 56/128 with updates; the difference interval included zero. The swapped/randomized controls changed final budgets while retaining the recipient's own intervention coefficients, so they do not test replacement of the entire intervention or trajectory.

The visible answer averaged 64.07 tokens for baseline, 101.51 for fixed A, and 80.59 for fixed B. Fixed A also had six truncated answers versus zero for baseline. Additional visible calculation is a confound; the current comparison does not isolate latent computation or the contribution of learning against all untrained controls. No independently trained, single-budget specialist comparison was run.

Cross-stage timing was not comparable: 640 identical trajectories reproduced their answers but ran 27–30% faster in a later stage for an unresolved runtime reason. An offline comparison within the original measurement grid also failed to establish an adaptive-controller cost advantage. No latency, throughput, or cost-efficiency gain is claimed.

On the separate 32-question retention bank, baseline scored 12/32, fixed A 13/32, fixed B 10/32, and adaptive allocation 11/32. This small diagnostic does not establish preservation of baseline skills. Technical checks confirmed an unchanged backbone fingerprint and the native baseline path; that does not guarantee preserved behavior when interventions are active.

### A rejected question-conditioned pilot

A separate, earlier frozen-0.6B campaign evaluated a question-only learned intervention on 64 sealed mathematical problems after freezing its configuration. It scored **5/64 versus baseline 22/64**: 2 rescues and 19 regressions, a difference of **−26.56 pp**, with a paired problem bootstrap interval of **−39.06 to −14.06 pp** (20,000 samples). This cohort is separate from the 128-question policy campaign above.

Its short-task retention diagnostic also fell from 31/32 to 29/32 with the intervention active. Privileged teacher-information and per-query fitting controls were capacity diagnostics, not evidence of a successful question-only generator. A recurrent extension was not run. The decision was to stop this tested approach; a follow-up would need a new hypothesis and fresh evaluation, not further tuning on the opened sealed cohort.

### Verification and disclosure boundary

The saved policy-campaign audit passed input-integrity, split-disjointness, and rescoring checks for 3,664 primary answers, with 16 supplemental answers checked separately. The separate sealed-campaign audit checked 832 generations and unchanged backbone weights. These are local evidence-integrity checks, not an external replication or proof of reasoning. For this public update, the six policy-table arm totals and paired counts were recomputed from saved confirmation completions using the existing scorer; no models were retrained or rerun.

| Private source record | SHA-256 |
|---|---|
| Reviewed policy-campaign summary | `d5fef8c2aa91fe4dc732b06a090d55d58cb48a3b7309a655a8058117d680d736` |
| Policy-campaign evaluated trajectories | `9b60393227145805e495993e43e09532631578974b9e372d343e395b4178b234` |
| Policy-campaign data manifest | `3a368675cb06f005d6a65d04507772e0c88fce267fb354d4c9c5cf90a488b1a1` |
| Separate sealed-campaign summary | `12cc9a7c28487398d5442b05754c74882ccbf46395fde62b34f49b144a4803d5` |
| Separate sealed-campaign paired statistics | `6f7253bbee6b7444e9ea529ddaec0010cd12f3c9cb5772c62d8f597b3c24ab2e` |

The public CSV adds selected aggregate comparisons, including the failed pilot and matched controller controls. It is not the full experiment log. The fingerprints anchor retained private records; this package does not allow independent reproduction of the withheld implementation. Exact interventions, training objectives and recipes, checkpoints, raw completions, private paths, and infrastructure remain outside this release.

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
