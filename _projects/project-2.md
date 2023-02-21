---
title: Project 2 Search
navbar: Resources
layout: resources
key: 2.0
bump: false
project: 2

tags:
 - text: 'New'
   type: 'is-primary'

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

name1: 'SearchExact'
name2: 'SearchPartial'

review0: 'review_v10'
tests0: 'tests_v11'
design0: 'design_v1x'

tests1:  'tests_v20'
tests2:  'tests_v21'
review1: 'review_v20'
review2: 'review_v21'
design:  'design_v2x'

---

For this project, you will extend your [previous project](project-{{ page.project | minus: 1 }}.html) to support processing multi-word search queries from a query file, conducting an **exact search** or **partial search** through the inverted index data structure for those queries, and outputting the search results ranked by term frequency in pretty JSON format to file.

## Functionality
{: .page-header }

The functionality for this project can be broken up until the following subproblems:

  1. **Process** a file of multi-word search queries into an optimized and normalized form that is consistent with how the inverted index was built.

  2. **Find** exact search results (v2.0+) or partial search results (v2.1+) from the inverted index data structure for each multi-word search query.

  3. **Rank** (or sort) search results so the most relevant results are first using a simple term frequency metric.

  4. **Output** the search results using a pretty JSON format to file.

The first `v2.0` minor release of this project must support **exact search** only. The second `v2.1` and following releases must also support **partial search** as well. See below for additional details. 

### Processing Queries

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

### Finding Matches

For each unique multi-word query line, your code must find matches from the inverted index data structure and generate a **search result** for each match. A search result is the **location or file path** that the query or queries were found, with **additional metadata** that allows for the ranking of that search result (discussed later). 

There are two search modes that determine which locations should be considered a match:

  - An **exact search** must include results (file locations) from the inverted index that exactly match *any* of the word stems from the processed multi-word query line. For example, the exact word stem `perfor` appears in the file `words.tExT` a total of `2` times. This mode must supported by `v2.0+` releases.

  - A **partial search** will return results that **start with** any of the word stems instead. For example, a word stem starts with `perfor` in the file `words.tExT` a total of `11` times. This mode must be supported by `v2.1+` releases.

For a query with multiple words, your code should sum together the number of times a match is found for each query word found at a location. For example, for an exact search of the multiple word query `[loris, okapi]`, the word stem `lori` (from `loris`) appears `3` times and the word `okapi` appears `2` times in the file `animals.text`. Therefore, the number of times any of the matching query words appear in the text file is `3 + 2 = 5` times total.

<i class="fas fa-exclamation-triangle"></i>
Your code must find matches and generate results by iterating through your inverted index data structure. It should not re-open and stem any files when conducting an exact or partial search.
{: .notification .is-warning }

### Ranking Results

