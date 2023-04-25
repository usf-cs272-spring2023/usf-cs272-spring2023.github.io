---
title: Project 5 Server
navbar: Resources
layout: resources
key: 5.0
bump: false

project: 5

tags:
 - text: 'Pending'
#    type: 'is-primary'

subsections:
  - text: 'Project Release v5.0'
    link: '#project-release-v50'

  - text: 'Core Functionality'
    link: '#core-functionality'

  - text: 'Extra Functionality'
    link: '#extra-functionality'

  - text: 'Design Deductions'
    link: '#design-deductions'

---

For this project, you will extend your [previous project](project-{{ page.project | minus: 1 }}.html) to create a fully functional multithreaded search engine with a web interface that allows users to enter search queries into an HTML form and get back search results as a dynamically generated web page using embedded Jetty and servlets. 

This project will be graded and reviewed during [finals week](final-review.html) only. If you do not attend a final code review, you will earn a 0 on these assignments regardless of what you implemented.
{: .notification .is-danger .is-light }

## Project Release v{{ page.project }}.0
{: .page-header }

This project release must support building an inverted index from a single web page as follows:

  - Your `main` method must process the command-line arguments `-server [port]` where the flag `-server` indicates to launch a multithreaded search engine web server, and the next *optional* argument `[port]` is the port the web server should use to accept socket connections. Use `8080` as the default value if it is not provided.

  - If the `-server` command-line argument is provided, your code should **enable multithreading** with the default number of worker threads even if the `-threads` flag is not provided. 
  
  - Your web server must use **Jetty and servlets** to provide a web interface for searching the inverted index. This core functionality is worth 50 points. See the [Core Functionality](#core-functionality) section for details.

  - After implementing core functionality, your web server must provide up to 50 points of extra functionality of your choosing. See the [Extra Functionality](#extra-functionality) section for details.

Your code should support all of the functionality from the [previous project](project-{{ page.project | minus: 1 }}.html) as well.

{% assign release = "tests_v50" %}
{% assign prereqs = "design_v3x,tests_v41" %}

### {{ site.data.projects[release].text }}

{% include project_tags.html release=release prereqs=prereqs %}

This grade is earned by meeting the following requirements:

  1. You must already have a passing grade for the [{{ site.data.projects["design_v3x"].text }}]({{ site.data.projects["design_v3x"].book }}) and [{{ site.data.projects["tests_v41"].text }}]({{ site.data.projects["tests_v41"].book }}) assignments.

  1. You must create a release that continues to pass the [{{ site.data.projects["tests_v41"].text }}]({{ site.data.projects["tests_v41"].book }}) and earlier tests.

  1. You must attend a [final code review](final-review.html) with the instructor where your core and extra functionality will be manually user tested. Core functionality must pass before extra functionality will be graded and tested.
  
This project is manually graded and reviewed during your [finals review](final-review.html) only. You can earn a **partial grade** for this assignment (unlike other test assignments).

There are no automated tests for this release. Do **NOT** use the "Request Project Tests Grade" issue template to request a grade.
{: .notification .is-danger .is-light }

{% assign release = "design_v50" %}
{% assign prereqs = "design_v3x,tests_v41,tests_v50" %}

### {{ site.data.projects[release].text }}

This grade is earned by meeting the following requirements:

  1. You earn a passing grade for the [{{ site.data.projects["tests_v50"].text }}]({{ site.data.projects["design_v3x"].book }}) assignment during your [final review](final-review.html).

  1. You have well-designed project code, including all code created for previous projects. See the [Design Deductions](#design-deductions) section for details.

This project will be graded and reviewed during [finals week](final-review.html) only.

### Run Examples

The following are a few examples (non-comprehensive) to illustrate the usage of the command-line arguments that can be passed to your `Driver` class via a "Run Configuration" in Eclipse, assuming you set the working directory to the `project-tests` directory.

Consider the following example:

```
-html "{{ site.github.url }}/project-web/input/simple/" -crawl 15 -threads 3 -server 8080
```

The above arguments behave the same as [project {{ page.project | minus: 1 }}](project-{{ page.project | minus: 1 }}.html), except it will also start up a web server on port `8080` for the user to interface with the search engine. No file output will be generated in this example.

## Core Functionality
{: .page-header }

The functionality for this project is broken into 2 parts: core functionality (50 points) and extra functionality (50 points). Together, these make up the {{ site.data.projects[tests_v50].text }} grade. **You must complete the core functionality before extra features.** 

The core functionality includes, in addition to maintaining the functionality of the [previous project](project-4.html), the following for a total of 50 points:

| Points | Functionality Description |
|:------:|:--------------------------|
| 10 | **Web Form:** Display a web page with a form that includes (at a minimum) a text box where users may enter a multi-word search query and a button to submit that query to the web server. |
| 10 | **Query Processing:** When the web form button is clicked, send the queries to a Jetty servlet and process those queries to match how the data is stored by your inverted index. |
| 10 | **Partial Search:** After processing the queries, the servlet should retrieve the partial search results of those queries from the index generated by the `Driver` class. The servlet implementation should be thread-safe and multi-user friendly. |
| 20 | **Search Results:** The servlet should return the partial search results to the client (or web browser) as dynamically generated HTML with sorted (most relevant first) and *clickable* hyperlinks. |
{: .table .is-hoverable }

You cannot earn credit for extra functionality until the core functionality is working properly.

## Extra Functionality
{: .page-header }

Once the core functionality is complete, you may implement up to 50 points of extra features. You must successfully implement *at least* 26 points to earn a passing grade on this assignment if you received full credit for core functionality.

These features are broken into several categories. You may choose any combination of features from these categories.

Have a feature idea? You can propose an extra feature in a public post on the course forums. If approved, the instructor will post the number of points that feature will be worth on the final project.
{: .notification .is-info .is-light }

### User Tracking Features

The following features requires your search engine to track user data. There are two implementation options (choose one):

  1. **Base Functionality:** Implemented by storing data in memory; only supports a single user.

  2. **Extra Functionality:** Implemented by storing data using session tracking or cookies; supports multiple users.

Ideally, you should use the same implementation option for all features in this subcategory. For example, if you implement search history using sessions, you should also implement visited results using sessions.

The possible features are:

| Base | Extra | Description |
|:----:|:-----:|:--------------------------|
| 10 | 15 | **Search History:** Store a history of all search queries. Allow users to view and clear that history. |
| 10 | 15 | **Visited Results:** Store a history of all visited search results (i.e. results clicked on). Allow users to view and clear that history. |
| 10 | 15 | **Favorite Results:** Allow users to save favorite search results. Allow users to view and clear those favorites. |
| 10 | 15 | **Time Stamps:** Add timestamps to each item stored. Implement this for all related features to earn full credit. |
| 10 | 15 | **Private Search:** Allow users to set an option that turns off all tracking of user data. Implement this for all related features to earn full credit. |
| 10 | 15 | **Last Visit Time:** Track and display the last time a user visited your search engine. This is NOT the current time that the page was generated! |
{: .table .is-hoverable }

There are 60 to 90 points possible in this category depending if you choose to implement base or extra functionality.

### Metadata Features

The following features requires your search engine to track search metadata (not specific to users). There are two implementation options:

  1. **Base Functionality:** Track metadata in memory (non-persistent).

  2. **Extra Functionality:** Track metadata in the on-campus SQL database (persistent).

Ideally, you should use the same implementation option for all features in this subcategory. For example, if you implement page snippets using a database, you should also implement popular queries using a database too.

The possible features are:

| Base | Extra | Description |
|:----:|:-----:|:--------------------------|
| 15 | 30 | **Page Snippets:** When a web page is crawled, store a short snippets of the page. Display the snippet whenever that page is returned as a result. |
| 15 | 30 | **Page Statistics:** When a web page is crawled, store the page title (via the `<title>` tag in HTML), content length (via the `Content-Length` HTTP header), and timestamp of the crawl. Display these statistics whenever that page is returned as a result. |
| 10 | 20 | **Most Visited Results:** Track the number of times a page has been visited by any user. Allow users to see the top 5 visited pages. |
| 10 | 20 | **Most Searched Queries:** Track the number of times a multi-word query has been searched for. Allow users to see the top 5 most popular queries. |
| 10 | 20 | **Reset Metadata:** Allow users with an administrator password to clear all the metadata stored. |
{: .table .is-hoverable }

Some features require others to be implemented first. For example, **Reset Metadata** cannot be implemented until at least one of the other features that stores metadata is implemented.

There are 60 to 120 points possible in this category depending if you choose to implement base or extra functionality.

### Extendable Features

The following features have base functionality that can be extended with additional functionality. The base functionality must be implemented first.

| Base | Extra | Base Functionality | Extended Functionality |
|:----:|:-----:|:-------------------|:-----------------------|
| 10 | 20 | **New Seed:** Allow a user to specify a new seed URL that should be added to the existing inverted index. If the URL has already been crawled, skip crawling that URL and output a warning to the user. | **Max Crawl Support:** In addition to entering a new seed URL, allow the user to also specify a maximum number of pages to crawl. This is the maximum number of new pages to crawl *in addition to* the pages already crawled. URLs that are already included in the inverted index should be skipped and should not contribute to this maximum count. |
| 10 | 20 | **Index Browser:** Allow users to browse your inverted index as an HTML page with all of the words stored, clickable links to all of the indexed URLs for those words, and the number of positions stored for that word and location (but not list all of the positions). | **Subindex Browser:** Allow users to enter a specific word and display the data stored in your inverted index for that specific word. |
| 10 | 20 | **Location Browser:** Allow users to browse all of the locations and their word counts stored by your inverted index as an HTML page with clickable links to all of the indexed URLs. | **Partial Location Search:** Allow users to browse all of the locations and their word counts for locations that start with the same text. For example, browse all locations that start with "https://www.cs.usfca.edu/~cs272". |
| 10 | 20 | **Index JSON File:** Allow users to download a JSON file of your inverted index by browsing to a specific endpoint on your web server. For example, if users visit "/download", it returns an `index.json` file they can download to their system. | **Alternative Format:** Allow users to download a file in another structured standardized file format (XML, YAML, etc.) by browsing to a specific endpoint on your web server. For example, if users visit "/download?file=index&type=yaml", it returns an `index.yaml` file they can download to their system. |
{: .table .is-hoverable }

For example, if you implement base functionality for **New Seed**, you will earn 10 points. If you implement both base and extra functionality for **New Seed** (including **Max Crawl Support**), you will earn 20 points instead.

There are 40 to 80 points possible in this category depending if you choose to implement base or extra functionality.

#### Miscellaneous Features

The following miscellaneous features may also be implemented:

| Points | Description |
|:------:|:--------------------------|
| 10 | **Graceful Shutdown:** Allow an administrator to trigger a graceful shutdown of your search engine without calling `System.exit()``. |
| 10 | **Search Statistics:** Display the total number of results along with the time it took to calculate and fetch those results, and display the score and number of matches per search result listed. |
| 10 | **Server Statistics:** As a footer on every page, display the server uptime (i.e. time since the server was started), total number of words stored, total number of locations stored, and total number of queries conducted. This information can be stored in memory by the server. |
| 10 | **Quick Search:** Add a new button to your search form (in addition to the normal search button) that automatically redirects the user to the first search result instead of listing all of the search results. This is similar to the Google Search "I'm Feeling Lucky" button. Output a warning if there are no search results. |
| 10 | **Reverse Sort Order:** Allow the user to select an option to reverse the sort order of the search results using a checkbox on the search form. |
| 10 | **Partial/Exact Search Toggle:** Allow the user to toggle on/off partial versus exact search using a checkbox on the search form. |
| 10 | **Web Framework:** Design a search engine using any popular CSS/style framework to create a consistent style for all the web pages. For example, consider using [Bulma](https://bulma.io/), [Bootstrap (Twitter)](https://getbootstrap.com/), [Pure.css](https://purecss.io/), [Material (Google)](https://material.io/develop/web/), [Semantic UI](https://semantic-ui.com/), and many more. |
| 10 | **Search Brand:** Design a search engine with a distinct brand, logo, and tagline. This includes creating a *locally-hosted logo image* and including it (along with your tagline) on all of the web pages. **Do not use unlicensed unattributed media on your website.**{: .has-text-danger } |
| 10 | **Light/Dark Mode Toggle:** Allow users to toggle between light mode (light colored background with dark text) and dark mode (dark colored background with light text) styles for your website. |
{: .table .is-hoverable }

There are 90 points possible in this category.

## Design Deductions
{: .page-header }

If you earn a passing grade on the [{{ site.data.projects["tests_v50"].text }}]({{ site.data.projects["design_v3x"].book }}) assignment during your [final review](final-review.html), then your code will be evaluated for its design.


PENDING


{% comment %}


### Final Code Review

Students must meet the requirements for the [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}) grade prior to their [final code review](final-review.html) appointment. Since the last normal code reviews are offered on **Friday, December 9th**, this means all project 3 code reviews must be completed by then. Students cannot have a project 3 and project 4 code review in their final code review appointment. See the [final code review](final-review.html) guide for details.

### Project Extra Credit

Students that are eligible for [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}) during finals week may complete additional [search engine](project-4b.html) functionality to earn extra credit in the project category. Students that are not eligible for this project may complete [other extra credit](extra-credit.html) opportunities instead at the end of the semester.

These extra credit opportunities can be completed to make up for points lost due to late penalties or other deductions in the projects category. These opportunities may not be used to earn over a 100% grade in this category. 
{% endcomment %}
