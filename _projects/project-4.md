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

  - Any URLs (or links) stored by your code must be in a cleaned, absolute, normalized, decoded form. This includes removing the fragment of the URL, converting to absolute, normalizing the path, and decoding any URL-encoded characters. The `URL` and `URI` classes may be used to represent and parse URLs, encoding/decoding each component, and converting relative URLs to absolute normalized form, but may not be used to download the headers or content of URLs.

  - Web pages must be requested using **sockets and HTTP/S** from the web server as follows:

    - If the HTTP headers returned by the web server includes a `200 OK` HTTP/S response status code and HTML content type, then download, process, and add the HTML content to the inverted index.
      
    - If the HTTP headers returned by the web server includes a **redirect** HTTP/S response status code, follow up to 3 redirects maximum until a `200 OK` is returned. Associate the final response with the *original* URL and process. For example, the URL [~cs212/redirect/one](https://www.cs.usfca.edu/~cs212/redirect/one) eventually redirects to [~cs212/simple/hello.html](https://www.cs.usfca.edu/~cs212/simple/hello.html). The web crawler will associate the HTTPS response of [~cs212/simple/hello.html](https://www.cs.usfca.edu/~cs212/simple/hello.html) with the original URL [~cs212/redirect/one](https://www.cs.usfca.edu/~cs212/redirect/one) when processing.

  - For efficiency (and to avoid being blocked or rate-limited by the web server), **download the content only once** from the web server and only if necessary when crawling web pages. Specifically:
  
      - Do not fetch the web page content more than once. For example, do not fetch the entire web page content once to check the headers and again to process the HTML.

      - Do not fetch the web page content if it is not HTML and a `200 OK` status code. For example, *only* the headers (not the content) will be downloaded for large text file without the `text/html` content-type, or for a `404` status web page.

  - The downloaded HTML must be processed before being stored in the inverted index as follows:

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

### ~~Project v{{ page.project }}.0 Review~~

There are no code reviews for this release. You are on your own to apply the concepts from class in your design.

## Project Release v{{ page.project }}.1
{: .page-header }

This project release must modify your web crawler functionality to use multithreading and crawl multiple pages at once. Specifically:

  - Your `main` method must modify how it process the command-line argument `-html [seed]` as follows:

      - If the `-html` flag is provided, your code should **enable multithreading** with the default number of worker threads even if the `-threads` flag is not provided. 

      - If the *optional* `-crawl [total]` arguments are provided, the flag `-crawl` flag that indicates the next argument `[total]` is the total number of URLs to crawl (including the seed URL) when building the index. If the `[total]` argument is not provided or an invalid number, use `1` as the default value.
  
  - All crawling must be multithreaded using a **thread-safe** inverted index and **work queue** such that a worker thread (or task) handles all of the processing associated with a single web page. This includes *everything* from downloading the HTTP headers to adding the processed text to the inverted index, even if building from only one web page.

  - The processing of the HTML must be modified to support crawling other web pages as follows:

      - After removing all HTML comments and block elements, but *before* removing all HTML tags, list all of the cleaned, absolute, normalized, decoded URLs within the HTML content.

      - For each of these URLs, queue a new crawl of that URL if:

          - The maximum crawl limit has not yet been reached.

          - That URL has not already been crawled or queued to be crawled. 
          
        A web page is considered *crawled* if its headers are fetched from a web server, even if it does not result in a `200 OK` status code. Do not fetch the web page if it has already been encountered before, regardless of whether it was added to the index or skipped (due to an invalid type or status code).

      - If using the work queue properly and the maximum crawl limit is 50 URLs, then the first 49 unique URLs on the seed page (plus the seed URL itself) will be part of the crawl.

Your code should support all of the functionality from the previous release as well.

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

### ~~Project v{{ page.project }}.1 Review~~

There are no code reviews for this release. You are on your own to apply the concepts from class in your design.

### ~~Project v{{ page.project }}.x Design~~

There is no design grade for this project. However, the design of this project could impact the [Project v5.0 Design]({{ site.data.projects.design_v50.book }}) grade!

## Run Examples
{: .page-header }

The following are a few examples (non-comprehensive) to illustrate the usage of the command-line arguments that can be passed to your `Driver` class via a "Run Configuration" in Eclipse, assuming you set the working directory to the `project-tests` directory.

Consider the following example:

```
-html "https://usf-cs272-fall2022.github.io/project-web/input/simple/" -crawl 15 -threads 3 -index index-crawl.json
```

The above arguments behave the same as [project {{ page.project | minus: 1 }}](project-{{ page.project | minus: 1 }}.html), except it will use a work queue to build the index from a seed web page and crawl up to `15` found links from that seed page.

```
-html "https://usf-cs272-fall2022.github.io/project-web/input/simple/" -index index-crawl.json
```

The above arguments are nearly the same, except use the default of `1` for the crawl limit. For `v4.1+` releases and later, it will also use a default of `5` worker threads in the work queue.

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
