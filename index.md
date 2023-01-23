---
title: Welcome
navbar: Home
---

Welcome to <strong class="has-text-usf-green">{{ site.data.info.code }} {{ site.data.info.name }}</strong> for <strong class="has-text-usf-green">{{ site.data.info.term }}</strong>. {{ site.description }}

This course will use the **hybrid modality** for the {{ site.data.info.term }} semester. Tuesday lectures will be in-person, but Thursday lectures will utilize a mix of synchronous and asynchronous remote modalities. See the [course syllabus]({{ site.data.info.links.syllabus.href }}) for details.

[Course Syllabus]({{ site.data.info.links.syllabus.href }}){: .button .is-primary }
[Suggestion Box]({{ site.data.info.links.suggestions.href }}){: .button .is-primary }

## Upcoming Schedule

Here is the upcoming course schedule, which includes links to lecture material, assigned quizzes and homework, and more:

{%- assign today_date = 'now' | date: '%Y-%m-%d' -%}
{%- assign today = today_date | date: '%s'| abs -%}
{%- assign beg_date = '2023-01-23' | date: '%s' | abs -%}
{%- assign beg_index = 0 -%}

{%- if today > beg_date -%}
  {%- assign end_index = site.data.schedule.weeks | size | minus:1 -%}
  {%- for week in site.data.schedule.weeks limit:end_index -%}
    {%- if week.days -%}
      {%- assign as_seconds = week.days[0].date | date: '%s' | abs -%}
      {%- assign as_seconds = as_seconds | minus: 86400 -%}
      {%- assign as_seconds = as_seconds | minus: 86400 -%}
      {%- if as_seconds > today -%}
        {%- break -%}
      {%- endif -%}
    {%- endif -%}
    {%- assign beg_index = forloop.index0 -%}
  {%- endfor -%}
{%- endif -%}

{% for week in site.data.schedule.weeks offset:beg_index limit:2 %}
{% include week.html week = week %}
{% endfor %}

[View Full Schedule]({{ site.data.info.links.schedule.href }}){: .button .is-primary }

## Navigation

This website serves as the main portal for all content related to this course. This includes the following:

<ul class="fa-ul ml-6">

{%- assign data = site.data.info.links %}
{%- for property in data %}
{%- assign name = property[0] %}
{%- assign item = property[1] %}

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