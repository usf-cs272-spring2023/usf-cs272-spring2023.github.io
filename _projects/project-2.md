---
title: Project 2 Search
navbar: Resources
layout: resources
key: 2.0
bump: false
project: 2

tags:
 - text: 'Pending'
  #  type: 'is-primary'

subsections:
  - text: 'Functionality'
    link: '#functionality'
  
  - text: 'Requirements v2.0'
    link: '#requirements-v20'

  - text: 'Requirements v2.1 '
    link: '#requirements-v21'

  - text: 'Assignments'
    link: '#assignments'

  - text: 'Getting Started'
    link: '#getting-started'

tests1:  'tests_v20'
tests2:  'tests_v21'
review1: 'review_v20'
review2: 'review_v21'
design:  'design_v2x'

---

For this project, you will extend your [previous project](project-{{ page.project | minus: 1 }}.html) to support conducting an **exact search** or **partial search** of multi-word queries through the inverted index data structure.

## Functionality
{: .page-header }

The functionality for this project can be broken up until the following subproblems:

  1. **Process** a file of multi-word search queries into an optimized and normalized form that is consistent with how the inverted index was built.

  2. **Find** exact or partial search results from the inverted index data structure for each multi-word search query.

  3. **Rank** (or sort) search results so the most relevant results are first using a simple term frequency metric.

  4. **Output** the search results using a pretty JSON format to file.

The first `v2.0` minor release of this project must support **exact search**. The second `v2.1` and following releases must also support **partial search** as well. See below for additional details. 

### Processing Search Queries

Search queries will be provided in a text file with one **multi-word** search query per line. When processing this file, your query parsing code must normalize and optimize the queries as follows:

  - **Clean and parse each query line.** Perform the same transformations to each line of query words as used when populating your inverted index. This includes cleaning the line of any non-alphabetic characters, converting the remaining characters to lowercase, splitting the cleaned line into words by whitespace, and stemming each word. For example, the query line:

      ```
      Observers observing 99 HIDDEN               capybaras!
      ```

    ...should be processed into the cleaned, parsed, and stemmed words `[observ, observ, hidden, capybara]` after this step.

  - **Remove duplicate query word stems.** There is no need to search for the same stem twice! For example, if the original query included both observers and observing, the stemmed version of both words observ should be included in the parsed query only once. Specifically, for the stemmed words:

      ```
      [observ, observ, hidden, capybara]
      ```

    ...the duplicate `observ` should be removed resulting in `[observ, hidden, capybara]` after this step.

  - **Sort the unique words for each query alphabetically.** This is less necessary for the search operation, but required for the search result output. For example, if the stemmed words without duplicates are:

      ```
      [observ, hidden, capybara]
      ```

    ...then the final parsed query words should be `[capybara, hidden, observ]` after this step.

At this stage, the normalized words from the original query line can be used by your inverted index data structure to generate search results and to produce the expected JSON file output.

### Finding Search Results

For each unique multi-word query line, your code must find **search results** from the inverted index data structure. A search result is primarily a location or file path that the query or queries were found, with additional metadata that allows for the ranking of that search result (discussed later). 

There are two search modes that determine which locations should be included in the search results for each query line:

  - An **exact search** must include results (file locations) from the inverted index that exactly match *any* of the word stems from the processed multi-word query line.

  - A **partial search** will return matches that **start with** any of the word stems instead.

Finally, your code must sort the search results using a simple **term frequency metric** to determine the most "useful" search results to output first. Your code must be able to output these sorted search results to file in a pretty JSON format.

### Ranking Search Results

