---
title: Project 3 Threads
navbar: Resources
layout: resources
key: 3.0
bump: false

project: 3

# tags:
#  - text: 'New'
#    type: 'is-primary'

subsections:
  - text: 'Project Release v3.0'
    link: '#project-release-v30'
  
  - text: 'Project Release v3.1'
    link: '#project-release-v31'

  - text: 'Project Release v3.x'
    link: '#project-release-v3x'

  - text: 'Run Examples'
    link: '#run-examples'

  - text: 'Getting Started'
    link: '#getting-started'

---

For this project, you will extend your [previous project](project-{{ page.project | minus: 1 }}.html) to support **multithreading** using a custom-made conditional read/write lock to create a **thread-safe inverted index** and a custom-made **work queue** to manage worker threads. The build process should be multithreaded such that a worker thread processes a single file. The search process should be multithreaded such that a worker thread processes a single multi-word query line.

To do this, you should create new classes (or extend existing classes) to support multithreading. **DO NOT REMOVE YOUR SINGLE THREADING CODE**, as you still need to support single threaded building and searching the index.

Do *NOT* use any of the classes in the `java.util.concurrent` package and do *NOT* use the `Stream.parallel` method for the multithreaded code.
{: .notification .is-danger .is-light }

## Project Release v3.0
{: .page-header }

The first release of this project must support multithreaded build using a custom-made conditional read/write lock to create a **thread-safe inverted index** and a custom-made **work queue** to manage worker threads. Specifically:

  - Your `main` method must process the command-line argument `-threads [num]` where the flag `-threads` enables multithreading, and indicates the next *optional* argument `[num]` is the number of worker threads to use. If the `[num]` argument is not provided, not a number, or less than `1`, use `5` as the default number of worker threads.

  - If multithreading is enabled, the inverted index must be made **thread-safe** using a custom-made **conditional read/write lock**. The conditional read/write lock must support concurrent read operations, non-concurrent write and read/write operations, and allow an active writer to acquire additional read or write locks as necessary (i.e. **reentrant** write lock).

  - If multithreading is enabled, your code must use a **work queue** to manage threads. The work queue must support the ability to automatically track pending (or unfinished) work (or tasks), and provide a method that waits until there is no more pending work. The work queue should also support the ability to shutdown gracefully, and should be gracefully shutdown after all building and searching operations are complete.

  - If multithreading is enabled, your code must support **multithreaded building** of the thread-safe inverted index using a work queue. Each worker thread should process one file at a time (i.e. create one "task" per file).

  - The output requirements are be the same as the [previous project](project-{{ page.project | minus: 1 }}.html). As before, your code should only generate output files if the necessary flags are provided.

Your code should support all of the functionality from the [previous project](project-{{ page.project | minus: 1 }}.html) as well.

If the `-threads` flag is not provided, then multithreading should not be enabled. A thread-safe inverted index should *not* be initialized, a work queue should *not* be initialized, no worker threads should be created, and the project should execute with a single main thread *exactly* as previous projects.
{: .notification .is-warning .is-light }

<!------------------------------------------------------->

{% assign release = "tests_v30" %}
{% assign prereqs = "tests_v21" %}

### {{ site.data.projects[release].text }}

{% include project_tags.html release=release prereqs=prereqs %}

This grade is earned by creating a release that passes the [`{{ site.data.projects[release].java }}`]({{ site.data.info.links.github.href }}/project-tests/blob/main/src/test/java/edu/usfca/cs272/{{ site.data.projects[release].java }}) tests locally and the tests tagged with `{{ site.data.projects[release].tags }}` remotely using GitHub Actions. Your code must also past the [{{ site.data.projects[prereqs].text }}]({{ site.data.projects[prereqs].book }}) tests from the previous project as well. Once passing, use the "Request Project Tests Grade" issue template to request a grade for the passing release.