Search engines rank their search results such that the most useful results are provided first. For example, [PageRank](https://en.wikipedia.org/wiki/PageRank) is the well-known algorithm initially used by Google to rank results by estimating the importance of a website by its incoming links.

When returning search results, your code must also return the results in sorted order using a simple version of [term frequency](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) and raw count to determine the usefulness of a result. To do this, your code will need to be able to calculate and store the following metadata:

  - **Total Words:** The total number of word stems for current result. For example, there are `6` word stems in the `hello.txt` text file. Your code should already have this information from the word stem counts calculated alongside your inverted index.

  - **Match Count:** The total number of matches found for the current result, which depends on whether an **exact search** or **partial search** was conducted.

With this information you can calculate the **score** of the search result as the percent of words in the file that match the query. This is calculated by dividing the total matches by the total words:

```
score = total matches / total words
```

Then, when sorting the search results, compare the results as follows:

  1. **Score:** First, compare search results by their score so results with higher scores appear first (i.e. sort in **descending** order by total matches / total words).

  1. **Count:** If two search results have the same score, then compare by the match counts so results with higher counts appear first (i.e. sort in **descending** order by total matches) instead.

  1. **Location:** If two search results have the same score and count, then compare by the location string (case-insensitive) instead (i.e. sort alphabetically om **ascending** order ignoring case by the path string).

You can use [`Double.compare(...)`](https://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/lang/Double.html#compare(double,double)), [`Integer.compare(...)`](https://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/lang/Integer.html#compare(int,int)), and [`String.compareToIgnoreCase(…)`](https://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/lang/String.html#compareToIgnoreCase(java.lang.String)) for these comparisons and the built-in sort methods in Java.

See the simple partial search results for `ant perf` for an example of results sorted by score, the results for `lori` for an example of results with the same score and thus sorted by match count, and finally the results for `capybara hidden` for an example of results with the same score and same count and thus sorted alphabetically by location.

<i class="fas fa-exclamation-triangle"></i>
If you calculate the score using `float` instead of `double` objects, or sort using `Path` instead of `String` objects, you may not get the expected results!
{: .notification .is-warning }

### Outputting Search Results

The search results should be output as a JSON array of JSON objects, where each object represents a single line from the original query file (e.g. one search). The query objects should have the cleaned, sorted query as its key and the results as its value. Specifically:

  - The key should be the quoted text value that is the parsed and sorted query words joined together by a space. For example, if the parsed query words are `[capybara, hidden, observ]` then the text output should be `"capybara hidden observ"` for the key.

  - The value should be an array of nested JSON objects, one per search result. Each individual search result should have, in order, these keys:

      - The key `count` with an integer value that is the match count found within the text file location.

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

      - The key `where` with a quoted text value that is the relative path to the text file location matching one or more of the query words.

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

For the first minor release of this project, your project must support query file processing, **exact search**, and search result ranking. See the `SearchExactTests.java` class for the test cases for this release.

### Arguments v2.0

Your `main` method must be placed in a class named `Driver` and must process the following *additional* command-line arguments:

  - `-query [path]` where the flag `-query` indicates the next argument `[path]` is a path to a text file of queries to be used for search. If this flag is not provided, then no search should be performed.

      For the first release of your project, your code should conduct an **exact search** for each multi-word line of queries found in the query file.

  - `-results [path]` where the flag `-results` indicates the next argument `[path]` is the path to use for the search results output file.
  
      If the `[path]` argument is not provided, use `results.json` as the default output filename. If the `-results` flag is not provided, do not produce an output file of search results but **still perform the search** operation.

The command-line flag/value pairs may be provided in any order or not at all. Do not convert paths to absolute form when processing command-line input!

Output **user-friendly error messages** in the case of exceptions or invalid input. Under no circumstance should your `main()` method output a stack trace to the user!

Your code should support all of the command-line arguments from the [previous project](project-{{ page.project | minus: 1 }}.html) as well.

### Run Examples v2.0

The following are a few examples (non-comprehensive) to illustrate the usage of the command-line arguments that can be passed to your `Driver` class via a "Run Configuration" in Eclipse, assuming you set the working directory to the `project-tests` directory.

Consider the following example:

```
-text "input/text/simple/" -query "input/query/simple.txt" -results actual/exact-simple-simple.json
```

The above arguments indicate that `Driver` should build an inverted index from text files in the `input/text/simple/` subdirectory of the current working directory, process the `simple.txt` query file in the `input/query` subdirectory, conduct an **exact search**, and output the search results to `exact-simple-simple.json` in the `actual` subdirectory. This is equivalent to the `testSimpleSimple()` test method of `SearchExactTests.java` in the [`project-tests`]({{ site.github.owner_url }}/project-tests) repository.

Note that word counts should **always** be calculated even if the `-counts` flag is missing, and a search should **always** be conducted if the `-query` file is provided even if the `-results` flag is missing.

## Requirements v2.1
{: .page-header }

For the second minor release of this project, your project must support query file processing, **partial search**, and search result ranking. It must also still support `v2.0` functionality as well. See the `SearchPartialTests.java` class for the test cases for this and following releases.

### Arguments v2.1

Your `main` method must be placed in a class named `Driver` and must process the following *additional* command-line arguments:

  - `-partial` where the flag `-partial` indicates all search operations performed should use **partial** search. If the flag is not present, any search operations should use exact search instead.

The command-line flag/value pairs may be provided in any order or not at all. Do not convert paths to absolute form when processing command-line input!

Output **user-friendly error messages** in the case of exceptions or invalid input. Under no circumstance should your `main()` method output a stack trace to the user!

Your code should support all of the command-line arguments from the [previous release](#arguments-v20) and [previous project](project-1.html) as well.

### Run Examples v2.0

The following are a few examples (non-comprehensive) to illustrate the usage of the command-line arguments that can be passed to your `Driver` class via a "Run Configuration" in Eclipse, assuming you set the working directory to the `project-tests` directory.

Consider the following example:

```
-text "input/text/simple/" -query "input/query/simple.txt" -partial -results actual/partial-simple-simple.json
```

The above arguments indicate that `Driver` should build an inverted index from text files in the `input/text/simple/` subdirectory of the current working directory, process the `simple.txt` query file in the `input/query` subdirectory, conduct an **partial search**, and output the search results to `partial-simple-simple.json` in the `actual` subdirectory. This is equivalent to the `testSimpleSimple()` test method of `SearchPartialTests.java` in the [`project-tests`]({{ site.github.owner_url }}/project-tests) repository.

Note that word counts should **always** be calculated even if the `-counts` flag is missing, and a search should **always** be conducted if the `-query` file is provided even if the `-results` flag is missing.

## Assignments
{: .page-header }

<style>
td:nth-child(4) {
  white-space: nowrap;
}
</style>

This project is broken into several assignments, including 2 test checkpoints, 2 review checkpoints, and a final design assignment:

| Deadline | Assignment | Test File(s) | Tag(s) | Prerequisites | Requirements |
|:--------:|:-----------|:-------------|:------------|:--------------|:-------------|
| {{ site.data.projects[page.tests1].date | date: "%b %d" }} | [{{ site.data.projects[page.tests1].text }}]({{ site.data.projects[page.tests1].book }}) | {{ page.name1 }} | test-v{{ page.project }}.0 | [{{ site.data.projects[page.review0].text }}]({{ site.data.projects[page.review0].book }}), [{{ site.data.projects[page.tests0].text }}]({{ site.data.projects[page.tests0].book }}) | Pass tests locally and remotely |
| {{ site.data.projects[page.review1].date | date: "%b %d" }} | [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].book }}) | {{ page.name1 }} | test-v{{ page.project }}.0 | [{{ site.data.projects[page.tests1].text }}]({{ site.data.projects[page.tests1].book }}), pass review checks | Attend code review with instructor |
| {{ site.data.projects[page.tests2].date | date: "%b %d" }} | [{{ site.data.projects[page.tests2].text }}]({{ site.data.projects[page.tests2].book }}) | {{ page.name2 }} | test-v{{ page.project }}.1 | [{{ site.data.projects[page.tests1].text }}]({{ site.data.projects[page.tests1].book }}) | Pass tests locally and remotely |
| {{ site.data.projects[page.review2].date | date: "%b %d" }} | [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].book }}) | {{ page.name2 }} | test-v{{ page.project }}.1 | [{{ site.data.projects[page.tests2].text }}]({{ site.data.projects[page.tests2].book }}), [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].book }}), pass review checks | Attend code review with instructor |
| {{ site.data.projects[page.design].date | date: "%b %d" }} | [{{ site.data.projects[page.design].text }}]({{ site.data.projects[page.design].book }}) | {{ page.name2 }} | test-v{{ page.project }}.x | [{{ site.data.projects[page.tests2].text }}]({{ site.data.projects[page.tests2].book }}), [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].book }}), [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].book }}), pass review checks | Pass code review with instructor (may take several additional code reviews) |