Search engines rank their search results such that the most useful results are provided first. For example, [PageRank](https://en.wikipedia.org/wiki/PageRank) is the well-known algorithm initially used by Google to rank results by estimating the importance of a website by its incoming links.

When returning search results, your code must also return the results in sorted order using a simple version of [term frequency](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) and raw count to determine the usefulness of a result. To do this, your code will need to be able to calculate the following:

  - **Total Words:** The total number of word stems in each text file. For example, there are `6` word stems in the `hello.txt` text file. You should already have this information from the word count calculated alongside your inverted index.

  - **Total Matches:** The total number of times any of the matching query words appear in the text file.

      - For **exact search**, this is the number of times the exact word stems appear. For example, the exact word stem `perfor` appears in the file `words.tExT` a total of `2` times.

      - For **partial search**, this is the number of times a word stem *starts with* one of the query words. For example, a word stem starts with `perfor` in the file `words.tExT` a total of `11` times.

    For a query with multiple words, your code should sum together the number of times a match is found for each query word found at a location. For example, for an exact search of the multiple word query `[loris, okapi]`, the word stem `lori` (from `loris`) appears `3` times and the word `okapi` appears `2` times in the file `animals.text`. Therefore, the number of times any of the matching query words appear in the text file is `3 + 2 = 5` times total.

With this information you can calculate the score of the search result as the percent of words in the file that match the query. This is calculated by dividing the total matches by the total words:

```
score = total matches / total words
```

Then, when sorting the search results, compare the results as follows:

  1. **Score:** First, compare search results by their score so results with higher scores appear first (i.e. sort in descending order by total matches / total words).

  1. **Count:** If two search results have the same score, then compare by the raw count so results with higher count appear first (i.e. sort in descending order by total matches) instead.

  1. **Location:** If two search results have the same score and count, then compare by the location string (case-insensitive) instead (i.e. sort alphabetically ignoring case by the path string).

You can use [`Double.compare(...)`](https://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/lang/Double.html#compare(double,double)), [`Integer.compare(...)`](https://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/lang/Integer.html#compare(int,int)), and [`String.compareToIgnoreCase(…)`](https://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/lang/String.html#compareToIgnoreCase(java.lang.String)) for these comparisons and the built-in sort methods in Java.

See the simple partial search results for `ant perf` for an example of results sorted by score, the results for `lori` for an example of results with the same score and thus sorted by count, and finally the results for `capybara hidden` for an example of results with the same score and same count and thus sorted alphabetically by location.

<i class="fas fa-exclamation-triangle"></i>
If you calculate the score using `float` instead of `double` objects, or sort using `Path` instead of `String` objects, you may not get the expected results!
{: .notification .is-warning }

<i class="fas fa-exclamation-triangle"></i>
Remember, mutable objects do not behave well when used in sets or as keys in maps. Use lists and <a href="https://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/util/Collections.html#sort(java.util.List)">Collections.sort(List)</a> to sort search results instead, not a <a href="https://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/util/TreeSet.html">TreeSet</a> data structure. 
{: .notification .is-warning }

### Outputting Search Results

The search results should be output as a JSON array of JSON objects, where each object represents a single line from the original query file (e.g. one search). The query objects should have the cleaned, sorted query as its key and the results as its value. Specifically:

  - The key should be the quoted text value that is the parsed and sorted query words joined together by a space. For example, if the parsed query words are `[capybara, hidden, observ]` then the text output should be `"capybara hidden observ"` for the key.

  - The value should be an array of nested JSON objects, one per search result. Each individual search result should have, in order, these keys:

      - The key `count` with an integer value that is the raw count (i.e. total matches) within the text file.

      - The key `score` with a floating point value with 8 digits after the decimal point of the search result score. You can use a `DecimalFormat` object to achieve this output:

          ```java
          DecimalFormat FORMATTER = new DecimalFormat("0.00000000");
          System.out.println(FORMATTER.format(Math.PI));
          ```

        Alternatively, you can use format strings:

          ```java
          String formatted = String.format("%.8f", Math.PI);
          System.out.println(formatted);
          ```

      - The key `where` with a quoted text value that is the relative path to the text file matching one or more of the query words.

The use of spaces, newline characters, and spaces are the same as before for “pretty” JSON output. Here is an example output snippet of a single search query with a single search result:

```json
  "observ perfor": [
    {
      "count": 13,
      "score": 0.36111111,
      "where": "input/text/simple/words.tExT"
    }
  ],
```

You can also find this output in the `search-exact-simple-simple.json` file in the [`project-tests`]({{ site.github.owner_url }}/project-tests) repository. See the other expected output files for more examples.

## Requirements v2.0
{: .page-header }

For the first minor release of this project, your project must support query file processing, **exact search**, and search result ranking.

### Arguments v2.0

Your `main` method must be placed in a class named `Driver` and must process the following command-line arguments:

  - `-query [path]` where the flag `-query` indicates the next argument `[path]` is a path to a text file of queries to be used for **exact** search. If this flag is not provided, then no search should be performed.

  - `-results [path]` where the flag `-results` indicates the next argument `[path]` is the path to use for the search results output file.
  
      If the `[path]` argument is not provided, use `results.json` as the default output filename. If the `-results` flag is not provided, do not produce an output file of search results but **still perform the search** operation.

The command-line flag/value pairs may be provided in any order or not at all. Do not convert paths to absolute form when processing command-line input!

Output **user-friendly error messages** in the case of exceptions or invalid input. Under no circumstance should your `main()` method output a stack trace to the user!

Your code should support all of the command-line arguments from the [previous project](project-1.html) as well.

### File Input v2.0

Pending

### File Output v2.0

{% comment %}










### Run Examples

The following are a few examples (non-comprehensive) to illustrate the usage of the command-line arguments that can be passed to your `Driver` class via a "Run Configuration" in Eclipse, assuming you set the working directory to the `project-tests` directory.

Consider the following example:

```
-text "input/text/simple/" -query "input/query/simple.txt" -exact -results actual/search-exact-simple.json
```

The above arguments indicate that `Driver` should build an inverted index from text files in the `input/text/simple/` subdirectory of the current working directory, process the `simple.txt` query file in the `input/query` subdirectory, conduct an exact search, and output the search results to `search-exact-simple.json` in the `actual` subdirectory. This is equivalent to the `testSimpleDirectory()` test method of `Project2Test.java` in the [`project-tests`]({{ site.github.owner_url }}/project-tests) repository.

```
-text "input/text/simple/" -query "input/query/simple.txt" -results actual/search-exact-simple.json
```

The above arguments are nearly the same, but should trigger a partial search instead.

Note that word counts should **always** be calculated even if the `-counts` flag is missing, and a search should **always** be conducted if the `-query` file is provided even if the `-results` flag is missing.

## Grading
{: .page-header }

This project grade is split into the following assignments:

| Assignment | Points | Deadline | Release | Prerequisites |
|:-----------|-------:|---------:|:-------:|:--------------|
| [{{ site.data.projects[page.tests].text }}]({{ site.data.projects[page.tests].canvas }}) | {{ site.data.projects[page.tests].points }} | {{ site.data.projects[page.tests].date | date: "%b %d, %Y" }} | `v{{ page.project }}.0.Z` | `Project{{ page.project }}Test.java`, [{{ site.data.projects[page.review0].text }}]({{ site.data.projects[page.review1].canvas }}), [Test Checks](grading.html#project-tests) |
| [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}) (30 min) | {{ site.data.projects[page.review1].points }} | {{ site.data.projects[page.review1].date | date: "%b %d, %Y" }} | `v{{ page.project }}.1.Z` | [{{ site.data.projects[page.design0].text }}]({{ site.data.projects[page.design0].canvas }}), [{{ site.data.projects[page.tests].text }}]({{ site.data.projects[page.tests].canvas }}), [Review Checks](grading.html#project-reviews) |
| [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].canvas }}) (15 min)<sup>1</sup> | {{ site.data.projects[page.review2].points }} | {{ site.data.projects[page.review2].date | date: "%b %d, %Y" }} | `v{{ page.project }}.2.Z` | [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}), [Review Checks](grading.html#project-reviews) |
| Project {{ page.project }} Review Y (15 min)<sup>2</sup> | 0 | {{ site.data.projects[page.design].date | date: "%b %d, %Y" }} | `v{{ page.project }}.Y.Z` | [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].canvas }}), [Review Checks](grading.html#project-reviews) |
| [{{ site.data.projects[page.design].text }}]({{ site.data.projects[page.design].canvas }}) | {{ site.data.projects[page.design].points }} | {{ site.data.projects[page.design].date | date: "%b %d, %Y" }} | `v{{ page.project }}.Y.Z` | [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].canvas }}), [Review Checks](grading.html#project-reviews), Passing Pull Request |
{: .table .is-auto .is-hoverable :}

<sup>1</sup> You should start on [project {{ page.project | plus: 1 }}](project-{{ page.project | plus: 1 }}.html) after receiving a grade for [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].canvas }})---do NOT wait to finish this project to start the next one!

