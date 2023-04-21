---
title: 'SQL Intro: Joining Data'
navbar: Guides
layout: guides
key: 1.2
---

It would be nice if we could see the offices associated with each phone number. A simple way to combine tables is:

```sql
SELECT * FROM offices, phones;
```

```
+----+--------+----+------+----------+-------------+-----------+
| id | office | id | area | phone    | description | office_id |
+----+--------+----+------+----------+-------------+-----------+
|  1 | HPS    |  1 | 415  | 422-5050 | Office      |         3 |
|  2 | SLE    |  1 | 415  | 422-5050 | Office      |         3 |
|  3 | CASA   |  1 | 415  | 422-5050 | Office      |         3 |
|  4 | CAPS   |  1 | 415  | 422-5050 | Office      |         3 |
|  1 | HPS    |  2 | 415  | 422-7256 | Office      |         2 |
|  2 | SLE    |  2 | 415  | 422-7256 | Office      |         2 |
|  3 | CASA   |  2 | 415  | 422-7256 | Office      |         2 |
|  4 | CAPS   |  2 | 415  | 422-7256 | Office      |         2 |
|  1 | HPS    |  3 | 415  | 422-5797 | Office      |         1 |
|  2 | SLE    |  3 | 415  | 422-5797 | Office      |         1 |
...
```
{: .invert }

However, this is not what we want. It performs something similar to a cross product. We could filter out the undesired rows follows:

```sql
SELECT * FROM offices, phones
WHERE offices.id = phones.office_id;
```

We use the `WHERE` constraint to only show those rows where the `id` columns match. The resulting output should be:

```
+----+--------+----+------+----------+-------------+-----------+
| id | office | id | area | phone    | description | office_id |
+----+--------+----+------+----------+-------------+-----------+
|  1 | HPS    |  3 | 415  | 422-5797 | Office      |         1 |
|  1 | HPS    |  4 | 888  | 471-2290 | Fax         |         1 |
|  2 | SLE    |  2 | 415  | 422-7256 | Office      |         2 |
|  3 | CASA   |  1 | 415  | 422-5050 | Office      |         3 |
|  4 | CAPS   |  5 | 415  | 422-6351 | Office      |         4 |
|  4 | CAPS   |  6 | 415  | 422-6352 | Office      |         4 |
|  4 | CAPS   |  7 | 855  | 531-0761 | After Hours |         4 |
+----+--------+----+------+----------+-------------+-----------+
7 rows in set (0.00 sec)
```
{: .invert }

We can even filter by multiple criteria:

```sql
SELECT * FROM offices, phones
WHERE offices.id = phones.office_id
AND description != 'Office';
```

You should see the following output:

```
+----+--------+----+------+----------+-------------+-----------+
| id | office | id | area | phone    | description | office_id |
+----+--------+----+------+----------+-------------+-----------+
|  1 | HPS    |  4 | 888  | 471-2290 | Fax         |         1 |
|  4 | CAPS   |  7 | 855  | 531-0761 | After Hours |         4 |
+----+--------+----+------+----------+-------------+-----------+
2 rows in set (0.00 sec)
```
{: .invert }

### Inner JOINs

Rather than creating a bunch of rows and then filtering out invalid rows, we can use an `INNER JOIN` operation instead on the `office_id` and `id` columns. The statement and the expected output are:

```sql
SELECT * FROM offices
INNER JOIN phones
ON offices.id = phones.office_id;
```

```
+----+--------+----+------+----------+-------------+-----------+
| id | office | id | area | phone    | description | office_id |
+----+--------+----+------+----------+-------------+-----------+
|  1 | HPS    |  3 | 415  | 422-5797 | Office      |         1 |
|  1 | HPS    |  4 | 888  | 471-2290 | Fax         |         1 |
|  2 | SLE    |  2 | 415  | 422-7256 | Office      |         2 |
|  3 | CASA   |  1 | 415  | 422-5050 | Office      |         3 |
|  4 | CAPS   |  5 | 415  | 422-6351 | Office      |         4 |
|  4 | CAPS   |  6 | 415  | 422-6352 | Office      |         4 |
|  4 | CAPS   |  7 | 855  | 531-0761 | After Hours |         4 |
+----+--------+----+------+----------+-------------+-----------+
7 rows in set (0.00 sec)
```
{: .invert }

