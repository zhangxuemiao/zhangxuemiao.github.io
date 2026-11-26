---
title: "Preference Curriculum: LLMs Should Always Be Pretrained on Their Preferred Data"
collection: publications
category: conferences
permalink: /publication/2025-07-27-preference-curriculum
date: 2025-07-27
venue: 'ACL 2025'
ccf: 'A'
paperurl: 'https://arxiv.org/abs/2501.13126'
authors: '<strong>Xuemiao Zhang</strong>, L. Xu, F. Duan, Y. Zhou, S. Wang, R. Weng, J. Wang, X. Cai'
excerpt: 'We propose PDPC, a preference curriculum framework built on perplexity difference (PD) that continuously perceives and trains on the data LLMs prefer. PD quantifies how differently a weak and a strong model perceive the difficulty of a sample: high-PD samples are harder for the weak model and are better placed in later stages of pretraining. We further propose a preference function that approximates and predicts a model''s data preference at any training step, so the dataset can be arranged offline and training proceeds without interruption. On 1.3B and 3B models, PDPC significantly outperforms baselines — the 3B model trained on 1T tokens gains over 8.1% average accuracy on MMLU and CMMLU.<br/><span class="pub-keywords"><strong>Keywords:</strong> LLM · curriculum learning · pretraining data strategy · model data preference</span>'
---

We propose PDPC, a preference curriculum framework built on perplexity difference (PD) that continuously perceives and trains on the data LLMs prefer. PD quantifies how differently a weak and a strong model perceive the difficulty of a sample: high-PD samples are harder for the weak model and are better placed in later stages of pretraining. We further propose a preference function that approximates and predicts a model's data preference at any training step, so the dataset can be arranged offline and training proceeds without interruption. On 1.3B and 3B models, PDPC significantly outperforms baselines — the 3B model trained on 1T tokens gains over 8.1% average accuracy on MMLU and CMMLU.

Motivation
------

Standard pretraining draws from one fixed data distribution from the first token to the last. That is at odds with how capability develops: as a model grows stronger, what counts as usefully difficult data shifts. Data that teaches a weak model little may be exactly what a stronger one needs, and vice versa. If preference is dynamic, the data schedule should be too.

The practical obstacle is that measuring a model's current preference means interrupting training to re-score the corpus — expensive, and invasive to the training loop. We wanted a curriculum that captures the dynamics while remaining fully precomputable.

Method
------

**The PD metric.** Perplexity difference compares how hard a sample is for a weak model against a strong one. A low-PD sample is learned comparably by both; a high-PD sample is one the weak model struggles with while the strong model handles it. PD is therefore a *model-aware* difficulty signal, rather than a human-intuited proxy such as text length, rare-word count, or syntactic complexity — the features a model finds hard are not necessarily the ones a person would name.

**Easy-to-hard ordering.** Because high-PD data is what distinguishes a strong model from a weak one, it belongs later in training, once the model has the capacity to extract value from it. Low-PD data, learned equally well at any capability level, is better spent early.

**The preference function.** Rather than measuring preference online, we approximate and predict what the model will prefer at an arbitrary training step. This is the key engineering move: the entire dataset ordering is computed **offline**, before training begins, so the run proceeds without pauses, re-scoring passes, or instrumentation of the training loop. The curriculum costs nothing at training time.

Experiments
------

PDPC is validated at 1.3B and 3B parameters, with the 3B model trained on 1T tokens and evaluated on MMLU and CMMLU among other benchmarks. The method significantly surpasses baselines at both scales; the headline result is an **average accuracy gain of over 8.1% on MMLU and CMMLU** for the 3B model.

Insights
------

- **Data preference is dynamic, not static.** The common practice of holding the data distribution fixed throughout pretraining leaves measurable performance on the table.
- **The gap between two models is a usable difficulty signal.** Perplexity difference between a weak and a strong model proxies sample difficulty relative to current capability — something absolute perplexity cannot express.
- **Curricula need not complicate training.** Because preference is predicted rather than observed, the schedule is precomputed and the training loop stays untouched. The gains come without the engineering cost that usually makes curriculum learning unattractive at scale.

This line of work continues in [FRAME]({{ site.baseurl }}/publication/2025-07-28-frame), which combines PD with perplexity to define a four-quadrant multi-stage schedule.

[Read the paper](https://arxiv.org/abs/2501.13126) · [ACL Anthology](https://aclanthology.org/2025.findings-acl.1091/)

**Keywords:** LLM · curriculum learning · pretraining data strategy · model data preference
