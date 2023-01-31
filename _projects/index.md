---
title: Projects
navbar: Resources
layout: resources
---

{%- assign pages = site.collections | where: 'label', page.collection | first -%}
{%- assign items = pages.docs | sort: 'key' -%}
{%- include resources.html items = items %}
