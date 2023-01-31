---
title: Project 4 Web Crawler
navbar: Guides
layout: guides
key: 4.1
bump: false
project: 4

tags:
  - text: 'New'
    type: 'is-primary'
#  - text: 'Pending'
#    type: 'is-muted'

subsections:
  - text: 'Requirements'
    link: '#requirements'

  - text: 'Grading'
    link: '#grading'

  - text: 'Getting Started'
    link: '#getting-started'

tests: 'project4_tests'
review1: 'project4_review'

review0: 'project3_review1'
design0: 'project3_design'

---

For this project, you will extend your [previous project](project-{{ page.project | minus: 1 }}.html) to create a multithreaded web crawler that builds the inverted index from a seed URL. To avoid a infinite crawl, the web crawler should crawl up to a **fixed limit of unique URLs** (including the seed URL).

Your multithreaded code should use a work queue such that a worker thread handles all of the processing associated with a single web page. This includes *everything* from downloading the web page to adding the processed text to the inverted index.

Do *NOT* use any of the classes in the `java.util.concurrent` package and do *NOT* use the `Stream.parallel` method for the multithreaded code.

**This writeup is for the web crawler functionality only.** See the general [Project 4 Final Project](project-4.html) guide for more details.

## Requirements
{: .page-header }

The following detail the functionality requirements that must be implemented for this project.

### Input Requirements

Your `main` method must be placed in a class named `Driver` and must process the following command-line arguments:

  - `-html [seed]` where the flag `-html` indicates the next argument `[seed]` is the seed URL your web crawler should initially crawl to build the inverted index. 
  
      If the `-html` flag is provided, your code should **enable multithreading** with the default number of worker threads even if the `-threads` flag is not provided.

  - `-max [total]` where the flag `-max` is an *optional* flag that indicates the next argument `[total]` is the total number of URLs to crawl (including the seed URL) when building the index. If the `[total]` argument is not provided or an invalid number, use `1` as the default value.

The command-line flag/value pairs may be provided in any order or not at all. Do not convert paths to absolute form when processing command-line input!

Output **user-friendly error messages** in the case of exceptions or invalid input. Under no circumstance should your `main()` method output a stack trace to the user!

Your code should support all of the command-line arguments from the [previous project](project-{{ page.project | minus: 1 }}.html) as well.

### Crawling the Web

Crawling the web can be done recursively. The process starts with a seed URL, and then is repeated for each new seed discovered. Specifically, for each page crawled, your code should:

  1. Use **sockets to download webpages** over HTTP or HTTPS, following up to 3 redirects. See the "Downloading URLs" section for details.

  2. Pre-process the HTML by **removing HTML block elements**, process HTTP/S links from the remaining HTML, and then add the found URLs to a queue for further crawling. See the "Processing URLs" for details.

  3. After processing links, **remove HTML tags and entities**, and then add the remaining stemmed words to the index. See the "Processing HTML" for details.

All of the above should happen within a worker thread; i.e. a worker thread will download, process, stem, and add one URL to the index at a time. 

For efficiency (and to avoid being blocked or rate-limited by the web server), **download the content only once** from the web server and only if necessary when crawling web pages. Specifically:

  - Do not fetch the web page content more than once. For example, do not fetch the entire web page content once to check the headers and again to process the HTML.

  - Do not fetch the web page content if it is not HTML. For example, for large text file without the `text/html` content-type, there is no need to download any of the content that appears after the headers.

  - Do not fetch the web page if it has already been encountered before, regardless of whether it was added to the index or skipped (due to an invalid type or status code).

See the following sections for additional details.

### Downloading URLs

