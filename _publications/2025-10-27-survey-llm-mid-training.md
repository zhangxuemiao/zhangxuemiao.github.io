---
title: "A Survey on LLM Mid-Training"
collection: publications
category: preprints
permalink: /publication/2025-10-27-survey-llm-mid-training
date: 2025-10-27
venue: 'arXiv:2510.23081, 2025'
paperurl: 'https://arxiv.org/abs/2510.23081'
authors: 'C. Tu*, <strong>Xuemiao Zhang</strong>* (* equal contribution), R. Weng, R. Li, C. Zhang, Y. Bai, H. Yan, J. Wang, X. Cai'
excerpt: 'A systematic survey of mid-training as a distinct stage between pretraining and post-training, covering its objectives, data strategies, training recipes, and evaluation, and organizing the design choices that make mid-training an effective way to strengthen foundation models.<br/><span class="pub-keywords"><strong>Keywords:</strong> LLM · mid-training · survey</span>'
---

A systematic survey of mid-training as a distinct stage between pretraining and post-training, covering its objectives, data strategies, training recipes, and evaluation, and organizing the design choices that make mid-training an effective way to strengthen foundation models.

Motivation
------

Progress in foundation models increasingly comes from multi-stage training, and a stage has emerged between pretraining and post-training that the literature had not named consistently or defined precisely. Practitioners were converging on similar techniques — targeted capability injection on intermediate data budgets — without a shared vocabulary for what they were doing or why it works. This survey gives mid-training a formal definition and organizes the scattered practice into a taxonomy.

Scope and organization
------

What distinguishes mid-training is its use of **intermediate data and computational resources**: it is an intermediate regime rather than a different kind of learning. Functionally, it systematically enhances specified capabilities — mathematics, coding, reasoning, long-context extension — while maintaining the foundational competencies established during pretraining. That dual requirement, targeted gain without general degradation, is what makes it a design problem in its own right.

The survey reviews optimization frameworks along three components:

- **Data curation** — how corpora are selected, synthesized and scheduled for the target capability.
- **Training strategies** — how the stage is scheduled, weighted and blended against general data.
- **Model architecture optimization** — structural choices that interact with the stage.

It then analyzes mainstream model implementations as *objective-driven interventions*, grouping deployed practice by the capability each intervention targets rather than by the lab or model family that produced it.

Takeaways
------

The central claim is that mid-training is a distinct and critical stage in the progressive development of LLM capabilities, not an extension of pretraining with a different learning rate. Treating it as its own design space — with its own data strategies, evaluation criteria and failure modes — makes the choices legible and transferable across model families.

Several papers in this line of work are surveyed as instances of the data-curation axis, including [LinkQA]({{ site.baseurl }}/publication/2026-08-01-linkqa) and [BoostQA]({{ site.baseurl }}/publication/2026-08-02-large-scale-diverse-synthesis) on synthesis, and [CoTP]({{ site.baseurl }}/publication/2026-04-24-expanding-reasoning-potential) on reasoning-data selection.

[Read the paper](https://arxiv.org/abs/2510.23081)

**Keywords:** LLM · mid-training · survey
