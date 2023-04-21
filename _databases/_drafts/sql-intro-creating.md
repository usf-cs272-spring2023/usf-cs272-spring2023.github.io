---
title: 'SQL Intro: Creating Tables'
navbar: Guides
layout: guides
key: 1.1
---

Now, we can start creating our tables. Start by creating a `offices` table. Each office should have a unique id, which will be the primary key of the table. In this case, we will just automatically increment the id value every time we insert into the table. Copy/paste the following at the prompt:

```sql
CREATE TABLE offices (
  id INTEGER NOT NULL AUTO_INCREMENT PRIMARY KEY,
  office CHAR(5) NOT NULL
);
```

You will see output similar to the snippet below. Notice that the `mysql>` text is the actual prompt, and the `->` symbol indicates a multi-line command and appears automatically after you press <kbd>Enter</kbd>. You will also get a status message after each command. In this case, we see that the query was okay. If there was an issue, you’ll see an error instead.

```
mysql> CREATE TABLE offices (
    ->   id INTEGER NOT NULL AUTO_INCREMENT PRIMARY KEY,
    ->   office CHAR(5) NOT NULL
    -> );
Query OK, 0 rows affected (0.02 sec)
```
{: .invert }

Verify that the `offices` table was created correctly with the `SHOW TABLES` and `DESCRIBE` statements. Below gives both the statements and the expected output, so make sure not to copy/paste the output into the `mysql` prompt:

```sql
SHOW TABLES;
```

```
+-------------------+
| Tables_in_user100 |
+-------------------+
| offices           |
+-------------------+
```
{: .invert }

```sql
DESCRIBE offices;
```

```
+--------+---------+------+-----+---------+----------------+
| Field  | Type    | Null | Key | Default | Extra          |
+--------+---------+------+-----+---------+----------------+
| id     | int(11) | NO   | PRI | NULL    | auto_increment |
| office | char(5) | NO   |     | NULL    |                |
+--------+---------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```
{: .invert }

If all looks good, then we can start inserting values into our table. We can insert multiple values at once using the following syntax:

```sql
INSERT INTO offices
(office)
VALUES ('HPS'), ('SLE'), ('CASA'), ('CAPS');
```

To see all of the rows you entered into your table, use the `SELECT` statement:

```sql
SELECT * FROM offices;
```

```
+----+--------+
| id | office |
+----+--------+
|  1 | HPS    |
|  2 | SLE    |
|  3 | CASA   |
|  4 | CAPS   |
+----+--------+
4 rows in set (0.00 sec)
```
{: .invert }

To sort the rows, use the `ORDER BY` clause:

```sql
SELECT * FROM offices ORDER BY office ASC;
```

```
+----+--------+
| id | office |
+----+--------+
|  4 | CAPS   |
|  3 | CASA   |
|  1 | HPS    |
|  2 | SLE    |
+----+--------+
4 rows in set (0.00 sec)
```
{: .invert }

Use the `DESC` keyword if you want to sort in descending order instead.

Now, create the `phones` table:

```sql
CREATE TABLE phones (
  id INTEGER NOT NULL AUTO_INCREMENT PRIMARY KEY,
  area CHAR(3) NOT NULL DEFAULT '415',
  phone CHAR(8) NOT NULL,
  description VARCHAR(15),
  office_id INTEGER NOT NULL
);
```

Make sure everything looks right. You can combine two statements together as long as you have the `;` semi-colons in the right places:

```sql
SHOW TABLES; DESCRIBE phones;
```

```
+-------------------+
| Tables_in_user100 |
+-------------------+
| offices           |
| phones            |
+-------------------+
2 rows in set (0.00 sec)

+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| area        | char(3)     | NO   |     | 415     |                |
| phone       | char(8)     | NO   |     | NULL    |                |
| description | varchar(15) | YES  |     | NULL    |                |
| office_id   | int(11)     | NO   |     | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)
```
{: .invert }

