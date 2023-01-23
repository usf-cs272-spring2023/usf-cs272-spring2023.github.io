---
title: Weekly Schedule
navbar: Schedule
---

The following is the current weekly schedule. This schedule is subject to change and will be frequently updated throughout the semester. The latest deadlines may also be found on [Canvas]({{ site.data.info.links.canvas.href }}).

<!-- quick navigation -->
<div class="buttons has-addons is-centered">
<a class="button is-small is-outlined" disabled>Weeks:</a>
{%- for week in site.data.schedule.weeks %}
<a class="button is-small is-link is-outlined" href="#{{ week.week | slugify }}">{{ week.week | split: " " | last }}</a>
{%- endfor %}
</div>

<!-- schedule -->
{%- for week in site.data.schedule.weeks %}
{% include week.html week = week %}
{% endfor -%}

<!-- icon legend -->
<div class="buttons is-centered are-small">
  {%- for icon in site.data.icons %}
  {%- if icon[1].show %}
  <button class="button is-static has-text-weight-light">
    <i class="{{ icon[1].type }} pr-1"></i> {{ icon[1].text }}
  </button>
  {% endif -%}
  {% endfor -%}
</div>