The "Test File(s)" column provides the prefix to the test files to focus on while developing locally. For example, "{{ page.name1 }}" refers to the "{{ page.name1 }}Tests.java" file in the the [`project-tests`]({{ site.github.owner_url }}/project-tests) repository.

You may start working on passing [{{ site.data.projects[page.tests1].text }}]({{ site.data.projects[page.tests1].book }}) for the `v{{ page.project }}.0` release of your project after passing both [{{ site.data.projects[page.review0].text }}]({{ site.data.projects[page.review0].book }}) and [{{ site.data.projects[page.tests0].text }}]({{ site.data.projects[page.tests0].book }}). However, **if you are not yet passing [{{ site.data.projects[page.design0].text }}]({{ site.data.projects[page.design0].book }}), you must create a new branch** for this functionality! You may not have `v{{ page.project }}.0` functionality in your `main` branch until completing the code reviews and passing the design for [project {{ page.project | minus: 1 }}](project-{{ page.project | minus: 1 }}.html) first!

## Getting Started
{: .page-header }

The following sections may be useful for getting started on this project.

### Related Homework

The following homework assignments may be useful for this project:

  - The **[{{ site.data.homework.ArgumentParser.text }}]({{ site.data.homework.ArgumentParser.href }})** homework is useful for processing the command-line arguments. This homework can be used directly for your project.

  - The **[{{ site.data.homework.FileStemmer.text }}]({{ site.data.homework.FileStemmer.href }})** homework is useful for processing query files. This homework can be used directly for your project.

  - The **[{{ site.data.homework.JsonWriter.text }}]({{ site.data.homework.JsonWriter.href }})** homework is useful for producing pretty JSON output. This homework can be used directly and extended as needed for your project.

  - The **[{{ site.data.homework.FileSorter.text }}]({{ site.data.homework.FileSorter.href }})** homework is useful for an example of how to create a custom comparable object for storing search metrics and sorting by those metrics. This homework should *not* be used directly for your project; there is no need to implement multiple different `Comparator` classes using different approaches.

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

  - Add the ability to **parse query files**. Compare your parsed queries to those in the expected output files. For example, the line `performer performers` should become `perform` after being parsed, cleaned, stemmed, sorted, and discarding duplicates.

  - Create a class that stores a **single search result** and implements the [Comparable](hhttps://www.cs.usfca.edu/~cs272/javadoc/api/java.base/java/lang/Comparable.html) interface. You will need data members for each of the sort criteria, including the location, total word count of the location, and the number of matches for that location.

  - Add an **exact search method** to your inverted index data structure that takes already parsed words from a single line of the query file, and returns a sorted list of search results. Output the results to the console for debugging.

  - Add the ability to **output the search results** to JSON file. Make sure the exact search results match the expected output.

After passing the first set of tests, then start working on:

  - Add a **partial search method** to your inverted index data structure that takes already parsed words from a single line of the query file, and returns a sorted list of search results.

Do not worry about efficiency, duplicate code, or encapsulation until *after* you are getting correct results!

It is important to **get started early** so you have plenty of time to think about how you want to approach the project *and* start coding iteratively. Planning to complete the code in too large of a chunk is a recipe to get stuck and fall behind!

<i class="fas fa-info-circle"></i>&nbsp;These hints may or may not be useful depending on your approach. Do not be overly concerned if you do not find these hints helpful for your approach for this project.
{: .notification }