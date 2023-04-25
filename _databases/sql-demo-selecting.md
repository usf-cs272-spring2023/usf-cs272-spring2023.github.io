---
title: 'SQL Demo: Selecting Data'
navbar: Resources
layout: resources
key: 2.2
---

<style>
table {
  width: auto !important;
}

.content figure {
  text-align: unset;
}
</style>

We will work up to the primary table we want. Lets start with something a little bit less complex first. When working on these, keep in mind there are many ways to write equivalent `SELECT` statements, so your solution may be different from the one given.

### Names and Email Table

See if you can re-create the following table with a `SELECT` statement:

| name                 | email                     |
|----------------------|---------------------------|
| Greg D. Benson       | benson@usfca.edu          |
| David Guy Brizan     | dgbrizan@usfca.edu        |
| Christopher Brooks   | cbrooks@usfca.edu         |
| Mehmet Emre          | memre@usfca.edu           |
| Sophie J. Engle      | sjengle@usfca.edu         |
| Alark Joshi          | apjoshi@usfca.edu         |
| EJ Jung              | ejung2@usfca.edu          |
| Olga Karpenko        | okarpenko@usfca.edu       |
| Michael C. Kremer    | mkremer@usfca.edu         |
| Matthew Malensek     | mmalensek@usfca.edu       |
| Phil Peterson        | phpeterson@usfca.edu      |
| Vahab Pournaghshband | vpournaghshband@usfca.edu |
| Kelsey Urgo          | kurgo@usfca.edu           |
| Ellen Veomett        | eveomett@usfca.edu        |
| David Wolber         | wolberd@usfca.edu         |
| Beste F. Yuksel      | byuksel@usfca.edu         |

<details>
<summary>See Answer.</summary>

{% highlight sql %}
SELECT
CONCAT(
  first, ' ', 
  IFNULL(CONCAT(middle, '. '), ''), 
  last) AS 'name',
CONCAT(usfid, '@usfca.edu') AS 'email'
FROM faculty_names
ORDER BY faculty_names.last;
{% endhighlight %}
</details><br/>

Now we can start working on some `JOIN` statements.

### Twitter Table

See if you can re-create the following table with a `SELECT` statement:

| name             | twitter          |
|------------------|------------------|
| Greg D. Benson   | @gregorydbenson  |
| David Guy Brizan | @davidguybrizan  |
| Mehmet Emre      | @maemre          |
| Sophie J. Engle  | @sjengle         |
| Alark Joshi      | @alark           |
| Matthew Malensek | @MatthewMalensek |
| David Wolber     | @wolberd         |
| Beste F. Yuksel  | @BesteFYuksel    |

<details>
<summary>See Answer.</summary>

{% highlight sql %}
SELECT
CONCAT(first, ' ', 
  IFNULL(CONCAT(middle, '. '), ''), 
  last) AS 'name',
CONCAT('@', twitterid) AS 'twitter'
FROM faculty_names
NATURAL JOIN faculty_twitter
ORDER BY faculty_names.last;
{% endhighlight %}
</details><br/>

### Courses Table

Next, lets see if we can create a list of courses and who teaches them. It is easier to start by just counting the number of people that teach each course:

| course | professors |
|--------|------------|
| CS 107 |          2 |
| CS 110 |          5 |
| CS 112 |          4 |
| CS 186 |          1 |
| CS 220 |          1 |
| CS 221 |          3 |
| CS 245 |          4 |
| CS 272 |          3 |
| CS 315 |          2 |
| CS 326 |          2 |
| CS 336 |          1 |
| CS 345 |          1 |
| CS 360 |          2 |
| CS 386 |          1 |
| CS 414 |          1 |
| CS 462 |          1 |
| CS 463 |          1 |
| CS 480 |          1 |
| CS 486 |          1 |
| CS 490 |          2 |

<details>
<summary>See Answer.</summary>

{% highlight sql %}
SELECT
course,
COUNT(DISTINCT(usfid)) AS 'professors'
FROM faculty_courses
NATURAL LEFT OUTER JOIN faculty_names
GROUP BY course
ORDER BY course, last;
{% endhighlight %}
</details><br/>

Now, we can move to the full table:

| course | professors                                                                 |
|--------|----------------------------------------------------------------------------|
| CS 107 | David Wolber, Beste F. Yuksel                                              |
| CS 110 | Alark Joshi, Michael C. Kremer, Kelsey Urgo, David Wolber, Beste F. Yuksel |
| CS 112 | Alark Joshi, EJ Jung, Olga Karpenko, David Wolber                          |
| CS 186 | Sophie J. Engle                                                            |
| CS 220 | Matthew Malensek                                                           |
| CS 221 | EJ Jung, Phil Peterson, Vahab Pournaghshband                               |
| CS 245 | David Guy Brizan, EJ Jung, Olga Karpenko, Ellen Veomett                    |
| CS 272 | Sophie J. Engle, Olga Karpenko, Phil Peterson                              |
| CS 315 | Greg D. Benson, Phil Peterson                                              |
| CS 326 | Greg D. Benson, Matthew Malensek                                           |
| CS 336 | Vahab Pournaghshband                                                       |
| CS 345 | Mehmet Emre                                                                |
| CS 360 | Sophie J. Engle, Alark Joshi                                               |
| CS 386 | Michael C. Kremer                                                          |
| CS 414 | Mehmet Emre                                                                |
| CS 462 | Christopher Brooks                                                         |
| CS 463 | David Guy Brizan                                                           |
| CS 480 | Alark Joshi                                                                |
| CS 486 | Kelsey Urgo                                                                |
| CS 490 | Olga Karpenko, Beste F. Yuksel                                             |

