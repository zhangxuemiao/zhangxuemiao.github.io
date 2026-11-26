---
title: "Expanding Reasoning Potential in Foundation Models by Learning Diverse Chains of Thought Patterns"
collection: publications
category: conferences
permalink: /publication/2026-04-24-expanding-reasoning-potential
date: 2026-04-24
venue: 'ICLR 2026'
ccf: 'A'
paperurl: 'https://arxiv.org/abs/2509.21124'
authors: '<strong>Xuemiao Zhang</strong>, C. Ren, C. Tu, R. Weng, S. Wang, H. Yan, J. Wang, X. Cai'
excerpt: 'This work is the first to define the reasoning potential of a base LLM as the reciprocal of the number of independent attempts needed to answer a question correctly, and proposes expanding that potential with diverse data rich in high-value reasoning patterns. We abstract atomic reasoning patterns — ones that generalize and support induction — from CoT sequences, and use them to construct a core reference set rich in valuable patterns. We then introduce a dual-granularity matching algorithm based on reasoning-pattern chains and token entropy that effectively selects high-value CoT data (CoTP) from a data pool matched against the core set. Using only 10B tokens of CoTP, an MoE-85A6B model improves by 9.58% on the challenging AIME 2024/2025 benchmarks and raises the ceiling of downstream RL performance by 7.81%.<br/><span class="pub-keywords"><strong>Keywords:</strong> LLM · reasoning potential of base models · reasoning pattern mining · high-value reasoning data</span>'
---

This work is the first to define the reasoning potential of a base LLM as the reciprocal of the number of independent attempts needed to answer a question correctly, and proposes expanding that potential with diverse data rich in high-value reasoning patterns. We abstract atomic reasoning patterns — ones that generalize and support induction — from CoT sequences, and use them to construct a core reference set rich in valuable patterns. We then introduce a dual-granularity matching algorithm based on reasoning-pattern chains and token entropy that effectively selects high-value CoT data (CoTP) from a data pool matched against the core set. Using only 10B tokens of CoTP, an MoE-85A6B model improves by 9.58% on the challenging AIME 2024/2025 benchmarks and raises the ceiling of downstream RL performance by 7.81%.

Motivation
------

Reinforcement learning is now the standard way to elicit reasoning from foundation models, but RL largely amplifies what the base model can already do — it cannot create capability that is absent before training starts. That makes the *reasoning potential* of the base model the real bottleneck, yet the field lacked a way to measure it or to deliberately expand it during mid-training.

We give the notion a formal footing. Define model potential Φ(ℳ, qᵢ) as the probability that the model produces a correct answer when sampling from its output distribution. If each attempt is an independent Bernoulli trial, the number of attempts Kᵢ needed to solve question qᵢ is geometrically distributed, so Φ(ℳ, qᵢ) = 1/𝔼[Kᵢ]. **Reasoning potential is the inverse of the expected number of independent attempts required to answer correctly** — and expanding it means reducing that expectation. This reframes data selection as an optimization problem: choose M samples from a source pool that minimize the potential gap against a model trained on an unknowable oracle set.

A negative result sharpens the problem. Naively pooling long-CoT corpora (OpenR1-Math, AM-DeepSeek-R1-Distilled, and hard BoostQA questions) performs *worse* on average than its individual constituents — simply mixing more reasoning data is not enough.

Method
------

**Atomic reasoning patterns.** A reasoning pattern is an atomic cognitive operation — a fundamental reasoning step that transfers across problem domains. Each CoT trace reduces to an ordered *pattern chain*. We prompt DeepSeek-V3 to annotate traces with their chains, performing annotation in Chinese because each character carries semantic content, which sharpens character-level n-gram distinctions between unrelated patterns.

**Core reference set.** Questions are filtered by difficulty and type to match a target distribution, with correctness established by majority vote across several strong reasoning models. For each question we generate r traces and score pattern importance by TF-IDF over pattern occurrences. Human curation then selects traces carrying distinctive, high-importance patterns, yielding a core set with per-instance importance weights.

**Dual-granularity matching.** Every CoT is represented at two granularities: its pattern chain (the abstract reasoning paradigm) and its *entropy chain* — per-token entropy hₜ = −Σ pₜ(v) log pₜ(v), which localizes the tokens carrying reasoning gain. Distance combines the two, D = λ·d_pattern + (1−λ)·d_entropy, each computed by weighted Dynamic Time Warping: the pattern-chain DTW uses importance weights with a character n-gram cosine distance, the entropy-chain DTW uses absolute differences.

Selection is then a capacity-constrained assignment problem — minimize total matching distance subject to each core instance receiving exactly *o* source instances and each source used at most once. Replicating core instances *o* times reduces this to a standard linear assignment, solved exactly by the Hungarian algorithm; we prove the equivalence to minimum-weight perfect bipartite matching. The formulation is domain-agnostic for any task decomposable into atomic patterns.

Experiments
------

An 85A6B Mixture-of-Experts model pre-trained on 14T tokens is mid-trained on 30B tokens, blending reasoning data with general-domain KnowEdu at 1:2 (≈10B reasoning tokens). Every model then receives an identical 60k long-CoT SFT set, which matters for fairness: without it, models lacking long-CoT ability are penalized for format rather than reasoning. RL uses GSPO for 300 steps. Evaluation spans AIME 2024/2025, HMMT 2025, BeyondAIME and MATH500, with general ability tracked on MMLU, CMMLU, C-Eval and others.

| Mid-training data | AIME25 | AIME24 | HMMT25 | MATH500 | AVG |
|---|---|---|---|---|---|
| KnowEdu | 0.33 | 1.22 | 5.10 | 45.80 | 10.49 |
| OpenR1-Math | 23.96 | 29.69 | 16.04 | 87.80 | 33.32 |
| AM-Distilled | 23.12 | 25.52 | 18.02 | 87.20 | 32.43 |
| LongCoTPool | 21.89 | 24.90 | 15.63 | 85.40 | 31.14 |
| **CoTP** | **28.02** | **37.92** | **20.73** | **90.80** | **37.53** |

CoTP gains 6.39% on average over the pooled baseline, including **9.58% across AIME 2024/2025**. Crucially the advantage survives RL rather than being consumed by it: post-RL average reaches 51.44 against 43.63 for the pool — the **7.81% lift in the RL ceiling**. Scaling mid-training to 60B tokens continues to improve results. Ablations confirm every component contributes, with the n-gram granularity mattering most.

Insights
------

- **Pattern coverage explains the gain.** Using DeepSeek-R1 as a proxy ideal model on AIME, CoTP produces 3,226 correct patterns and 1,815 key patterns (27.98% overlap with R1's key patterns), against 2,246 and 1,345 (20.73%) for the pool — a 7.25-point gain in overlap.
- **The model converges toward the reference paradigm.** Pattern-chain DTW distance to the core set drops from 0.51 before mid-training to 0.35 after, a 31.4% reduction; problem-type KL divergence falls from 0.18 to 0.04.
- **Reasoning and reflection are entangled.** Effective patterns not only guide CoT steps but also trigger introspection, appearing either inside discrete patterns or as recursive loops that signal iterative refinement.
- **Mid-training gains transfer to RL.** pass@k rises steadily with both k and RL steps, and truncation rates stay low and stable under RL (≈4–6% for CoTP versus ≈45% for the general-domain baseline), suggesting better-conditioned RL dynamics.

[Read the paper](https://arxiv.org/abs/2509.21124) · [Code and data](https://github.com/rc314159-creator/CoTP)

**Keywords:** LLM · reasoning potential of base models · reasoning pattern mining · high-value reasoning data
