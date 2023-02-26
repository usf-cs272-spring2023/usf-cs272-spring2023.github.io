---
title: Exam 2 Review
navbar: Guides
layout: default
key: 1.2

tags:
  - text: 'New'
    type: 'is-primary'

topics:
  - text: 'Logging'
    tags: 'debugging'
    code: ['Logging']

  - text: 'Regular Expressions'
    tags: 'regexes'
    code: ['RegularExpressions']

#  - text: 'Software Testing'
#    tags: 'testing'
#    code: ['UnitTesting']


  - text: 'Multithreading'
    tags: 'threads'
    code: ['Multithreading', 'Synchronization', 'WorkQueues']

  - text: 'Web and Sockets'
    tags: 'web'
    code: ['HyperText', 'Sockets']

  - text: 'Jetty and Servlets'
    tags: 'servlets'
    code: ['ServletBasics', 'ServletData', 'Sessions']

  - text: 'Databases and SQL'
    tags: 'sql'
    code: false

---

<style>
ul.icons {
  list-style-type: none;
  margin-left: 1.5em;
  margin-top: 0em;
}

ul.icons > li {
  position: relative;
}

ul.icons > li > i {
  width: 1.25em;
  left: -1.5em;
  position: absolute;
  text-align: center;
  line-height: inherit;
}
</style>

The exam will cover topics on the lecture content, homework assignments, and quizzes covered up thus far in class. This includes the following topics:

{% for topic in page.topics -%}
  - {{ topic.text }}
{% endfor %}

See below for resources and additional details.

## Logistics

In addition to the general [exam logistics](exam-logistics.html), you want to consider the following:

  - You should bookmark your favorite regular expression, <!--JUnit,--> Log4j2, HTTP, Jetty, and SQL resources. The README files of the lecture code have several recommendations for each topic.

  - Make sure you can access the on-campus database. You will be able to `SOURCE` the tables from the exam and test out your SQL queries there.

Everything else will be similar to the previous exam.

## Resources

{% assign columns = 'slides,quizzes,homework' | split: ',' %}
<table class="table is-hoverable">
<thead>
  <tr>
    <th width="20%">Topic and Code</th>
    <th width="30%">Lecture Slides</th>
    <th width="30%">Practice Quizzes</th>
    <th width="20%">Homework</th>
  </tr>
</thead>

<tbody>

{% for topic in page.topics %}
<tr>
  <td>
    <ul class="icons">
      {% if topic.code %}
      {% for repo in topic.code %}
      <li><i class="{{ site.data.icons.code.class }}"></i> <a href="{{ site.data.info.links.github.link }}/lectures/tree/main/{{ repo }}">{{ repo }}</a></li>
      {% endfor %}
      {% else %}
      <li><i class="{{ site.data.icons.pending.class }}"></i> {{ topic.text }}</li>
      {% endif %}
    </ul>
  </td>

  {% for column in columns %}
  {% assign data = site.data[column] %}
  {% assign filtered = data | where_exp:"item", "item.tags contains topic.tags" %}
  <td>
    <ul class="icons">
      {% for item in filtered -%}
      <li>
        <i class="{{ site.data.icons[item.icon].class }}"></i>
        {% if column == "homework" %}
        <a href="{{ site.data.info.links.github.link }}/homework-{{ item.text }}-template/">{{ item.text }}</a>
        {% elsif column == "quizzes" %}
        <a href="{{ item.test }}">{{ item.text }}</a>
        {% else %}
        <a href="{{ item.link }}">{{ item.text }}</a>
        {% endif %}
        {% if item.save -%}
        <a href="/files/{{ item.text }}.pdf"><i class="fas fa-download"></i></a>
        {%- endif %}
        {% if item.tube -%}
        <a href="{{ item.tube }}"><i class="fab fa-youtube"></i></a>
        {%- endif %}
      </li>
      {% endfor %}
      {% if filtered.size < 1 %}
      <li type="none">(None)</li>
      {% endif %}
    </ul>
  </td>
  {% endfor %}
</tr>
{% endfor %}

</tbody>
</table>

Note that homework is often associated with multiple topics and appears multiple times in the table.

See the [Schedule](/schedule.html) for links to the many videos and recordings made for this content.

## Example Topics

The following are some example topics that you may want to make sure you understand. This is a non-comprehensive list. Some of these topics may not appear on the exam and some topics not covered here may appear on the exam.

- You should understand how to use the `Pattern` and `Matcher` classes in Java to handle regular expressions, and the difference between the `find()` versus `matches()` methods.

- You should understand how to create **character classes**, such as `[a-z]` and `[^0-9]`, in regular expressions.

- You should understand how to use **predefined character classes** such as `\w`, `\W`, `\s`, `\S`, and `.` in regular expressions.

