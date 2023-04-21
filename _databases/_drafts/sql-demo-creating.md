---
title: 'SQL Demo: Creating Tables'
navbar: Guides
layout: guides
key: 2.1
---

<style>
table {
  width: auto !important;
}

.content figure {
  text-align: unset;
}
</style>

First, we should figure out what tables we need. In the table above, note the following:

  - Every professor has a first and last name, but not all professors have a middle name.
  - Every professor has a unique email address based off their USF username. This means we can use this username as a primary key for each professor. (This might not be space efficient, however, if we expect this primary key to be repeated in multiple tables.)
  - Only some professors have Twitter accounts. Each Twitter account is unique.
  - Each professor teaches a different number of undergraduate courses.

We do not want to overcomplicate things by creating too many tables, but we also do not want a lot of wasted space in each table. We will strike this balance as follows:

  - For columns that have **one-to-many** relationships, like courses, we will create separate tables.

  - For columns that have **one-to-one** relationships where the columns are likely to be **non-empty**, like first name, last name, and username, we will keep them in the same table.

  - For columns that have a **one-to-one** relationship where the columns are likely to be **empty**, like Twitter username and middle name, we will demonstrate the two options: keeping middle name in the same table, but splitting Twitter usernames to a separate table.

We will need to setup some relationships for these tables, namely that the Twitter and courses tables refer to professors that already exist in the faculty table.

### Create Faculty Table

We will create a `faculty_names` table with 1 row per professor that stores:

  - The USF username (named `usfid`), which must be unique, may not be null, and can be used as the primary key for this table and the foreign key for other tables.

  - The `first`, `middle`, and `last` names in separate columns. The `middle` name may be null. None of these columns need to be unique. For example, more than one person has the first name `David` in the department.

**Exercise:** See if you can create a `faculty_names` table that will match the following:

| Field  | Type        | Null | Key | Default | Extra |
|--------|-------------|------|-----|---------|-------|
| usfid  | char(20)    | NO   | PRI | NULL    |       |
| first  | varchar(20) | NO   |     | NULL    |       |
| middle | varchar(20) | YES  |     | NULL    |       |
| last   | varchar(20) | NO   |     | NULL    |       |

<details>
<summary>See Answer.</summary>

{% highlight sql %}
CREATE TABLE faculty_names (
  usfid   CHAR(20)    NOT NULL PRIMARY KEY,
  first   VARCHAR(20) NOT NULL,
  middle  VARCHAR(20),
  last    VARCHAR(20) NOT NULL
);
{% endhighlight %}

</details><br/>

Once you have the table created, you can insert values as follows:

```sql
INSERT INTO faculty_names
(usfid, first, middle, last)
VALUES
('apjoshi',         'Alark',       NULL, 'Joshi'   ),
('benson',          'Greg',        NULL, 'Benson'  ),
('byuksel',         'Beste',       NULL, 'Yuksel'  ),
('cbrooks',         'Christopher', NULL, 'Brooks'  ),
('dgbrizan',        'David',      'Guy', 'Brizan'  ),
('ejung2',          'EJ',          NULL, 'Jung'    ),
('jajohnson9',      'Jeffrey',     NULL, 'Johnson' ),
('memre',           'Mehmet',      NULL, 'Emre'    ),
('mkremer',         'Michael',     NULL, 'Kremer'  ),
('mmalensek',       'Matthew',     NULL, 'Malensek'),
('okarpenko',       'Olga',        NULL, 'Karpenko'),
('phpeterson',      'Phil',        NULL, 'Peterson'),
('sjengle',         'Sophie',      NULL, 'Engle'   ),
('vpournaghshband', 'Vahab',       NULL, 'Pournaghshband'),
('wolberd',         'David',       NULL, 'Wolber'  );
```

### Create the Twitter Table

