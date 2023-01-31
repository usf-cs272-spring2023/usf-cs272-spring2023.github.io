---
title: Project Extra Credit
navbar: Guides
layout: guides
key: 5.2

tags:
  - text: 'New'
    type: 'is-primary'

---

This guide details extra credit opportunities that can be completed for projects 1, 2, and 3. Students that are eligible for the [Project 4b Search Engine](project-4b.html) project should complete the extra credit for that project instead.

These extra credit opportunities can be completed as part of the [extension process](final-review.html#extensions) only. Otherwise, your efforts are better spent catching up on late work instead.

## Eligibility
{: .page-header }

Extra credit may only be requested to make up for points missed due to late penalties on completed project assignments with a non-zero grade in Canvas. It may not be requested to make up for points missed due to other deductions or for incomplete project assignments for which a partial grade was assigned.

For example, suppose you completed the assignments for project 1 on time, assignments for project 2 late, and received a partial grade of 50/100 points on project 3 tests due to failing test cases at the end of the semester. You may only earn extra credit for points lost due to late penalties on project 2 in this case. If you lost `2` points on project 2 tests and `10` points on project 2 design, you may earn up to `12` points of extra credit total. If you completed `20` points of extra credit options, the extra credit will be capped to `12` points instead. In this scenario, if you also want to earn extra credit on points lost on project 3, you must first work to pass all of the tests first and request a late grade.

## Options
{: .page-header }

Below are some extra credit options. Each option is worth between 5 to 20 points of extra credit.

  - **Driver Logic (5 points):** Reduce the duplicate logic in your `Driver` class for outputting data to a JSON file using lambda expressions (but not stream pipelines).

  - **Custom Sorting (5 points)**: Create a `Comparator` that will sort your custom search result object by score, total number of words in the file (instead of the number of matches), and then the path. Create test code to demonstrate this `Comparator` works. This test code can be a simple class with hard-coded arguments and console output; it does not need to be a unit test using JUnit.

  - **JSON Logic (10 points):** Reduce the duplicate logic in your JSON writing and stemming classes using lambda expressions (but not stream pipelines).

  - **Stemming Logic (10 points):** Reduce the duplicate logic in your stemming classes using lambda expressions (but not stream pipelines).

  - **Verbose Flag  (10 points)**: Add a flag, `-verbose`, that enables logging `DEBUG` messages and above to the console. Add `DEBUG` messages before and after any file is read or written in the code, in every constructor call, and any time a thread is about to terminate. Create test code to demonstrate this feature works. This test code can be a simple class with hard-coded arguments and console output; it does not need to be a unit test using JUnit.

  - **Load JSON (15 Points):** Add a flag `-load [path]` to load an inverted index from a JSON file. For example, `-load index.json` should read the JSON file at path `index.json` and load it into the inverted index being used for search. Use the [`org.json`](https://github.com/stleary/JSON-java) package to parse the JSON text. You can decide how you want to handle updating the word counts when loading the inverted index from JSON. Create test code to demonstrate this feature works. This test code can be a simple class with hard-coded arguments and console output; it does not need to be a unit test using JUnit.

  - **Javadoc (15 points):** Update and polish all of your Java documentation, then use the `javadoc` tool to generate a documentation website. Place the generated html in a [docs](https://github.blog/2016-08-22-publish-your-project-documentation-with-github-pages/) subfolder in your `main` branch. Ask the instructor or TA to enable GitHub Pages for your repository so that your documentation is viewable as a website.

  - **Index Empty Tests (20 points):** Create custom JUnit tests for your inverted index data structure. There should be 1 test per public method that use an **empty** index.

  - **Index Non-Empty Tests (20 points):** Create custom JUnit tests for your inverted index data structure. There should be 1 test per public method that use a **non-empty** index.


You are encouraged to ask for clarification on any of these you are interested in completing in a **public** anonymous post. You are also welcome to propose other extra credit options  in a **public** anonymous post.

## Submission
{: .page-header }

You must first have an [approved extension](final-review.html#extensions) to submit extra credit. See the [final code review](final-review.html#extensions) writeup for the deadline to complete extra credit.

To submit your extra credit, make a single private post with *all* of the extra credit features you completed by the deadline. You should make only one post; do not post until all of the features you planned to implement are complete. In your post, make sure to include your name and the total amount of extra credit you are requesting. For each extra credit option, you should also include (1) the name of the option and (2) a link to the exact branch, file(s), and line(s) of code with your implementation.

**Be prepared to answer followup questions in a timely manner.** If I cannot find and verify your implementation(s), you will have one opportunity to clarify where to look. However, you will *not* have an opportunity to fix any issues with your implementation(s).