<sup>2</sup> It takes approximately 3 to 4 code reviews to pass the design of this project. Keep in mind you may have only 30 minutes worth of code review appointments every 5 days. Due to weekends, that works out to approximately 3 weeks or more of code reviews.

See the [Project Grading](grading.html) guide for details on how projects are graded. 

## Getting Started
{: .page-header }

The following sections may be useful for getting started on this project.

### Related Homework

The following homework assignments may be useful for this project:

  - The **[{{ site.data.homework.argumentparser.text }}]({{ site.data.homework.argumentparser.link }})** homework is useful for processing the command-line arguments. This homework can be used directly for your project.

  - The **[{{ site.data.homework.wordcleaner.text }}]({{ site.data.homework.wordcleaner.link }})** homework is useful for processing query files. This homework can be used directly for your project.

  - The **[{{ site.data.homework.prettyjsonwriter.text }}]({{ site.data.homework.prettyjsonwriter.link }})** homework is useful for producing pretty JSON output. This homework can be used directly and extended as needed for your project.

  - The **[{{ site.data.homework.filesorter.text }}]({{ site.data.homework.filesorter.link }})** homework is useful for an example of how to create a custom comparable object for storing search metrics and sorting by those metrics. This homework should *not* be used directly for your project; there is no need to implement multiple different `Comparator` classes using different approaches.