The order of tables or columns for the `ON` clause does not change the rows, but swapping the order of tables does change the order of the columns:

```sql
SELECT * FROM phones
INNER JOIN offices
ON phones.office_id = offices.id;
```

```
+----+------+----------+-------------+-----------+----+--------+
| id | area | phone    | description | office_id | id | office |
+----+------+----------+-------------+-----------+----+--------+
|  3 | 415  | 422-5797 | Office      |         1 |  1 | HPS    |
|  4 | 888  | 471-2290 | Fax         |         1 |  1 | HPS    |
|  2 | 415  | 422-7256 | Office      |         2 |  2 | SLE    |
|  1 | 415  | 422-5050 | Office      |         3 |  3 | CASA   |
|  5 | 415  | 422-6351 | Office      |         4 |  4 | CAPS   |
|  6 | 415  | 422-6352 | Office      |         4 |  4 | CAPS   |
|  7 | 855  | 531-0761 | After Hours |         4 |  4 | CAPS   |
+----+------+----------+-------------+-----------+----+--------+
7 rows in set (0.00 sec)
```
{: .invert }

We can specify which columns we want displayed:

```sql
SELECT office, area, phone, description FROM offices
INNER JOIN phones
ON offices.id = phones.office_id;
```

```
+--------+------+----------+-------------+
| office | area | phone    | description |
+--------+------+----------+-------------+
| HPS    | 415  | 422-5797 | Office      |
| HPS    | 888  | 471-2290 | Fax         |
| SLE    | 415  | 422-7256 | Office      |
| CASA   | 415  | 422-5050 | Office      |
| CAPS   | 415  | 422-6351 | Office      |
| CAPS   | 415  | 422-6352 | Office      |
| CAPS   | 855  | 531-0761 | After Hours |
+--------+------+----------+-------------+
7 rows in set (0.00 sec)
```
{: .invert }

We can skip the `ON` clause with a `NATURAL JOIN` instead of an `INNER JOIN`:

```sql
SELECT * FROM offices
NATURAL JOIN phones;
```

```
+----+--------+------+----------+-------------+-----------+
| id | office | area | phone    | description | office_id |
+----+--------+------+----------+-------------+-----------+
|  1 | HPS    | 415  | 422-5050 | Office      |         3 |
|  2 | SLE    | 415  | 422-7256 | Office      |         2 |
|  3 | CASA   | 415  | 422-5797 | Office      |         1 |
|  4 | CAPS   | 888  | 471-2290 | Fax         |         1 |
+----+--------+------+----------+-------------+-----------+
4 rows in set (0.00 sec)
```
{: .invert }

Oh no! Notice the numbers are now incorrect. The `NATURAL JOIN` joined on `id` in both tables instead of `office_id`. To make this work, we have to fix our table as follows:

```sql
ALTER TABLE offices CHANGE id office_id INTEGER;
```

Now we can see the new column name:

```sql
DESCRIBE offices;
```

```
+-----------+---------+------+-----+---------+-------+
| Field     | Type    | Null | Key | Default | Extra |
+-----------+---------+------+-----+---------+-------+
| office_id | int(11) | NO   | PRI | NULL    |       |
| office    | char(5) | NO   |     | NULL    |       |
+-----------+---------+------+-----+---------+-------+
2 rows in set (0.01 sec)
```
{: .invert }

And our natural join works as expected:

```sql
SELECT * FROM offices
NATURAL JOIN phones;
```

