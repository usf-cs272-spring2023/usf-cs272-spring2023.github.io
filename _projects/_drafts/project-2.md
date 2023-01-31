---
title: Project 2 Partial Search
navbar: Guides
layout: guides
key: 2.0
bump: false
project: 2

# tags:
#  - text: 'New'
#    type: 'is-primary'

subsections:
  - text: 'Requirements'
    link: '#requirements'

  - text: 'Grading'
    link: '#grading'

  - text: 'Getting Started'
    link: '#getting-started'

tests: 'project2_tests'
review1: 'project2_review1'
review2: 'project2_review2'
design: 'project2_design'

review0: 'project1_review1'
design0: 'project1_design'

---

For this project, you will extend your [previous project](project-{{ page.project | minus: 1 }}.html) to support multi-word **exact search** and **partial search** queries. 

To do this, your code must modify how input text files are processed to also **track the total word count of each file** when building the inverted index. Your code must be able to output these word counts to file in pretty JSON format.

Your code must be able to process a query file line-by-line into a **normalized and optimized multiple-word queries** matching the processing used to build the inverted index. Then, it must conduct either an exact or partial search for those queries. An **exact search** will only return exact matches from the inverted index for any of the word stems in the multi-word query line. A **partial search** will return matches that **start with** any of the word stems instead.

Finally, your code must sort the search results using a simple **term frequency metric** to determine the most "useful" search results to output first. Your code must be able to output these sorted search results to file in a pretty JSON format.

## Requirements
{: .page-header }

The following detail the functionality requirements that must be implemented for this project.

### Input Requirements

Your `main` method must be placed in a class named `Driver` and must process the following command-line arguments:

  - `-counts [path]` where the flag `-counts` indicates the next argument `[path]` is the path to use to output all of the locations and their word count. 
  
      If the `[path]` argument is not provided, use `counts.json` as the default output filename. If the `-counts` flag is not provided, your could should **still calculate the word counts** but should not produce an output file of those counts.

  - `-query [path]` where the flag `-query` indicates the next argument `[path]` is a path to a text file of queries to be used for search. If this flag is not provided, then no search should be performed.

  - `-exact` where the flag `-exact` indicates all search operations performed should be exact search. If the flag is *not* present, any search operations should use partial search instead.

  - `-results [path]` where the flag `-results` indicates the next argument `[path]` is the path to use for the search results output file. This may include partial or exact search results! 
  
      If the `[path]` argument is not provided, use `results.json` as the default output filename. If the `-results` flag is not provided, do not produce an output file of search results but **still perform the search** operation.

The command-line flag/value pairs may be provided in any order or not at all. Do not convert paths to absolute form when processing command-line input!

Output **user-friendly error messages** in the case of exceptions or invalid input. Under no circumstance should your `main()` method output a stack trace to the user!

Your code should support all of the command-line arguments from the [previous project](project-1.html) as well.

### Calculating Word Count

Before you can calculate search results, you need to know how many word stems were stored in your inverted index for each text file.

**Avoid opening up any file more than once!** Your code should store this information alongside the inverted index, as the files are first processed.

These word counts will be output to file using the same pretty JSON format as the inverted index. Here is the expected output for the word count of all the text files associated with this project:

```json
{
  "input/text/guten/1400-0.txt": 187368,
  "input/text/guten/2701-0.txt": 215398,
  "input/text/guten/50468-0.txt": 10969,
  "input/text/guten/pg1322.txt": 124370,
  "input/text/guten/pg1661.txt": 107396,
  "input/text/guten/pg22577.txt": 63630,
  "input/text/guten/pg37134.txt": 16696,
  "input/text/rfcs/rfc3629.txt": 4294,
  "input/text/rfcs/rfc475.txt": 3228,
  "input/text/rfcs/rfc5646.txt": 27075,
  "input/text/rfcs/rfc6805.txt": 9785,
  "input/text/rfcs/rfc6838.txt": 9367,
  "input/text/rfcs/rfc7231.txt": 28811,
  "input/text/simple/.txt/hidden.txt": 1,
  "input/text/simple/a/b/c/d/subdir.txt": 1,
  "input/text/simple/animals.text": 11,
  "input/text/simple/animals_copy.text": 11,
  "input/text/simple/animals_double.text": 22,
  "input/text/simple/capital_extension.TXT": 1,
  "input/text/simple/capitals.txt": 4,
  "input/text/simple/digits.tXt": 2,
  "input/text/simple/dir.txt/findme.Txt": 17,
  "input/text/simple/hello.txt": 6,
  "input/text/simple/position.teXt": 20,
  "input/text/simple/symbols.txt": 10,
  "input/text/simple/words.tExT": 36,
  "input/text/stems/stem-in.txt": 22275,
  "input/text/stems/stem-out.txt": 22275
}
```

...and for the stand-alone `sentences.md` file:

```json
{
  "input/text/simple/sentences.md": 77
}
```
You can also find this output in the `expected/counts` subdirectory in the [`project-tests`]({{ site.github.owner_url }}/project-tests) repository.

### Processing Search Queries

Search queries will be provided in a text file with one **multi-word** search query per line. Eventually, these queries will come from a search box on a webpage instead.

When processing this file, your query parsing code must normalize and optimize the queries as follows:

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

### Calculating Search Results

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

### Output Requirements

The output of your inverted index should be the same from the previous project. See the "Calculating Word Count" section for the desired output.

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
