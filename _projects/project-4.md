---
title: Project 4 Crawl
navbar: Resources
layout: resources
key: 4.0

project: 4

tags:
  - text: 'New'
    type: 'is-primary'

subsections:
  - text: 'Project Release v4.0'
    link: '#project-release-v40'
  
  - text: 'Project Release v4.1'
    link: '#project-release-v41'

  - text: 'Run Examples'
    link: '#run-examples'

  - text: 'Getting Started'
    link: '#getting-started'

---

For this project, you will extend your [previous project](project-{{ page.project | minus: 1 }}.html) to create a **web crawler** that builds the inverted index from a seed URL.

There are no code review or design grade assignments for this project. The next project that will be reviewed is [Project 5](project-5.html) during finals week only. However, the design of this project could impact the [Project v5.0 Design]({{ site.data.projects.design_v50.book }}) grade!
{: .notification .is-danger .is-light }

## Project Release v{{ page.project }}.0
{: .page-header }

This project release must support building an inverted index from a single web page as follows:

  - Your `main` method must process the command-line argument `-html [seed]` where the flag `-html` indicates the next argument `[seed]` is the seed URL your web crawler should initially crawl to build the index. By default, only one web page will be crawled. 

  - Web pages must be requested using **sockets and HTTP/S** from the web server. The `URL` class may be used to represent and parse URLs into their components, but may not be used to download the headers or content of URLs.
      
    - If the web server returns a `200 OK` HTTP/S response status code and HTML content type, download and process the HTML.
      
    - If the web server returns a **redirect** HTTP/S response status code, follow up to 3 redirects maximum until a `200 OK` is returned. Associate the final response with the *original* URL and process. For example, the URL [~cs212/redirect/one](https://www.cs.usfca.edu/~cs212/redirect/one) eventually redirects to [~cs212/simple/hello.html](https://www.cs.usfca.edu/~cs212/simple/hello.html). The web crawler will associate the HTTPS response of [~cs212/simple/hello.html](https://www.cs.usfca.edu/~cs212/simple/hello.html) with the original URL [~cs212/redirect/one](https://www.cs.usfca.edu/~cs212/redirect/one) when processing.
      
  - The download HTML must be processed before being stored in the inverted index as follows:

    1. Remove HTML comments and the `head`, `style`, `script`, `noscript`, and `svg` block elements.

    1. Remove all of the remaining HTML tags.

    1. Convert any HTML 4 entities to their Unicode symbol and remove any other HTML entities found that could not be converted.

    1. Clean, parse, and stem the resulting text.

    1. Efficiently add the stems to the inverted index with the absolute, cleaned, and normalized URL as the location.

  - The output requirements are the same as the [previous project](project-{{ page.project | minus: 1 }}.html), except that absolute, cleaned, and normalized URLs will be output instead of path locations. As before, your code should only generate output files if the necessary flags are provided.

Your code should support all of the functionality from the [previous project](project-{{ page.project | minus: 1 }}.html) as well. This includes the ability to search from a query file!

Eventually, your web crawler must be multithreaded. It is up to you whether you want to start with a single threaded or multithreaded implementation for this release.
{: .notification .is-warning .is-light }

<!------------------------------------------------------->

{% assign release = "tests_v40" %}
{% assign prereqs = "tests_v31,review_v32" %}

### {{ site.data.projects[release].text }}

{% include project_tags.html release=release prereqs=prereqs %}

This grade is earned by meeting the following requirements:

  1. You must already have a passing grade for the [{{ site.data.projects["review_v32"].text }}]({{ site.data.projects["review_v32"].book }}) assignment from the [previous project](project-{{ page.project | minus: 1 }}.html).

  1. You must create a release that:
    
      - Passes the [`{{ site.data.projects[release].java }}`]({{ site.data.info.links.github.href }}/project-tests/blob/main/src/test/java/edu/usfca/cs272/{{ site.data.projects[release].java }}) tests locally

      - Passes the tests tagged with `{{ site.data.projects[release].tags }}` remotely using GitHub Actions

      - Passes the [{{ site.data.projects["tests_v31"].text }}]({{ site.data.projects["tests_v31"].book }}) tests from the [previous project](project-{{ page.project | minus: 1 }}.html)
    
Once ready, use the "Request Project Tests Grade" issue template to request a grade for the passing release.

### ~~Project v4.0 Review~~

There are no code reviews for this release. You are on your own to apply the concepts from class in your design.

## Project Release v{{ page.project }}.1
{: .page-header }

This project release must modify your web crawler functionality to use multithreading and crawl multiple pages at once. Specifically:

  - Details pending.

Do *NOT* use any of the classes in the `java.util.concurrent` package and do *NOT* use the `Stream.parallel` method for the multithreaded code.
{: .notification .is-danger .is-light }

<!------------------------------------------------------->

{% assign release = "tests_v41" %}
{% assign prereqs = "tests_v40" %}

### {{ site.data.projects[release].text }}

{% include project_tags.html release=release prereqs=prereqs %}

This grade is earned by meeting the following requirements:

  1. You must already have a passing grade for the [{{ site.data.projects["review_v32"].text }}]({{ site.data.projects["review_v32"].book }}) assignment from the [previous project](project-{{ page.project | minus: 1 }}.html).

  1. You must create a release that:
    
      - Passes the [`{{ site.data.projects[release].java }}`]({{ site.data.info.links.github.href }}/project-tests/blob/main/src/test/java/edu/usfca/cs272/{{ site.data.projects[release].java }}) tests locally

      - Passes the tests tagged with `{{ site.data.projects[release].tags }}` remotely using GitHub Actions

      - Passes the [{{ site.data.projects["tests_v31"].text }}]({{ site.data.projects["tests_v31"].book }}) tests from the [previous project](project-{{ page.project | minus: 1 }}.html)

      - Executes at least **{{ site.data.projects[release].time }} times faster** with 3 worker threads versus 1 worker thread

Once ready, use the "Request Project Tests Grade" issue template to request a grade for the passing release.

### ~~Project v4.0 Review~~

There are no code reviews for this release. You are on your own to apply the concepts from class in your design.

### ~~Project v4.x Design~~

There is no design grade for this project. However, the design of this project could impact the [Project v5.0 Design]({{ site.data.projects.design_v50.book }}) grade!

## Run Examples
{: .page-header }

Details pending.

## Getting Started
{: .page-header }

The following sections may be useful for getting started on this project. 

**You should NOT wait until you have covered all of the associated homework assignments and lecture content to start the project.**{: .has-text-danger :} You should develop the project *iteratively* as you progress throughout the semester, integrating concepts one at a time into your project code.

### Related Content

The following homework assignments and lectures may be useful for this project:

  - The **[{{ site.data.homework.HtmlFetcher.text }}]({{ site.data.homework.HtmlFetcher.link }})** homework is useful for following HTTP/S redirects and downloading HTML over a socket connection. This homework can be used directly for your project.

  - The **[{{ site.data.homework.HtmlCleaner.text }}]({{ site.data.homework.HtmlCleaner.href }})** homework is useful for processing the download HTML. This homework can be used directly for your project, but be careful about how much HTML content is removed before links are parsed!

  - The **[{{ site.data.homework.LinkFinder.text }}]({{ site.data.homework.LinkFinder.href }})** homework is useful for parsing links from HTML (after block elements are removed). This homework can be used directly for your project.

  - The **regular expressions** lecture code illustrates how to use regular expressions, which will be useful for processing HTTP responses and HTML.

  - The **work queues** lecture code illustrates how to use a work queue and create tasks for recursive problems. If your approach is not recursive, this example might not be helpful for this project.

  - The **sockets** lecture code illustrates how to use sockets and create HTTP requests, which will be useful for communicating with web servers over HTTP.

You can modify homework assignments and lecture code as necessary for this project. However, make sure your code is passing all of the tests and you understand the concepts *before* using the code.

### Suggestions

Your goal should be to get to **testable code** as quickly as possible first, and then **developing iteratively** to pass the functionality tests. One possible breakdown of tasks are:

  - Outside of the relevant homework and lecture classes, there is only one new class (a web crawler class) required for this project.

  - Take a non-static instance-based approach to your web crawler. The web crawler eventually needs to track which web pages have been already processed, which is easier to do with an instance-based approach.

  - Create a method that can add a single HTML web page to the inverted index without any multithreading.

  - When those tests are passing, work on the test cases to add multiple HTML web pages to the inverted index using multithreading instead.

  - Your code must have an efficient approach to multithreading to pass the benchmark tests. Wait until you are able to pass the benchmark tests of the previous project before worrying about efficiency for this project.

It is important to **get started early** so you have plenty of time to think about how you want to approach the project *and* start coding iteratively. Planning to complete the code in too large of a chunk is a recipe to get stuck and fall behind!

<i class="fas fa-info-circle"></i>&nbsp;These hints may or may not be useful depending on your approach. Do not be overly concerned if you do not find these hints helpful for your approach for this project.
{: .notification }

{% comment %}

      If the `-html` flag is provided, your code should **enable multithreading** with the default number of worker threads even if the `-threads` flag is not provided. 

  - Your multithreaded crawler code 

Your multithreaded code should use a work queue such that a worker thread handles all of the processing associated with a single web page. This includes *everything* from downloading the web page to adding the processed text to the inverted index.

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


{% endcomment %}