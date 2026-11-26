---
title: "Large-Scale Diverse Synthesis for Mid-Training"
collection: publications
category: conferences
permalink: /publication/2026-08-02-large-scale-diverse-synthesis
date: 2026-08-02
venue: 'ACL 2026'
ccf: 'A'
paperurl: 'https://arxiv.org/abs/2508.01326'
authors: '<strong>Xuemiao Zhang</strong>, C. Tu, C. Ren, R. Weng, H. Yan, J. Wang, X. Cai'
excerpt: 'We propose a novel diversified pipeline that synthesizes BoostQA, a 100B-token, STEM-focused and difficulty-enhanced QA dataset. Mid-training Llama-3 8B on 40B tokens of BoostQA achieves a 12.74% average improvement on MMLU and CMMLU.<br/><span class="pub-keywords"><strong>Keywords:</strong> LLM · large-scale QA synthesis · diverse synthesis · mid-training</span>'
---

We propose a novel diversified pipeline that synthesizes BoostQA, a 100B-token, STEM-focused and difficulty-enhanced QA dataset. Mid-training Llama-3 8B on 40B tokens of BoostQA achieves a 12.74% average improvement on MMLU and CMMLU.

Motivation
------

Knowledge-intensive training data is scarce, and existing synthetic QA does not solve it. Most pipelines bind generated pairs to a source passage, so they merely restate the original corpus rather than enriching it. Open QA datasets concentrate on narrow domains — mathematics above all — and large cross-domain collections barely exist. More fundamentally, synthesis is undifferentiated: it generates more data without knowing where the model is actually weak.

We start instead by measuring the deficiencies. Probing checkpoints from 2T to 10T tokens across disciplines and difficulty tiers reveals two consistent gaps: accuracy is high in the humanities and low in STEM, and it degrades monotonically with difficulty — 60.74% at the easiest tier against 33.78% at the hardest on the 10T checkpoint. The synthesis pipeline is then built to target exactly these two weaknesses.

Method
------

**Diverse seed curation.** Seeds are drawn from heterogeneous sources — QA pairs, books, and quality web pages — rather than a single corpus. Decontamination against evaluation benchmarks uses exact 10-gram matching plus embedding similarity, applied to both seeds and synthetic output.

**Annotation system.** A discipline classifier sorts content into 62 first-level disciplines with 82.18% agreement against a much stronger reference model. A difficulty scorer calibrates five tiers from estimated pass rates of top-university students in the relevant major under one-hour conditions, validated at Krippendorff's α = 0.85 and Pearson r = 0.92 against expert judgment.

**Two-way synthesis.** The *multi-grade synthesizer* prompts DeepSeek-R1 with educational role prompts at high-school, college and graduate level, enforcing format rules and oversampling STEM seeds — this buys breadth. The *high-difficulty synthesizer* pins the role to graduate level and reweights hard seeds, requiring generated pairs to land in the top tiers — this buys depth, raising the hard-item share from 12.91% to 25.25%, a 1.96× increase.

A diagnostic motivated the second path: relabeling revealed systematic *downward drift* in educational stage. Asking for graduate-level content yields mostly college-level output (52.38%), and asking for college level yields mostly high-school (56.41%) — so prompts must aim a tier above the target.

**Refinement and blending.** DeepSeek-V3 screens solvability, discards unanswerable items and re-derives answers by stepwise reasoning. BoostQA is mixed 1:1 with KnowEdu, which anchors the distribution against forgetting and supplies the prose context that bare QA lacks.

Experiments
------

BoostQA is 100B tokens of general-domain QA (140B with CoT) — far larger than comparable synthetic sets such as MegaMathQA (7.0B) or JiuZhang3.0 (4.6B). Mid-training runs Llama-3 8B from its 2T checkpoint on 40B tokens, evaluated across 12 benchmarks against general corpora and five QA blends.

| | MMLU | CMMLU | MMLU-STEM | AVG (12) |
|---|---|---|---|---|
| Pre-training baseline | 55.08 | 52.23 | 45.17 | 43.12 |
| KnowEdu | 58.17 | 62.99 | 49.16 | 44.72 |
| Nemotron-CC | 58.62 | 62.02 | 49.25 | 44.91 |
| JiuZhang3.0 | 56.55 | 60.30 | 48.68 | 48.01 |
| **BoostQA** | **64.78** | **68.00** | **57.71** | **51.66** |

The **12.74% average gain on MMLU and CMMLU** is against the pretraining baseline, with +8.54% across 12 benchmarks and **+3.65% over JiuZhang3.0**, the strongest baseline. Scaling data to 190B tokens adds a further 5.76% on MMLU/CMMLU, and BoostQA mid-trained from the 2T checkpoint overtakes KnowEdu trained from 10T — synthesis quality substituting for pretraining compute. Refinement alone contributes 1.90%, changing 16.18% of answers.

Insights
------

- **Probe first, then synthesize.** Measuring where the model fails — by discipline and by difficulty — turns synthesis from bulk generation into targeted repair. The two synthesis paths map directly onto the two measured gaps.
- **Generators drift easier than instructed.** The systematic downward drift in educational stage means naive prompting silently produces data below the intended difficulty. Prompting one tier high is a simple but necessary correction.
- **Question format shapes which benchmarks improve.** Multiple-choice data helps MMLU through format alignment, while essay-style items matter for comprehension tasks like DROP — breadth of format matters alongside breadth of content.
- **Synthetic QA also lifts other corpora.** Adding BoostQA improves Nemotron-CC Document by 3.15% and FineWeb-Edu by 7.32%, suggesting complementarity rather than substitution.
- **A caution.** Every QA blend shows heightened loss volatility, which we attribute to the tension between dense QA and descriptive prose — one reason the KnowEdu anchor matters.

**Limitations.** Without CoT, BoostQA still trails JiuZhang3.0 on MATH. The 190B-token scaling curve had not plateaued, so the ceiling remains unmeasured.

A companion work, [LinkQA]({{ site.baseurl }}/publication/2026-08-01-linkqa), approaches the same scarcity problem through knowledge-point graphs and multi-seed synthesis.

[Read the paper](https://arxiv.org/abs/2508.01326) · [Code and data](https://github.com/ChasyT/BoostQA) · [ACL Anthology](https://aclanthology.org/2026.findings-acl.814/)

**Keywords:** LLM · large-scale QA synthesis · diverse synthesis · mid-training
