---
layout: resources
navbar: Resources
title: All Homework
key: 4
bump: true
---

Here is a table of all the homework assignments for this semester:

<table class="table is-hoverable is-auto">
<thead>
	<tr>
		<th>Homework Name</th>
		<th>Canvas Link</th>
		<th>Github Link</th>
		<th>Deadline</th>
	</tr>
</thead>

<tbody>

{% for item in site.data.homework %}
	<tr>
		<td><code>{{ item[1].text }}</code></td>
		<td><a href="{{ item[1].href }}">Assignment</a></td>
		<td><a href="{{ site.data.info.links.github.href }}/{{ item[1].repo }}">Template</a></td>
		<td>{{ item[1].date | date: "%a, %b %d, %Y" }}</td>
	</tr>
{% endfor -%}

</tbody>
</table>