<details>
<summary>See Answer.</summary>

{% highlight sql %}
SELECT
course,
GROUP_CONCAT(
  CONCAT(first, ' ', 
    IFNULL(CONCAT(middle, '. '), ''), 
    last)
  ORDER BY last
  SEPARATOR ', '
) AS 'professors'
FROM faculty_courses
NATURAL LEFT OUTER JOIN faculty_names
GROUP BY course
ORDER BY course, last;
{% endhighlight %}
</details><br/>

### Faculty Table

Next, we can try and combine everything together. As before, I suggest you first work on a query that just gives counts:

| name                 | email                     | twitter | courses |
|----------------------|---------------------------|---------|---------|
| Greg D. Benson       | benson@usfca.edu          |       1 |       2 |
| David Guy Brizan     | dgbrizan@usfca.edu        |       1 |       2 |
| Christopher Brooks   | cbrooks@usfca.edu         |       0 |       1 |
| Mehmet Emre          | memre@usfca.edu           |       1 |       2 |
| Sophie J. Engle      | sjengle@usfca.edu         |       1 |       3 |
| Alark Joshi          | apjoshi@usfca.edu         |       1 |       4 |
| EJ Jung              | ejung2@usfca.edu          |       0 |       3 |
| Olga Karpenko        | okarpenko@usfca.edu       |       0 |       4 |
| Michael C. Kremer    | mkremer@usfca.edu         |       0 |       2 |
| Matthew Malensek     | mmalensek@usfca.edu       |       1 |       2 |
| Phil Peterson        | phpeterson@usfca.edu      |       0 |       3 |
| Vahab Pournaghshband | vpournaghshband@usfca.edu |       0 |       2 |
| Kelsey Urgo          | kurgo@usfca.edu           |       0 |       2 |
| Ellen Veomett        | eveomett@usfca.edu        |       0 |       1 |
| David Wolber         | wolberd@usfca.edu         |       1 |       3 |
| Beste F. Yuksel      | byuksel@usfca.edu         |       1 |       3 |

<details>
<summary>See Answer.</summary>

{% highlight sql %}
SELECT
CONCAT(first, ' ', IFNULL(CONCAT(middle, '. '), ''), last) AS 'name',
CONCAT(usfid, '@usfca.edu') AS 'email',
COUNT(DISTINCT(twitterid)) AS 'twitter',
COUNT(DISTINCT(course)) AS 'courses'
FROM faculty_names
NATURAL LEFT OUTER JOIN faculty_twitter
NATURAL LEFT OUTER JOIN faculty_courses
GROUP BY faculty_names.usfid
ORDER BY faculty_names.last;
{% endhighlight %}
</details><br/>

Now, we can work on creating the final version of our output:

| name                 | email                     | twitter          | courses                        |
|----------------------|---------------------------|------------------|--------------------------------|
| Greg D. Benson       | benson@usfca.edu          | @gregorydbenson  | CS 315, CS 326                 |
| David Guy Brizan     | dgbrizan@usfca.edu        | @davidguybrizan  | CS 245, CS 463                 |
| Christopher Brooks   | cbrooks@usfca.edu         |                  | CS 462                         |
| Mehmet Emre          | memre@usfca.edu           | @maemre          | CS 345, CS 414                 |
| Sophie J. Engle      | sjengle@usfca.edu         | @sjengle         | CS 186, CS 272, CS 360         |
| Alark Joshi          | apjoshi@usfca.edu         | @alark           | CS 110, CS 112, CS 360, CS 480 |
| EJ Jung              | ejung2@usfca.edu          |                  | CS 112, CS 221, CS 245         |
| Olga Karpenko        | okarpenko@usfca.edu       |                  | CS 112, CS 245, CS 272, CS 490 |
| Michael C. Kremer    | mkremer@usfca.edu         |                  | CS 110, CS 386                 |
| Matthew Malensek     | mmalensek@usfca.edu       | @MatthewMalensek | CS 220, CS 326                 |
| Phil Peterson        | phpeterson@usfca.edu      |                  | CS 221, CS 272, CS 315         |
| Vahab Pournaghshband | vpournaghshband@usfca.edu |                  | CS 221, CS 336                 |
| Kelsey Urgo          | kurgo@usfca.edu           |                  | CS 110, CS 486                 |
| Ellen Veomett        | eveomett@usfca.edu        |                  | CS 245                         |
| David Wolber         | wolberd@usfca.edu         | @wolberd         | CS 107, CS 110, CS 112         |
| Beste F. Yuksel      | byuksel@usfca.edu         | @BesteFYuksel    | CS 107, CS 110, CS 490         |

<details>
<summary>See Answer.</summary>

{% highlight sql %}
SELECT
CONCAT(first, ' ', IFNULL(CONCAT(middle, '. '), ''), last) AS 'name',
CONCAT(usfid, '@usfca.edu') AS 'email',
IFNULL(CONCAT('@', twitterid), '') AS 'twitter',
GROUP_CONCAT(course ORDER BY course SEPARATOR ', ') AS 'courses'
FROM faculty_names
NATURAL LEFT OUTER JOIN faculty_twitter
NATURAL LEFT OUTER JOIN faculty_courses
GROUP BY faculty_names.usfid
ORDER BY faculty_names.last;
{% endhighlight %}
</details>
