---
title: Welcome
navbar: Home
---

Welcome to <strong class="has-text-usf-green">{{ site.data.info.code }} {{ site.data.info.name }}</strong> for <strong class="has-text-usf-green">{{ site.data.info.term }}</strong>. {{ site.description }}

This course will use the **hybrid modality** for the {{ site.data.info.term }} semester. Tuesday lectures will be in-person, but Thursday lectures will utilize a mix of synchronous and asynchronous remote modalities. See the [course syllabus]({{ site.data.info.links.syllabus.href }}) for details.

[Course Syllabus]({{ site.data.info.links.syllabus.href }}){: .button .is-primary }
[Suggestion Box]({{ site.data.info.links.suggestions.href }}){: .button .is-primary }

## Navigation

This website serves as the main portal for all content related to this course. This includes the following:

<ul class="fa-ul ml-6">

{%- assign data = site.data.info.links %}
{%- for values in data %}
{%- assign label = values[0] %}
{%- assign item = data[label] %}

{%- if forloop.first %}
{%- assign type = item.type %}
{%- endif %}

<li {% if type != item.type %}class="mt-3"{% endif %}>
  {%- if item.icon %}
  <span class="fa-li"><i class="{{ item.icon }}"></i></span>
  {%- endif %}
  <a href="{{ item.href | relative_url }}">{{ item.text }}</a>: {{ item.info }}
</li>

{%- assign type = item.type %}
{%- endfor %}
</ul>

When in doubt, post on [course forums]({{ site.data.info.links.forums.link }}) for help finding content.