You can modify homework assignments as necessary for this project. However, make sure your code is passing all of the tests before using.

**You should NOT wait until you have completed all of the associated homework assignments to start the project.**{: .has-text-danger :} You should develop the project *iteratively* as you progress throughout the semester, integrating assignments one at a time into your project code.

### Related Content

The following lecture content may be useful for this project:

  - The lectures on **data structures** (especially iteration and search) may be helpful for creating the exact and partial search algorithms.

  - The lectures on **inheritance** and **functional programming** may be helpful for sorting search results.

You can use and modify lecture code as necessary for this project. However, make sure you understand the concepts *before* using the code.

**You should NOT wait until you have covered all of the associated lecture content to start the project.**{: .has-text-danger :} You should develop the project *iteratively* as you progress throughout the semester, integrating concepts one at a time into your project code.

### Suggestions

Your goal should be to get to **testable code** as quickly as possible first, and then **developing iteratively** to pass the functionality tests. Once you have testable code, try working on the following:

  - Add the ability to **store total word count** whenever a file is indexed, and the ability to output these counts when the `-counts` flag is provided.

  - Add the ability to **parse query files**. Compare your parsed queries to those in the expected output files. For example, the line `performer performers` should become `perform` after being parsed, cleaned, stemmed, sorted, and discarding duplicates.

  - Create a class that stores a **single search result** and implements the [Comparable](hhttps://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/lang/Comparable.html) interface. You will need data members for each of the sort criteria, including the location, total word count of the location, and number of times the query occurs at that location.

  - Add an **exact search method** to your inverted index data structure that takes already parsed words from a single line of the query file, and returns a sorted list of search results. Output the results to the console for debugging.

  - Add the ability to **output the search results** to JSON file. Make sure the exact search results match the expected output.

  - Add a **partial search method** to your inverted index data structure that takes already parsed words from a single line of the query file, and returns a sorted list of search results.

  - Do not worry about efficiency, duplicate code, or encapsulation until *after* you are getting correct results.

It is important to **get started early** so you have plenty of time to think about how you want to approach the project *and* start coding iteratively. Planning to complete the code in too large of a chunk is a recipe to get stuck and fall behind!

<i class="fas fa-info-circle"></i>&nbsp;These hints may or may not be useful depending on your approach. Do not be overly concerned if you do not find these hints helpful for your approach for this project.
{: .notification }
{% endcomment %}