We will create a `faculty_twitter` table that tracks Twitter accounts for professors that stores:

  - The Twitter account (named `twitterid`) which must not be null, must be unique, and may be used as the primary key for this table.

  - The ID for the professor this Twitter account belongs to, which must not be null. This should reference the `usfid` primary key in the `faculty_names` table.

**Exercise:** See if you can create a `faculty_twitter` table that will match the following:

| Field     | Type     | Null | Key | Default | Extra |
|-----------|----------|------|-----|---------|-------|
| twitterid | char(15) | NO   | PRI | NULL    |       |
| usfid     | char(20) | NO   | MUL | NULL    |       |

<details>
<summary>See Answer.</summary>

{% highlight sql %}
CREATE TABLE faculty_twitter (
  twitterid   CHAR(15) NOT NULL PRIMARY KEY,
  usfid       CHAR(20) NOT NULL,
  FOREIGN KEY (usfid)
  REFERENCES  faculty_names (usfid)
);
{% endhighlight %}

</details><br/>

Once you have the table created, you can insert values as follows:

```sql
INSERT INTO faculty_twitter
(usfid, twitterid)
VALUES
('apjoshi',   'alark'),
('benson',    'gregorydbenson'),
('byuksel',   'BesteFYuksel'),
('dgbrizan',  'davidguybrizan'),
('mmalensek', 'MatthewMalensek'),
('sjengle',   'sjengle'),
('wolberd',   'wolberd');
```

### Create the Courses Table

We will create a `faculty_courses` table that tracks the undergraduate courses that professors have taught recently. Since courses will not be unique in the same way as Twitter accounts or USF usernames, we need a separate primary key:

  - The course ID (named `courseid`) which must not be null, must be unique, and is an auto-incremented primary key.

  - The ID for the professor that taught this course, which must not be null. This should reference the `usfid` primary key in the `faculty_names` table.

  - The course number (named `course`) to identify the course taught by this professor (e.g. `CS 212`).

**Exercise:** See if you can create a `faculty_courses` table that will match the following:

<details>
<summary>See Answer.</summary>

{% highlight sql %}
CREATE TABLE faculty_courses (
  courseid INTEGER NOT NULL AUTO_INCREMENT PRIMARY KEY,
  usfid    CHAR(20) NOT NULL,
  course   CHAR(10) NOT NULL,
  FOREIGN KEY (usfid)
  REFERENCES  faculty_names (usfid)
);
{% endhighlight %}

</details><br/>

Once you have the table created, you can insert values as follows:

```sql
INSERT INTO faculty_courses
(usfid, course)
VALUES
('apjoshi',    'CS 110'),
('apjoshi',    'CS 112'),
('apjoshi',    'CS 360'),
('benson',     'CS 315'),
('benson',     'CS 326'),
('byuksel',    'CS 107'),
('byuksel',    'CS 110'),
('byuksel',    'CS 490'),
('cbrooks',    'CS 462'),
('dgbrizan',   'CS 245'),
('dgbrizan',   'CS 463'),
('ejung2',     'CS 112'),
('ejung2',     'CS 245'),
('jajohnson9', 'CS 107'),
('jajohnson9', 'CS 110'),
('jajohnson9', 'CS 490'),
('memre',      'CS 345'),
('mkremer',    'CS 110'),
('mmalensek',  'CS 220'),
('mmalensek',  'CS 326'),
('okarpenko',  'CS 112'),
('okarpenko',  'CS 245'),
('okarpenko',  'CS 272'),
('okarpenko',  'CS 490'),
('phpeterson', 'CS 221'),
('phpeterson', 'CS 315'),
('sjengle',    'CS 272'),
('sjengle',    'CS 360'),
('vpournaghshband', 'CS 221'),
('vpournaghshband', 'CS 336'),
('wolberd',    'CS 107'),
('wolberd',    'CS 110'),
('wolberd',    'CS 112');
```

<a href="sql-demo-selecting.html" class="button is-primary"><span>Next: Selecting Data</span>&nbsp;<i class="fas fa-arrow-alt-right"></i></a>