- You should understand how to use the `?`, `*`, and `+` **quantifiers** in regular expressions.

- You should understand the difference between a **greedy versus reluctant quantifier** in regular expressions.

- You should understand how to create and use **capturing groups** and non-capturing groups in regular expressions.

- You should understand how to use the `i`, `m`, and `s` **flags**.

- You should understand how to use the `^`, `$`, `\A`, `\z`, and `\b` **boundary matchers**.

- You should understand the different ways to use the **`?` character** in a regular expression. For example: `(?i)` to turn on the `i` flag, `(?:i)` to create a non-capturing group that matches the `i` character, `[?!]` to create a character class that matches the `?` and `!` characters, `i?` to match the `i` character 0 or 1 times (greedy), and `i+?` to match the `i` character 1 or more times (reluctant).

<!-- - You should understand how to interpret JUnit test classes, and create your own basic JUnit tests. This includes understanding the `@Test`, `@Nested`, `@BeforeEach`, `@AfterEach`, and `@ParameterizedTest` annotations and the methods in the `org.junit.jupiter.api.Assertions` package.-->

- You should understand how to use **assertions** in Java using the `assert` keyword, and where it is (or is not) appropriate to use.

- You should understand how to **configure and use Log4j2** for logging and debugging. If given a configuration file, you should be able to determine where log messages will go (file or console), which level log messages will be output (all, info, debug, etc.), and in what format.

- You should understand the **pros and cons to multithreading**, and when to use multithreading.

- You should understand the different **states of a thread**, and how methods such as `start()`, `join()`, `wait()`, and `notifyAll()` affect a thread’s state.

- You should understand how to use the **`synchronized` keyword**, and how the object used for the lock affects the number of threads that may enter a code block.

- You should understand how to use a **custom read/write lock** instead of the `synchronized` keyword to protect access to shared data.

- You should understand how to use a **thread pool** and **work queue**, and how they work.

- You should understand how to **create and use worker threads** (using the `Runnable` interface or `Thread` class) versus **how to create and use work** (or tasks, `Runnable` objects) to be used with a work queue.

- You should understand basics about the **Internet versus the Web**, and the different **components of a URL**.

- You should understand how to create and use basic **sockets** to connect to a server.

- You should understand how to create basic **HTTP requests**.

- You should understand and be able to create basic **HTML**.

- You should understand the difference between **static and dynamic web pages**.

- You should understand the basic **client-server architecture** used by Jetty and servlets.

- You should understand how to create dynamic web pages using **Jetty and servlets**.

- You should understand how to create, use, and modify **HTTP cookies** using Jetty.

- You should understand the benefits of **relational databases**.

- You should understand how to create and use statements in the [**Data Definition Language** (DDL)](https://mariadb.com/kb/en/data-definition/) of SQL. This includes the `CREATE`, `ALTER`, and `DROP` statements.

- You should understand how to create and use different **types of columns** in SQL. This includes the `INTEGER`, `TINYINT`, `SMALLINT`, `BIGINT`, `NUMERIC`, `FLOAT`, `DOUBLE`, `CHAR`, `VARCHAR`, `ENUM`, `DATE`, `DATETIME`, and `TIMESTAMP` types.

- You should understand how to use the `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `DEFAULT`, `NOT NULL`, and `AUTO_INCREMENT` **keywords** when creating columns and tables in SQL.

- You should understand how to create and use statements in the [**Data Manipulation Language** (DML)](https://mariadb.com/kb/en/data-manipulation/) of SQL. This includes the [`SELECT`](https://mariadb.com/kb/en/selecting-data/), `INSERT`, `UPDATE`, and `DELETE` statements.

- You should understand how to use different types of [`JOIN` clauses](https://mariadb.com/kb/en/joins/) to **combine results** from multiple related tables in SQL. This includes `INNER JOIN`, `NATURAL JOIN`, `LEFT OUTER JOIN`, `RIGHT OUTER JOIN`, `NATURAL LEFT OUTER JOIN`, and `NATURAL RIGHT OUTER JOIN`.

- You should understand how to **sort results** using the `ORDER BY` clause in a `SELECT` statement.

- You should understand how to **filter results** using the `WHERE` clause in a `SELECT` statement.

- You should understand how to **combine columns** using the `CONCAT()` function and give **columns aliases** using the `AS` clause in a `SELECT` statement.

- You should understand how to **combine rows** using the `GROUP BY` clause (and [aggregate functions](https://mariadb.com/kb/en/aggregate-functions/)) in `SELECT` statement.

- You should understand how to use [**aggregate functions**](https://mariadb.com/kb/en/aggregate-functions/) like `GROUP_CONCAT()`, `COUNT()`, `AVG()`, and `SUM()` in a `SELECT` statement.