It is difficult to detect whether your code is multithreading as required. The tests can only detect is whether your code initializes a work queue, not whether that work queue is properly used. Submitting code for a test grade that is not multithreading using a work queue and conditional wread/write lock will result in a **-5% to -10% deduction** to the test score. You are strongly encouraged to use logging to verify tasks are being created and run as intended.
{: .notification .is-danger .is-light }

<!------------------------------------------------------->

{% assign release = "review_v30" %}
{% assign prereqs = "tests_v30" %}

### {{ site.data.projects[release].text }}

{% include project_tags.html release=release prereqs=prereqs %}

This grade is earned by attending a code review appointment with the instructor. Before requesting code review, your code should have professional style and documentation and integrated feedback from *all* previous code reviews.

## Project Release v3.1
{: .page-header }

{% assign release = "tests_v31" %}
{% assign prereqs = "tests_v30" %}

The second release of this project must modify the existing functionality to also  support multithreaded search. It must also be *faster* than the single-threaded implementation. Specifically:

  - If multithreading is enabled, your code must support **multithreaded searching** of the thread-safe inverted index using a work queue. Each worker thread should process one multi-word query line at a time (i.e. create one task per line).

  - If multithreading is enabled, the multithreaded building and searching operations must be at least **{{ site.data.projects[release].time }} times faster** than the single-threaded implementations.

Your code should support all of the functionality from the [previous project](project-{{ page.project | minus: 1 }}.html) and release as well.

<!------------------------------------------------------->

### {{ site.data.projects[release].text }}

{% include project_tags.html release=release prereqs=prereqs %}

This grade is earned by creating a release that passes the [`{{ site.data.projects[release].java[0] }}`]({{ site.data.info.links.github.href }}/project-tests/blob/main/src/test/java/edu/usfca/cs272/{{ site.data.projects[release].java[0] }}) tests locally and the tests tagged with `{{ site.data.projects[release].tags }}` remotely using GitHub Actions. Your code must also past the [{{ site.data.projects[prereqs].text }}]({{ site.data.projects[prereqs].book }}) tests from the previous release as well. Once passing, use the "Request Project Tests Grade" issue template to request a grade for the passing release.

This release also has a speedup requirement. The multithreaded functionality must be **{{ site.data.projects[release].time }} times faster** than the single-threaded functionality. The benchmark tests are primarily located in the [`{{ site.data.projects[release].java[1] }}`]({{ site.data.info.links.github.href }}/project-tests/blob/main/src/test/java/edu/usfca/cs272/{{ site.data.projects[release].java[1] }}) test class tagged with `{{ site.data.projects[release].tags }}` in the code.

<!------------------------------------------------------->

{% assign release = "review_v31" %}

### {{ site.data.projects[release].text }}

{% include project_tags.html release=release prereqs="review_v30,tests_v31" %}

This grade is earned by attending a code review appointment with the instructor. Before requesting code review, your code should have professional style and documentation and integrated feedback from *all* previous code reviews.

## Project Release v3.x
{: .page-header }

All additional releases of this project must maintain the existing functionality and improve upon the design and efficiency of the code.

{% assign release = "review_v32" %}

### {{ site.data.projects[release].text }}

{% include project_tags.html release=release prereqs="review_v31" %}

All v3.2+ code reviews have a more strict speedup requirement. The multithreaded functionality must be **{{ site.data.projects[release].time }} times faster** than the single-threaded functionality. If you are unable to reach this speedup on GitHub Actions, reach out on Piazza for help before requesting a code review.

Once you are at this point in project {{ page.project }}, you should consider starting work on the [next project](project-{{ page.project | plus: 1 }}.html) in a separate branch.
{: .notification .is-warning .is-light }

{% assign release = "design_v3x" %}

### {{ site.data.projects[release].text }}

{% include project_tags.html release=release prereqs="review_v31" %}

This grade is earned by passing the code review process with the instructor. This takes approximately 3 to 4 code reviews total. The final project release must be **{{ site.data.projects[release].time }} times faster** than the single-threaded functionality when run on GitHub Actions.