```
+-----------+--------+----+------+----------+-------------+
| office_id | office | id | area | phone    | description |
+-----------+--------+----+------+----------+-------------+
|         1 | HPS    |  3 | 415  | 422-5797 | Office      |
|         1 | HPS    |  4 | 888  | 471-2290 | Fax         |
|         2 | SLE    |  2 | 415  | 422-7256 | Office      |
|         3 | CASA   |  1 | 415  | 422-5050 | Office      |
|         4 | CAPS   |  5 | 415  | 422-6351 | Office      |
|         4 | CAPS   |  6 | 415  | 422-6352 | Office      |
|         4 | CAPS   |  7 | 855  | 531-0761 | After Hours |
+-----------+--------+----+------+----------+-------------+
```
{: .invert }

### Outer JOINs

There are also `OUTER JOIN`s. Inner joins only include rows where both tables match. An outer join will include all rows from the outer table, and show the matches from the other table. If there is not a match, you will see a `NULL` value.

We will need to add a couple of values to our tables to demonstrate this:

```sql
INSERT INTO offices VALUES (5, 'LWSC');
INSERT INTO phones VALUES (8, '555', '422-5555', null, 6);
```

Now, we can see the difference with an outer join:

```sql
SELECT * FROM offices
LEFT OUTER JOIN phones
ON offices.office_id = phones.office_id;
```

```
+-----------+--------+------+------+----------+-------------+-----------+
| office_id | office | id   | area | phone    | description | office_id |
+-----------+--------+------+------+----------+-------------+-----------+
|         3 | CASA   |    1 | 415  | 422-5050 | Office      |         3 |
|         2 | SLE    |    2 | 415  | 422-7256 | Office      |         2 |
|         1 | HPS    |    3 | 415  | 422-5797 | Office      |         1 |
|         1 | HPS    |    4 | 888  | 471-2290 | Fax         |         1 |
|         4 | CAPS   |    5 | 415  | 422-6351 | Office      |         4 |
|         4 | CAPS   |    6 | 415  | 422-6352 | Office      |         4 |
|         4 | CAPS   |    7 | 855  | 531-0761 | After Hours |         4 |
|         5 | LWSC   | NULL | NULL | NULL     | NULL        |      NULL |
+-----------+--------+------+------+----------+-------------+-----------+
8 rows in set (0.00 sec)
```
{: .invert }

And, of course, we can make those joins `NATURAL` to save some typing:

```sql
SELECT * FROM offices
NATURAL LEFT OUTER JOIN phones;
```

```
+-----------+--------+------+------+----------+-------------+
| office_id | office | id   | area | phone    | description |
+-----------+--------+------+------+----------+-------------+
|         3 | CASA   |    1 | 415  | 422-5050 | Office      |
|         2 | SLE    |    2 | 415  | 422-7256 | Office      |
|         1 | HPS    |    3 | 415  | 422-5797 | Office      |
|         1 | HPS    |    4 | 888  | 471-2290 | Fax         |
|         4 | CAPS   |    5 | 415  | 422-6351 | Office      |
|         4 | CAPS   |    6 | 415  | 422-6352 | Office      |
|         4 | CAPS   |    7 | 855  | 531-0761 | After Hours |
|         5 | LWSC   | NULL | NULL | NULL     | NULL        |
+-----------+--------+------+------+----------+-------------+
```
{: .invert }

There is a `RIGHT OUTER JOIN` too. The direction indicates which table all of the rows will be taken from first, and then joined into:

```sql
SELECT * FROM offices
NATURAL RIGHT OUTER JOIN phones;
```

```
+-----------+----+------+----------+-------------+--------+
| office_id | id | area | phone    | description | office |
+-----------+----+------+----------+-------------+--------+
|         3 |  1 | 415  | 422-5050 | Office      | CASA   |
|         2 |  2 | 415  | 422-7256 | Office      | SLE    |
|         1 |  3 | 415  | 422-5797 | Office      | HPS    |
|         1 |  4 | 888  | 471-2290 | Fax         | HPS    |
|         4 |  5 | 415  | 422-6351 | Office      | CAPS   |
|         4 |  6 | 415  | 422-6352 | Office      | CAPS   |
|         4 |  7 | 855  | 531-0761 | After Hours | CAPS   |
|         6 |  8 | 555  | 422-5555 | NULL        | NULL   |
+-----------+----+------+----------+-------------+--------+
8 rows in set (0.00 sec)
```
{: .invert }