Now, time to insert a row into this table. This time, lets insert 1 row at a time:

```sql
INSERT INTO phones
(area, phone, description, office_id)
VALUES ('415', '422-5050', 'Office', 3);
```

When inserting, you can mix up the order of the columns:

```sql
INSERT INTO phones
(office_id, area, phone, description)
VALUES (2, '415', '422-7256', 'Office');
```

When inserting, you can take advantage of the default value for area:

```sql
INSERT INTO phones
(phone, description, office_id)
VALUES ('422-5797', 'Office', 1);
```

You can verify the default value did get set correctly by looking at only the last row:

```sql
SELECT * FROM phones WHERE id=3;
```

```
+----+------+----------+-------------+-----------+
| id | area | phone    | description | office_id |
+----+------+----------+-------------+-----------+
|  3 | 415  | 422-5797 | Office      |         1 |
+----+------+----------+-------------+-----------+
1 row in set (0.00 sec)
```
{: .invert }

You can also insert a `NULL` into the description column:

```sql
INSERT INTO phones
(area, phone, description, office_id)
VALUES ('888', '471-2290', null, 1);
```

You can skip specifying the columns, but then you have to provide all columns (including ones that are `AUTO_INCREMENT`, may be `NULL`, or have default values):

```sql
INSERT INTO phones
VALUES (5, '415', '422-6351', 'Office', 4);
```

Lets make sure everything looks right so far:

```sql
SELECT * FROM phones;
```

```
+----+------+----------+-------------+-----------+
| id | area | phone    | description | office_id |
+----+------+----------+-------------+-----------+
|  1 | 415  | 422-5050 | Office      |         3 |
|  2 | 415  | 422-7256 | Office      |         2 |
|  3 | 415  | 422-5797 | Office      |         1 |
|  4 | 888  | 471-2290 | NULL        |         1 |
|  5 | 415  | 422-6351 | Office      |         4 |
+----+------+----------+-------------+-----------+
5 rows in set (0.00 sec)
```
{: .invert }

Actually, lets get rid of that `NULL` value. We can update a row as follows:

```sql
UPDATE phones
SET description='Fax'
WHERE ISNULL(description);
```

You should see the following messages, indicating 1 row was changed:

```
Query OK, 1 row affected (0.02 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```
{: .invert }

Now to double-check our work:

```sql
SELECT * FROM phones WHERE id=4;
```

```
+----+------+----------+-------------+-----------+
| id | area | phone    | description | office_id |
+----+------+----------+-------------+-----------+
|  4 | 888  | 471-2290 | Fax         |         1 |
+----+------+----------+-------------+-----------+
1 row in set (0.00 sec)
```
{: .invert }

We have a couple more numbers to add to complete this example, which we can do as follows:

```sql
INSERT INTO phones
VALUES
(6, '415', '422-6352', 'Office', 4),
(7, '855', '531-0761', 'After Hours', 4);
```

Finally, we have a complete `phones` table:

```sql
SELECT * FROM phones;
```

```
+----+------+----------+-------------+-----------+
| id | area | phone    | description | office_id |
+----+------+----------+-------------+-----------+
|  1 | 415  | 422-5050 | Office      |         3 |
|  2 | 415  | 422-7256 | Office      |         2 |
|  3 | 415  | 422-5797 | Office      |         1 |
|  4 | 888  | 471-2290 | Fax         |         1 |
|  5 | 415  | 422-6351 | Office      |         4 |
|  6 | 415  | 422-6352 | Office      |         4 |
|  7 | 855  | 531-0761 | After Hours |         4 |
+----+------+----------+-------------+-----------+
7 rows in set (0.00 sec)
```
{: .invert }

<a href="sql-intro-joining.html" class="button is-primary"><span>Next: Joining Data</span>&nbsp;<i class="fas fa-arrow-alt-right"></i></a>