## Run Examples
{: .page-header }

The following are a few examples (non-comprehensive) to illustrate the usage of the command-line arguments that can be passed to your `Driver` class via a "Run Configuration" in Eclipse, assuming you set the working directory to the `project-tests` directory.

Consider the following example:

```
-text "input/text/simple/" -query "input/query/simple.txt" -results actual/search-exact-simple.json -threads 3
```

The above arguments behave the same as [project {{ page.project | minus: 1 }}](project-{{ page.project | minus: 1 }}.html), except use `3` worker threads in a work queue to multithread. For v3.0 releases, this should multithread building only. For v3.1+ releases, this should multithread both building and searching.

```
-text "input/text/simple/" -query "input/query/simple.txt" -results actual/search-exact-simple.json -threads
```

The above arguments are nearly the same, except use the default of `5` worker threads.

```
-text "input/text/simple/" -query "input/query/simple.txt" -threads
```

The above arguments are similar, except it does NOT produce any file output. The code should still build the index and calculate the search results! This is important for benchmarking your code.

```
-text "input/text/simple/" -query "input/query/simple.txt" -results actual/search-exact-simple.json
```

This should behave *exactly* the same as [project {{ page.project | minus: 1 }}](project-{{ page.project | minus: 1 }}.html), using single-threading without any worker threads.

## Getting Started
{: .page-header }

The following sections may be useful for getting started on this project. 

**You should NOT wait until you have covered all of the associated homework assignments and lecture content to start the project.**{: .has-text-danger :} You should develop the project *iteratively* as you progress throughout the semester, integrating concepts one at a time into your project code.

### Related Content

The following homework assignments and lectures may be useful for this project:

  - The **[{{ site.data.homework.LoggerSetup.text }}]({{ site.data.homework.LoggerSetup.link }})** homework is useful for learning how to set up and configure `log4j2`, which will be helpful when it comes to debugging multithreaded code.

  - The **[{{ site.data.homework.ReadWriteLock.text }}]({{ site.data.homework.ReadWriteLock.link }})** homework is useful for creating a custom-made conditional read/write lock. The conditional lock class can be used directly for your project. It also illustrates how to use a conditional lock to make a data structure class thread-safe.

  - The **[{{ site.data.homework.PrimeFinder.text }}]({{ site.data.homework.PrimeFinder.link }})** homework is useful for creating a work queue that tracks pending work. This work queue class can be used directly for your project. It also illustrates how to use this work queue and create tasks for **non-recursive** problems.

  - The **synchronization** lecture code illustrates how to use a conditional read/write lock to make a data structure class thread-safe.

  - The **work queues** lecture code illustrates how to use a work queue and create tasks for recursive problems. If your approach is not recursive, this example might not be helpful for this project.

You can modify homework assignments and lecture code as necessary for this project. However, make sure your code is passing all of the tests and you understand the concepts *before* using the code.

### Suggestions

**Do not start on this project until you understand the multithreading code shown in class.** If you are stuck on the code shown in class, we are here to help!

Your goal should be to get to **testable code** as quickly as possible first, and then **developing iteratively** to pass the functionality tests. One possible breakdown of tasks are:

  - Configure `log4j2` add debug messages in your code. Once you are certain a class is working, disable debug messages for that class in your `log4j2.xml` configuration file.

  - Extend your previous inverted index to create a thread-safe version using a custom-made conditional read/write lock.

  - Create new code to build the index using a work queue (creating one task per file). Make sure your code still passes the tests.

  - Test your code in a multitude of settings and with different numbers of threads. Some issues will only come up occasionally, or only on certain systems.

  - Test your code with logging enabled. Then, test your code with logging completely disabled. Your code will run faster without logging, which sometimes causes some concurrency problems.

**Do not worry about efficiency until after your first code review.** 

It is important to **get started early** so you have plenty of time to think about how you want to approach the project *and* start coding iteratively. Planning to complete the code in too large of a chunk is a recipe to get stuck and fall behind!
