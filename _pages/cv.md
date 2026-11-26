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
<!-- TODO: 请补全起止年份；本科院校与专业待补 -->
* Ph.D. in Software Engineering (工学博士), Peking University, 20XX–20XX
* B.S., 20XX–20XX

Experience
======
* Nov 2025–present: Pre-Training Researcher, **dots.llm** Team, Xiaohongshu (RedNote)
  * Large language model pre-training and mid-training
  * Data selection, data quality, and large-scale data synthesis

* Jul 2023–Nov 2025: Pre-Training Researcher, **LongCat** LLM Team, Meituan
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
