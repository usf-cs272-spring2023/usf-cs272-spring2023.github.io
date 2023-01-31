---
title: Project 1 Inverted Index
navbar: Resources
layout: resources
key: 1.0
bump: true
project: 1

tags:
 - text: 'Pending'
#    type: 'is-primary'

subsections:
  - text: 'Requirements v1.0'
    link: '#requirements-v10'

  - text: 'Requirements v1.1 '
    link: '#requirements-v11'

  - text: 'Grading'
    link: '#grading'

  - text: 'Getting Started'
    link: '#getting-started'

tests: 'project1_tests'
review1: 'project1_review1'
review2: 'project1_review2'
design: 'project1_design'

---

For this project, you will write a Java program that processes all text files in a directory and its subdirectories, cleans and parses the text into [word stems](https://en.wikipedia.org/wiki/Stemming), builds a word count and in-memory [inverted index](https://en.wikipedia.org/wiki/Inverted_index) from that processed text, and outputs it [JSON](http://json.org/) format.

The process of **[stemming](https://en.wikipedia.org/wiki/Stemming)** reduces a word to a base form (or "stem"), so that words like `interesting`, `interested`, and `interests` all map to the stem `interest`. Stemming is a common preprocessing step in many search engines.

An **inverted index** is a nested data structure that stores the mapping from words to the documents and positions within those documents where those words were found. It is a common in-memory data structure used by many search engines.

## Requirements v1.0
{: .page-header }

For the first minor release of this project, your project must process either a single text file or process a directory of text files into word stems and store word counts of the processed file(s).

### Arguments v1.0

Your `main` method must be placed in a class named `Driver` and must process the following command-line arguments:

  - `-text [path]` where the flag `-text` indicates the next argument `[path]` is a path to either a single file or a directory.

      If the value is a single file (regardless of its extension), open and process that file. If the value is a directory, find and add all of the text files (with `.txt` and `.text` extensions) in that directory and its subdirectories and process those files.
      
      See the "File Input v1.0" below for details on how to process the files and store the calculated word counts.

  - `-counts [path]` where the flag `-counts` indicates the next argument `[path]` is the path to use to output all of the processed locations and their word counts in a "pretty JSON" format.
  
      If the `[path]` argument is not provided, use `counts.json` as the default output filename. If the `-counts` flag is not provided, your could should **still calculate the word counts** but should not produce an output file of those counts. 
      
      See the "File Output v1.0" below for details on the "pretty JSON" output format.

The command-line flag/value pairs may be provided in any order or not at all. Do not convert paths to absolute form when processing command-line input!

Output **user-friendly error messages** in the case of exceptions or invalid input. Under no circumstance should your `main()` method output a stack trace to the user!

### File Input v1.0

If the appropriate command-line arguments are provided, process the provided input path as follows:

  - If a single file is provided as input, process that individual file regardless of its extension. 

  - If a directory is provided as input, process all text files within that directory and all subdirectories. Any files that end in the `.text` or `.txt` extension (case-insensitive) should be considered a text file.

  - Use the `UTF-8` character encoding and efficient buffered reading for all file processing.

When processing a file, clean and parse the text into word stems by removing any non-letter symbols (including digits, punctuation, accents, special characters), convert the remaining alphabetic characters to lowercase, split the text into words by whitespace, and then stem the word using the [Apache OpenNLP](https://opennlp.apache.org/) toolkit. Specifically:

  - Use the regular expression `(?U)[^\\p{Alpha}\\p{Space}]+` to remove special characters from text.

  - Use the regular expression `(?U)\\p{Space}+` to split text into words by whitespace.

  - Use the [SnowballStemmer](https://opennlp.apache.org/docs/1.9.3/apidocs/opennlp-tools/opennlp/tools/stemmer/snowball/SnowballStemmer.html) English stemming algorithm in OpenNLP to stem words.

Store the processed file path and number of found word stems (the word count) in an appropriate data structure.

### File Output v1.0

If the appropriate command-line arguments are provided, the contents of your word counts data structure should be output using the human-readable "pretty" [JSON](http://json.org/) format. Specifically:

  - Output the path locations as `String` types in alphabetically sorted order.

  - Use two <code>&nbsp;&nbsp;</code> space characters for indentation and newline `\n` characters in between elements.

  - Numbers like integers should never be quoted. Any string or object key, however, should always be surrounded by `"` quotes. 
  
  - Objects (similar to maps) should use `{` and `}` curly braces, and arrays should use `[` and `]` square brackets. 
  
  - Make sure there are no trailing commas after the last element.

  - The paths should be output in the form they were originally provided. Do not convert paths to absolute form or convert the path separators!

  - Use the `UTF-8` character encoding and efficient buffered writing to produce the necessary output.

Here is the expected output for the word count of all the text files associated with this project:

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

You can also find this output in the `expected-nix/counts` subdirectory in the [`project-tests`]({{ site.github.owner_url }}/project-tests) repository.

The project tests account for different path separators (forward slash `/` for Linux/Mac systems, and backward slash `\` for Windows systems). **Your code does NOT have to convert between the two!**
{: .notification .is-warning }

### Run Examples v1.0

The following are a few non-comprehensive examples to illustrate the usage of the command-line arguments that can be passed to your `Driver` class via a "Run Configuration" in Eclipse, assuming you set the working directory to the `project-tests` directory.

<details>
<p><summary>View Details</summary></p>

<div markdown=1>
Consider the following example:

```
-text "input/text/simple/hello.txt"
-counts "actual/counts-simple-hello.json"
```

The above arguments indicate that `Driver` should process the single `hello.txt` file in the `input/text/simple` subdirectory of the current working directory, and output the word counts as JSON to the `counts-simple-hello.json` file in the `actual` subdirectory.

```
-text "input/text/simple/" -counts
```

The above arguments indicate that `Driver` should process all of the text files found in the `text/simple` subdirectory of the current working directory, and output the word counts as JSON to the default path `counts.json` in the current working directory.

```
-text "input/text/simple/"
```

The above arguments indicate that `Driver` should process all of the text files found in the `input/text/simple` subdirectory of the current working directory, but it should *not* produce an output file. It must still calculate and store the word counts in memory however!

```
-counts
```

The above arguments indicate that `Driver` should output the word counts as JSON to the default path `counts.json` in the current working directory. The word counts will be empty, since no `-path` flag argument was provided.
</div>
</details>

## Requirements v1.1
{: .page-header }

For the second minor release of this project, your project must also store an in-memory inverted index of the processed file(s) alongside the word counts.

### Arguments v1.1

In addition to the command-line arguments from previous releases, your `main` method process the following command-line arguments:

  - `-text [path]` behaves as before, except additional processing must happen per file to build an in-memory inverted index data structure alongside the word counts. See the "File Input v1.1" section below for details.

  - `-index [path]` where the flag `-index` is flag that indicates the inverted index should be output to a JSON file, and the next *optional* argument `[path]` is the path to use for the output file. If the optional path argument is not provided, use `index.json` as the default output path. 
      
      If the `-index` flag is provided, always generate output even if it is empty. If the `-index` flag is not provided, do not produce an output file.

      See the "File Output v1.1" below for details.

The command-line flag/value pairs may be provided in any order or not at all. Do not convert paths to absolute form when processing command-line input!

Output **user-friendly error messages** in the case of exceptions or invalid input. Under no circumstance should your `main()` method output a stack trace to the user!

### File Input v1.1

The input text files must be cleaned and stemmed as before, however your code must now also create an in-memory **inverted index** data structure alongside the word counts. The inverted index must store a mapping from a word to the location(s) the word was found, and the position(s) in that file the word is located. The positions should start at 1. *This will require nesting multiple built-in data structures.* 

For example, suppose we have the following mapping stored in our inverted index:

```json
{
  "capybara": {
    "input/mammals.txt": [
      11
    ]
  },
  "platypus": {
    "input/dangerous/venomous.txt": [
      2
    ],
    "input/mammals.txt": [
      3,
      8
    ]
  }
}
```

This indicates that the word `capybara` is found in the file `input/mammals.html` in position `11`. The word `platypus` is found in two files, `input/mammals.html` and `input/dangerous/venomous.html`. In the file `input/mammals.html`, the word `platypus` appears twice in positions `3` and `8`. In file `input/dangerous/venomous.html`, the word `platypus` is in position `2` in the file. 

Each file should only be opened once; the word counts and inverted index should be built at the same time.

### File Output v1.1

The output of the inverted index data structure must use the "pretty" JSON format as the word counts. Since the inverted index is a nested data structure, the JSON output must also be nested. The inner-most word positions should be output as a JSON array of numbers (with square braces), whereas the other levels should be output as JSON objects (with curly braces). 

The "File Input v1.1" section provides an example of the expected JSON output. You can find additional example output files in the `expected-nix/index` subdirectory in the [`project-tests`]({{ site.github.owner_url }}/project-tests) repository.

### Run Examples v1.1


The following are a few non-comprehensive examples to illustrate the usage of the command-line arguments that can be passed to your `Driver` class via a "Run Configuration" in Eclipse, assuming you set the working directory to the `project-tests` directory.

<details>
<p><summary>View Details</summary></p>

<div markdown=1>
Consider the following example:

```
-text "input/text/simple/hello.txt"
-index "actual/counts-simple-hello.json"
```

The above arguments indicate that `Driver` should process the single `hello.txt` file in the `input/text/simple` subdirectory of the current working directory, and output the inverted index as JSON to the `index-simple-hello.json` file in the `actual` subdirectory.

```
-text "input/text/simple/" -counts -index
```

The above arguments indicate that `Driver` should process all of the text files found in the `text/simple` subdirectory of the current working directory, then output the word counts as JSON to the default path `counts.json` and the inverted index as JSON to the default path `index.json` in the current working directory.

```
-text "input/text/simple/"
```

The above arguments indicate that `Driver` should process all of the text files found in the `input/text/simple` subdirectory of the current working directory, but it should *not* produce an output file. It must still calculate and store the word counts and inverted index in memory however!

```
-index
```

The above arguments indicate that `Driver` should output the inverted index as JSON to the default path `index.json` in the current working directory. The inverted index will be empty, since no `-path` flag argument was provided.
</div>
</details>

## Grading
{: .page-header }

Pending

## Getting Started
{: .page-header }

The following sections may be useful for getting started on this project.

### Related Homework

The following homework assignments may be useful for this project:

  - The **[{{ site.data.homework.ArgumentParser.text }}]({{ site.data.homework.ArgumentParser.href }})** homework is useful for processing the command-line arguments. This homework can be used directly for your project.

  - The **[{{ site.data.homework.FileStemmer.text }}]({{ site.data.homework.FileStemmer.link }})** homework is useful for converting text files into word stems. This homework can be used directly for your project.

  - The **[{{ site.data.homework.JsonWriter.text }}]({{ site.data.homework.JsonWriter.href }})** homework is useful for producing pretty JSON output. This homework can be used directly and extended as needed for your project.

  - The **[{{ site.data.homework.FileIndex.text }}]({{ site.data.homework.FileIndex.href }})** homework is useful as an example for designing a data structure like an inverted index.
  
      *This homework should NOT be used directly for your project; a forward index has a different underlying structure than an inverted index!*

  - The **[{{ site.data.homework.FileFinder.text }}]({{ site.data.homework.FileFinder.link }})** homework is useful for listing all of the text files in a directory using **functional programming**. This can also be done *without* functional programming using examples from the file IO lectures.

You can modify homework assignments as necessary for this project. However, make sure your code is passing all of the tests before using.

**You should NOT wait until you have completed all of the associated homework assignments to start the project.**{: .has-text-danger :} You should develop the project *iteratively* as you progress throughout the semester, integrating assignments one at a time into your project code.

### Related Content

The following lecture content may be useful for this project:

  - The lectures on **exception handling** may be useful for interacting with the user.

  - The lectures on **file IO** may be useful for reading and writing large files efficiently.

  - The lectures on **data structures** may be useful for creating and iterating through data structures in an efficient and generalized way.

  - The lectures on **object-oriented programming** may be useful for designing reusable and generalized code.

  - The lectures on **inheritance** and **functional programming** may also be helpful, but are NOT required for core functionality.

You can use and modify lecture code as necessary for this project. However, make sure you understand the concepts *before* using the code.

**You should NOT wait until you have covered all of the associated lecture content to start the project.**{: .has-text-danger :} You should develop the project *iteratively* as you progress throughout the semester, integrating concepts one at a time into your project code.

### Suggestions

Your goal should be to get to **testable code** as quickly as possible first, and then **developing iteratively** to pass the `v1.0` functionality tests first. One possible breakdown of tasks are:

  1. Figure out how to run tests *individually* and focus only on the first test method until that test passes. Do NOT run all of the tests at once!

  2. Create code that handles parsing command-line arguments for the first test. Output the parsed arguments to the console.

  3. Create code that is able to parse the single text file specified by the first test into word stems. Output the stems to the console.

  4. Create code that is able to write the word stems to the JSON file specified by the first test. Do NOT worry about the data structure or format of the output file yet!

  5. Compare the actual output file produced by your code to the expected file. While the output format will not match, you want to manually check the same stems are found in both files.

You now have **testable code** at this point. Once you have testable code, try working on the following:

  {:start="6"}
  6. Create the data structure that will store your word counts.

  7. Create code to output your data structure to file, but not yet in the right format. Manually check that the path locations and counts match the expected output.

  8. Create code to output your data structure to file in proper "pretty" JSON format.

Start trying to pass all of the single file tests. Do not move on until the tests for single files are passing. Then, try the following:

  {:start="8"}
  8. Create code to find all of the text files in a directory and output those to the console. Manually check that it is finding the right set of text files.

  9. Create code (ideally reusing what you already have) to process the text files into your data structure.

Then, start trying to pass the directory tests. Do not move on until those tests are passing. Finally, work on the exception tests *last* after everything else is passing. 

Once you have the `v1.0` functionality complete, adding to your existing data structure code to enable storing data in an inverted index format as well. Then, create code that can output that data structure as JSON.

It is important to **get started early** so you have plenty of time to think about how you want to approach the project *and* start coding iteratively. Planning to complete the code in too large of a chunk is a recipe to get stuck and fall behind!

<i class="fas fa-info-circle"></i>&nbsp;These hints may or may not be useful depending on your approach. Do not be overly concerned if you do not find these hints helpful for your approach for this project.
{: .notification }

{% comment %}


### Output Requirements

If the appropriate command-line arguments are provided, the contents of your inverted index should be output in alphabetically sorted order as a nested JSON object using a "pretty" format using two <code>&nbsp;&nbsp;</code> space characters for indentation.

According to the [JSON standard](http://json.org/), numbers like integers should never be quoted. Any string or object key, however, should always be surrounded by `"` quotes. Objects (similar to maps) should use `{` and `}` curly braces, and arrays should use `[` and `]` square brackets. Make sure there are no trailing commas after the last element. For example:

```json
{
  "capybara": {
    "input/mammals.txt": [
      11
    ]
  },
  "platypus": {
    "input/dangerous/venomous.txt": [
      2
    ],
    "input/mammals.txt": [
      3,
      8
    ]
  }
}
```



## Grading
{: .page-header }

This project grade is split into the following assignments:

| Assignment | Points | Deadline | Release | Prerequisites |
|:-----------|-------:|---------:|:-------:|:--------------|
| [{{ site.data.projects[page.tests].text }}]({{ site.data.projects[page.tests].canvas }}) | {{ site.data.projects[page.tests].points }} | {{ site.data.projects[page.tests].date | date: "%b %d, %Y" }} | `v{{ page.project }}.0.Z` | `Project{{ page.project }}Test.java`, [Test Checks](grading.html#project-tests) |
| [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}) (30 min) | {{ site.data.projects[page.review1].points }} | {{ site.data.projects[page.review1].date | date: "%b %d, %Y" }} | `v{{ page.project }}.1.Z` | [{{ site.data.projects[page.tests].text }}]({{ site.data.projects[page.tests].canvas }}), [Review Checks](grading.html#project-reviews) |
| [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].canvas }}) (15 min)<sup>1</sup> | {{ site.data.projects[page.review2].points }} | {{ site.data.projects[page.review2].date | date: "%b %d, %Y" }} | `v{{ page.project }}.2.Z` | [{{ site.data.projects[page.review1].text }}]({{ site.data.projects[page.review1].canvas }}), [Review Checks](grading.html#project-reviews) |
| Project {{ page.project }} Review Y (15 min)<sup>2</sup> | 0 | {{ site.data.projects[page.design].date | date: "%b %d, %Y" }} | `v{{ page.project }}.Y.Z` | [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].canvas }}), [Review Checks](grading.html#project-reviews) |
| [{{ site.data.projects[page.design].text }}]({{ site.data.projects[page.design].canvas }}) | {{ site.data.projects[page.design].points }} | {{ site.data.projects[page.design].date | date: "%b %d, %Y" }} | `v{{ page.project }}.Y.Z` | [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].canvas }}), [Review Checks](grading.html#project-reviews), Passing Pull Request |
{: .table .is-auto .is-hoverable :}

<sup>1</sup> You should start on [project {{ page.project | plus: 1 }}](project-{{ page.project | plus: 1 }}.html) after receiving a grade for [{{ site.data.projects[page.review2].text }}]({{ site.data.projects[page.review2].canvas }})---do NOT wait to finish this project to start the next one!

<sup>2</sup> It takes approximately 4 to 5 code reviews to pass the design of this project. Keep in mind you may have only 30 minutes worth of code review appointments every 5 days. Due to weekends, that works out to approximately 3 weeks or more of code reviews.

See the [Project Grading](grading.html) guide for details on how projects are graded. 


{% endcomment %}