For each unique normalized URL that must be crawled (up until the limit), the worker thread must:

  - Use sockets, NOT use the URL class, to download the web page over HTTP or HTTPs.

  - If the HTTP response status was a redirect, follow the redirect up to a limit of 3 redirects (to avoid an infinite redirect loop). The content at the end of this process will be assigned to the original URL.

    For example, the URL [~cs212/redirect/one](https://www.cs.usfca.edu/~cs212/redirect/one) eventually redirects to [~cs212/simple/hello.html](https://www.cs.usfca.edu/~cs212/simple/hello.html). However, the web crawler will associate the content of [~cs212/simple/hello.html](https://www.cs.usfca.edu/~cs212/simple/hello.html) with the original link [~cs212/redirect/one](https://www.cs.usfca.edu/~cs212/redirect/one) instead.

  - If the HTTP response status code is `200 OK` and the content-type is `text/html`, download the HTML (without other HTTP headers) for additional processing.

See the next section for how to process the downloaded HTML content.

### Processing URLs

After the HTML has been downloaded, the next step is to process that HTML to find additional links to crawl. Specifically:

  - Remove any HTML comments and block elements that should not be considered for parsing links, including the `head`, `style`, `script`, `noscript`, and `svg` elements.

  - Parse all of the URLs remaining on the page, and convert them to normalized absolute form (e.g. remove fragments, convert relative URLs to absolute URLs).

  - For each of the processed absolute URLs, queue a new crawl of that URL if:

    1. The maximum crawl limit has not yet been reached.

    2. This URL has not already been crawled or queued to be crawled.

If using the work queue properly and the maximum limit is 50 URLs, then the first 49 unique URLs on the seed page (plus the seed URL itself) will be part of the crawl.

### Process HTML

After the links have been processed, the next step is to remove the remaining HTML markup. The remaining text must be cleaned and stemmed the same way as text files. Specifically:

  - Remove all of the remaining HTML tags.

  - Convert any HTML 4 entities to their Unicode symbol and remove any other HTML entities found that could not be converted.

  - Clean, parse, and stem the resulting text.

  - Efficiently add the stems to the inverted index.

Once the stems are added to the index, the search functionality should work the same as before.

### Output Requirements

The output of your inverted index and search results should be the same from the [previous project](project-{{ page.project | minus: 1 }}.html), except that URLs will be output instead of file paths.

As before, your code should **only generate output files if the necessary flags are provided**. If the correct flags are provided, your code should perform the indexing and search operations even if file output is not being generated.

### Run Examples

The following are a few examples (non-comprehensive) to illustrate the usage of the command-line arguments that can be passed to your `Driver` class via a "Run Configuration" in Eclipse, assuming you set the working directory to the `project-tests` directory.

Consider the following example:

```
-html "https://usf-cs272-fall2022.github.io/project-web/input/simple/" -limit 15 -threads 3 -index index-crawl.json
```

The above arguments behave the same as [project {{ page.project | minus: 1 }}](project-{{ page.project | minus: 1 }}.html), except it will build the index from a seed web page and process up to `15` found links from that seed page.

```
-html "https://usf-cs272-fall2022.github.io/project-web/input/simple/" -index index-crawl.json
```

The above arguments are nearly the same, except use the default of `1` for the limit and `5` worker threads.

## Grading
{: .page-header }

This project grade is split into the following assignments:

| Assignment | Points | Deadline | Release | Prerequisites |
|:-----------|-------:|---------:|:-------:|:--------------|
| [{{ site.data.projects[page.tests].text }}]({{ site.data.projects[page.tests].canvas }}) | {{ site.data.projects[page.tests].points }} | {{ site.data.projects[page.tests].date | date: "%b %d, %Y" }} | `v{{ page.project }}.0.Z` | `Project{{ page.project }}Test.java`, [{{ site.data.projects[page.review0].text }}]({{ site.data.projects[page.review0].canvas }}), [Test Checks](grading.html#project-tests) |
| [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}) (30 min) | {{ site.data.projects[page.review1].points }} | {{ site.data.projects[page.review1].date | date: "%b %d, %Y" }} | `v{{ page.project }}.1.Z` | [{{ site.data.projects[page.design0].text }}]({{ site.data.projects[page.design0].canvas }}), [{{ site.data.projects[page.tests].text }}]({{ site.data.projects[page.tests].canvas }}), [Review Checks](grading.html#project-reviews) |
{: .table .is-auto .is-hoverable :}

It is only possible to earn credit for the [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}) during the [final code review](final-review.html) appointment. See the [Project 4](project4.html) guide for more details on how this project is graded.

## Getting Started
{: .page-header }

The following sections may be useful for getting started on this project.

### Related Homework

The following homework assignments may be useful for this project:

  - The **[{{ site.data.homework.linkfinder.text }}]({{ site.data.homework.linkfinder.link }})** homework is useful for parsing links from HTML (after block elements are removed). This homework can be used directly for your project.

  - The **[{{ site.data.homework.htmlcleaner.text }}]({{ site.data.homework.htmlcleaner.link }})** homework is useful for processing the download HTML. This homework can be used directly for your project, but be careful about how much HTML content is removed before links are parsed!

  - The **[{{ site.data.homework.htmlfetcher.text }}]({{ site.data.homework.htmlfetcher.link }})** homework is useful for following HTTP redirects and downloading HTML over a socket connection. This homework can be used directly for your project.

The homework assignments useful for project 3 related to multithreading will be useful again for this project too:

  - The **[{{ site.data.homework.loggersetup.text }}]({{ site.data.homework.loggersetup.link }})** homework is useful for learning how to set up and configure `log4j2`, which will be helpful when it comes to debugging multithreaded code.

  - The **[{{ site.data.homework.readwritelock.text }}]({{ site.data.homework.readwritelock.link }})** homework is useful for creating a  simple conditional read/write lock. The conditional lock class can be used directly for your project. It also illustrates how to use a conditional lock to make a data structure class thread-safe.

  - The **[{{ site.data.homework.primefinder.text }}]({{ site.data.homework.primefinder.link }})** homework is useful for creating a work queue that tracks pending work. This work queue class can be used directly for your project. It also illustrates how to use this work queue and create tasks for non-recursive problems.

You can modify homework assignments as necessary for this project. However, make sure your code is passing all of the tests before using.

**You should NOT wait until you have completed all of the associated homework assignments to start the project.**{: .has-text-danger :} You should develop the project *iteratively* as you progress throughout the semester, integrating assignments one at a time into your project code.

### Related Content

The following lecture content may be useful for this project:

  - The **regular expressions** lecture code illustrates how to use regular expressions, which will be useful for processing HTTP responses and HTML.

  - The **work queues** lecture code illustrates how to use a work queue and create tasks for recursive problems. If your approach is not recursive, this example might not be helpful for this project.

  - The **sockets** lecture code illustrates how to use sockets and create HTTP requests, which will be useful for communicating with web servers over HTTP.

You can use and modify lecture code as necessary for this project. However, make sure you understand the concepts *before* using the code.

**You should NOT wait until you have covered all of the associated lecture content to start the project.**{: .has-text-danger :} You should develop the project *iteratively* as you progress throughout the semester, integrating concepts one at a time into your project code.

### Suggestions

Your goal should be to get to **testable code** as quickly as possible first, and then **developing iteratively** to pass the functionality tests.

One possible breakdown of tasks are:

  - Outside of the relevant homework and lecture classes, there is only one new class (a web crawler class) required for this project.

  - Take a non-static instance-based approach to your web crawler. The web crawler needs to track which webpages have been already processed, which is easier to do with an instance-based approach.

  - Start with the test cases that add a single HTML webpage to the inverted index without any multithreading.

  - When those tests are passing, work on the test cases that add multiple HTML webpages to the inverted index using multithreading. You do *not* need to make a single-threaded version of this logic.

  - Your code must have an efficient approach to multithreading to pass the runtime tests. Wait until you are able to pass the runtime tests of the previous project before worrying about efficiency for this project.

It is important to **get started early** so you have plenty of time to think about how you want to approach the project *and* start coding iteratively. Planning to complete the code in too large of a chunk is a recipe to get stuck and fall behind!

<i class="fas fa-info-circle"></i>&nbsp;These hints may or may not be useful depending on your approach. Do not be overly concerned if you do not find these hints helpful for your approach for this project.
{: .notification }