---
title: Project 4 Final Project
navbar: Guides
layout: guides
key: 4.0
bump: true

project: 4

tags:
 - text: 'New'
   type: 'is-primary'

tests: 'project4_tests'
review1: 'project4_review'
review0: 'project3_review1'
design0: 'project3_design'

---

For this project, you will extend your [previous project](project-{{ page.project | minus: 1 }}.html) to create a fully functional search engine. This is the **last project** for this course and is split into two main components:

  - **[Project 4a Web Crawler](project-4a.html)**: Use a work queue to build the index from a seed URL instead of text files. This component will graded using functionality tests only.

  - **[Project 4b Search Engine](project-4b.html)**: Create a search engine web application using Jetty and servlets. This component will be graded in one [final code review](final-review.html) during finals week (if eligible).

See the specific project writeups for mode details.

## Grading
{: .page-header }

<article class="message is-warning">
  <div class="message-body"><i class="far fa-exclamation-triangle"></i>&nbsp;The code review for project 4 are handled slightly differently than other projects. Pay close attention to the sections below!</div>
</article>

This project grade is split into the following assignments:

| Assignment | Points | Deadline | Release | Prerequisites |
|:-----------|-------:|---------:|:-------:|:--------------|
| [{{ site.data.projects[page.tests].text }}]({{ site.data.projects[page.tests].canvas }}) | {{ site.data.projects[page.tests].points }} | {{ site.data.projects[page.tests].date | date: "%b %d, %Y" }} | `v{{ page.project }}.0.Z` | `Project{{ page.project }}Test.java`, [{{ site.data.projects[page.review0].text }}]({{ site.data.projects[page.review1].canvas }}), [Test Checks](grading.html#project-tests) |
| [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}) (30 min) | {{ site.data.projects[page.review1].points }} | {{ site.data.projects[page.review1].date | date: "%b %d, %Y" }} | `v{{ page.project }}.1.Z` | [{{ site.data.projects[page.design0].text }}]({{ site.data.projects[page.design0].canvas }}), [{{ site.data.projects[page.tests].text }}]({{ site.data.projects[page.tests].canvas }}), [Review Checks](grading.html#project-reviews) |

See below for additional details.

### Final Code Review

Students must meet the requirements for the [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}) grade prior to their [final code review](final-review.html) appointment. Since the last normal code reviews are offered on **Friday, December 9th**, this means all project 3 code reviews must be completed by then. Students cannot have a project 3 and project 4 code review in their final code review appointment. See the [final code review](final-review.html) guide for details.

### Project Extra Credit

Students that are eligible for [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}) during finals week may complete additional [search engine](project-4b.html) functionality to earn extra credit in the project category. Students that are not eligible for this project may complete [other extra credit](extra-credit.html) opportunities instead at the end of the semester.

These extra credit opportunities can be completed to make up for points lost due to late penalties or other deductions in the projects category. These opportunities may not be used to earn over a 100% grade in this category. 