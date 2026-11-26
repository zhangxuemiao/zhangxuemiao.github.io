---
title: "Enhancing LLMs via High-Knowledge Data Selection"
collection: publications
category: conferences
permalink: /publication/2025-02-25-high-knowledge-data-selection
date: 2025-02-25
venue: 'AAAI 2025'
ccf: 'A'
paperurl: 'https://arxiv.org/abs/2505.14070'
authors: 'F. Duan*, <strong>Xuemiao Zhang</strong>* (* equal contribution), S. Wang, H. Que, Y. Liu, W. Rong, X. Cai'
excerpt: 'We propose a gradient-free High-Knowledge Scorer (HKS) that selects high-quality data along the knowledge dimension, addressing the scarcity of knowledge in pretraining corpora. HKS first builds a comprehensive multi-domain pool of knowledge elements and introduces knowledge density and coverage as metrics for the knowledge content of a text. Building on these, we design a composite knowledge scorer to select knowledge-intensive data. Experiments show that HKS improves model performance on both knowledge-intensive and general understanding tasks, strengthening general and domain-specific capabilities alike.<br/><span class="pub-keywords"><strong>Keywords:</strong> LLM · knowledge scoring of pretraining data · high-quality data selection</span>'
---

We propose a gradient-free High-Knowledge Scorer (HKS) that selects high-quality data along the knowledge dimension, addressing the scarcity of knowledge in pretraining corpora. HKS first builds a comprehensive multi-domain pool of knowledge elements and introduces knowledge density and coverage as metrics for the knowledge content of a text. Building on these, we design a composite knowledge scorer to select knowledge-intensive data. Experiments show that HKS improves model performance on both knowledge-intensive and general understanding tasks, strengthening general and domain-specific capabilities alike.

Motivation
------

Data selection research optimizes for quality, but quality filters reward fluent, well-formed prose — which can be thin on substance. A polished paragraph that states nothing and a dense encyclopedic passage look similar to a writing-style rater. Knowledge richness is a distinct axis that prior work largely ignores, leaving pretraining corpora knowledge-scarce in a way quality scoring cannot detect.

Method
------

**Knowledge element pool.** We simplify knowledge-graph triplets into *knowledge elements* — n-gram nouns naming a concept, fact, theory, principle or definition. Sources are Wikipedia entries and academic keywords (≈20M raw elements), augmented by GPT-4 extraction from Wikipedia documents. Elements are categorized into five domains — science, society, culture, art, life — by a classifier reaching 96.2% accuracy, then quality-filtered and deduplicated to a final pool of **5M elements**. Matching elements in documents uses the Aho-Corasick automaton for speed.

**Two metrics.** For a text x of length n_p: *knowledge density* d(x) = n_k / n_p counts how much knowledge per token, while *knowledge coverage* c(x) = ñ_k / N_k measures how many **distinct** elements appear relative to the pool — a diversity indicator. They behave very differently: the top million documents by density average 20,950 tokens, those by coverage only 811.

**Composite scorer.** Treating the metrics as independent, the score factorizes as f(d)·g(c) with both functions increasing and concave, encoding diminishing returns. We selected the functional form by correlating candidates against human pairwise preferences on 2K text pairs; **score(x) = d(x)·ln(c(x)+1)** won at Spearman 0.855. Selection then takes either the top-k documents or a softmax sample. Restricting the element pool to one domain yields a domain-specific variant.

The scorer is gradient-free and runs on CPU — far cheaper than perplexity- or gradient-based selection.

Experiments
------

Selection runs over 20B bilingual tokens (10B from The Pile, 10B from WuDao), training a 1.1B model, with continual pretraining of Llama-3-8B on 100B tokens for scale. Baselines span random, density-only, coverage-only, inverse HKS, perplexity, EL2N and DSIR. Evaluation covers 16 sub-tasks grouped by language and by knowledge-intensive versus general understanding.

| Method | EN knowledge | EN general | ZH knowledge | ZH general | AVG |
|---|---|---|---|---|---|
| Random | 23.77 | 47.56 | 25.15 | 28.66 | 32.49 |
| PPL | 24.44 | 45.92 | 24.46 | 22.99 | 29.41 |
| EL2N | 24.52 | 48.23 | 26.11 | 24.34 | 30.84 |
| DSIR | 19.26 | 48.12 | 25.61 | 23.97 | 29.75 |
| **HKS (top-k)** | **26.32** | **48.93** | **27.37** | **32.00** | **35.07** |

HKS wins every column, gaining **2.37 points on average over random**, while every competing selection method lands *below* random. At scale, Llama-3-8B continually pretrained on HKS-selected data gains +0.9 / +3.8 / +2.7 points on MMLU / CMMLU / C-Eval over random selection. Sweeping the high-knowledge token fraction shows performance declining monotonically as that fraction falls. The science-restricted variant improves targeted-domain scores by up to 2.5 points.

Insights
------

- **Knowledge is orthogonal to the usual quality signals.** HKS correlates weakly — trending negative — with perplexity, EL2N and DSIR, yet outperforms all of them. It is measuring something those methods do not see.
- **Density and coverage capture different things and both are needed.** Volume versus variety: the two metrics are near-uncorrelated, and either alone underperforms the composite.
- **Knowledge content appears to help reasoning, not just recall.** BBH improves along with the knowledge fraction, which is not what a purely factual-recall account would predict.
- **Perplexity is a poor proxy for downstream performance.** HKS is slightly worse than a coverage baseline on corpus perplexity while being clearly better on tasks.
- **Selection direction matters.** Inverse HKS falls below random, confirming the score tracks something real rather than merely reshaping the distribution.

**Limitations.** The 5M pool could be larger and more finely categorized, the scoring formula is empirically rather than theoretically justified, and hardware limited the main experiments to ≈1.1B parameters.

[Read the paper](https://arxiv.org/abs/2505.14070)

**Keywords:** LLM · knowledge scoring of pretraining data · high-quality data selection
