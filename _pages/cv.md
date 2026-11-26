---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
<!-- TODO: 请补全起止年份与专业名称 -->
* Ph.D., Peking University, 20XX–20XX
* B.S., 20XX–20XX

Experience
======
<!-- TODO: 请补全起止年份 -->
* 20XX–present: Pre-Training Researcher, **dots.llm** Team, Xiaohongshu (RedNote)
  * Large language model pre-training and mid-training
  * Data selection, data quality, and large-scale data synthesis

* 20XX–20XX: Pre-Training Researcher, **LongCat** LLM Team, Meituan
  * Contributed to the LongCat-Flash series of foundation models

Research Interests
======
* Large language models: pre-training and mid-training
* Data selection, data quality assessment, and large-scale data synthesis
* Reasoning in foundation models

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
