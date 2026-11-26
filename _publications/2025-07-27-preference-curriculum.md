---
title: "Preference Curriculum: LLMs Should Always Be Pretrained on Their Preferred Data"
collection: publications
category: conferences
permalink: /publication/2025-07-27-preference-curriculum
date: 2025-07-27
venue: 'ACL 2025'
ccf: 'A'
authors: '<strong>Xuemiao Zhang</strong>, L. Xu, F. Duan, Y. Zhou, S. Wang, R. Weng, J. Wang, X. Cai'
excerpt: 'We propose PDPC, a preference curriculum framework built on perplexity difference (PD) that continuously perceives and trains on the data LLMs prefer. PD quantifies how differently a weak and a strong model perceive the difficulty of a sample: high-PD samples are harder for the weak model and are better placed in later stages of pretraining. We further propose a preference function that approximates and predicts a model''s data preference at any training step, so the dataset can be arranged offline and training proceeds without interruption. On 1.3B and 3B models, PDPC significantly outperforms baselines — the 3B model trained on 1T tokens gains over 8.1% average accuracy on MMLU and CMMLU.<br/><span class="pub-keywords"><strong>Keywords:</strong> LLM · curriculum learning · pretraining data strategy · model data preference</span>'
---

We propose PDPC, a preference curriculum framework built on perplexity difference (PD) that continuously perceives and trains on the data LLMs prefer. PD quantifies how differently a weak and a strong model perceive the difficulty of a sample: high-PD samples are harder for the weak model and are better placed in later stages of pretraining. We further propose a preference function that approximates and predicts a model's data preference at any training step, so the dataset can be arranged offline and training proceeds without interruption. On 1.3B and 3B models, PDPC significantly outperforms baselines — the 3B model trained on 1T tokens gains over 8.1% average accuracy on MMLU and CMMLU.

**Keywords:** LLM · curriculum learning · pretraining data strategy · model data preference