We can get a full outer join using the `UNION` operation:

```sql
SELECT * FROM offices
LEFT OUTER JOIN phones
ON offices.office_id = phones.office_id
UNION
SELECT * FROM offices
RIGHT OUTER JOIN phones
ON phones.office_id = offices.office_id;
```

```
+-----------+--------+------+------+----------+-------------+-----------+
| office_id | office | id   | area | phone    | description | office_id |
+-----------+--------+------+------+----------+-------------+-----------+
|         3 | CASA   |    1 | 415  | 422-5050 | Office      |         3 |
|         2 | SLE    |    2 | 415  | 422-7256 | Office      |         2 |
|         1 | HPS    |    3 | 415  | 422-5797 | Office      |         1 |
|         1 | HPS    |    4 | 888  | 471-2290 | Fax         |         1 |
|         4 | CAPS   |    5 | 415  | 422-6351 | Office      |         4 |
|         4 | CAPS   |    6 | 415  | 422-6352 | Office      |         4 |
|         4 | CAPS   |    7 | 855  | 531-0761 | After Hours |         4 |
|         5 | LWSC   | NULL | NULL | NULL     | NULL        |      NULL |
|      NULL | NULL   |    8 | 555  | 422-5555 | NULL        |         6 |
+-----------+--------+------+------+----------+-------------+-----------+
9 rows in set (0.01 sec)
```
{: .invert }

### Functions

There are other operations, like `CONCAT`:

```sql
SELECT office,
CONCAT('(', area, ') ', phone)
FROM offices
NATURAL LEFT OUTER JOIN phones
ORDER BY office;
```

The results are as follows. Note the column names, which will be how we access results later when we connect this to Java.

```
+--------+--------------------------------+
| office | CONCAT('(', area, ') ', phone) |
+--------+--------------------------------+
| CAPS   | (855) 531-0761                 |
| CAPS   | (415) 422-6351                 |
| CAPS   | (415) 422-6352                 |
| CASA   | (415) 422-5050                 |
| HPS    | (415) 422-5797                 |
| HPS    | (888) 471-2290                 |
| LWSC   | NULL                           |
| SLE    | (415) 422-7256                 |
+--------+--------------------------------+
```
{: .invert }

For a better column name, use the `AS` clause:

```sql
SELECT office,
CONCAT('(', area, ') ', phone) AS 'phone'
FROM offices
NATURAL LEFT OUTER JOIN phones
ORDER BY office;
```

```
+--------+----------------+
| office | phone          |
+--------+----------------+
| CAPS   | (855) 531-0761 |
| CAPS   | (415) 422-6351 |
| CAPS   | (415) 422-6352 |
| CASA   | (415) 422-5050 |
| HPS    | (415) 422-5797 |
| HPS    | (888) 471-2290 |
| LWSC   | NULL           |
| SLE    | (415) 422-7256 |
+--------+----------------+
8 rows in set (0.00 sec)
```
{: .invert }

We can also use aggregate functions in combination with `GROUP BY` to do some interesting things:

```sql
SELECT office,
COUNT(phone) AS 'phone numbers'
FROM offices
NATURAL LEFT OUTER JOIN phones
GROUP BY office_id;
```

```
+--------+---------------+
| office | phone numbers |
+--------+---------------+
| HPS    |             2 |
| SLE    |             1 |
| CASA   |             1 |
| CAPS   |             3 |
| LWSC   |             0 |
+--------+---------------+
```
{: .invert }

This is especially useful for calculating averages, maximums, etc.

<a href="sql-intro-enforcing.html" class="button is-primary"><span>Next: Enforcing Relationships</span>&nbsp;<i class="fas fa-arrow-alt-right"></i></a>
