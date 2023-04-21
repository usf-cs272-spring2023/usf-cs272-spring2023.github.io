---
title: 'SQL Intro: Enforcing Relationships'
navbar: Guides
layout: guides
key: 1.3
---

But wait, there is something weird going on in our tables now:

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
|  8 | 555  | 422-5555 | NULL        |         6 |
+----+------+----------+-------------+-----------+
8 rows in set (0.00 sec)
```
{: .invert }

Do we have an office with id `6` in our table?

```sql
SELECT * FROM offices WHERE office_id=6;
```

```
Empty set (0.00 sec)
```
{: .invert }

Why did our database let us add an entry for a non-existent office? For now, lets get rid of the invalid data:

```sql
DELETE FROM phones WHERE office_id=6;
```

```
Query OK, 1 row affected (0.06 sec)
```
{: .invert }

So, if we want our database to enforce relationships between our table we need to make sure we are using a database engine that enforces foreign key constraints, like `InnoDB`. Let’s fix our tables so this works, starting with demo_users. To see the details about this table, use the following command:

```sql
SHOW CREATE TABLE offices;
```

You should see something similar to:

```
+---------+--------------------------+
| Table   | Create Table             |
+---------+--------------------------+
| offices | CREATE TABLE `offices` (
  `office_id` int(11) NOT NULL,
  `office` char(5) NOT NULL,
  PRIMARY KEY (`office_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
+---------+--------------------------+
1 row in set (0.00 sec)
```
{: .invert }

If it isn’t using InnoDB as the ENGINE, then run the following commands:

```sql
ALTER TABLE offices ENGINE=InnoDB;
ALTER TABLE phones ENGINE=InnoDB;
```

Next, we have to add in the `FOREIGN KEY` constraint:

```sql
ALTER TABLE phones
ADD FOREIGN KEY (office_id)
REFERENCES offices(office_id);
```

Now, when we try to add a row with a non existent user, we get an error. Try it out:

```sql
INSERT INTO phones VALUES
(8, '555', '422-5555', null, 6);
```

```
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`user100`.`phones`, CONSTRAINT `phones_ibfk_1` FOREIGN KEY (`office_id`) REFERENCES `offices` (`office_id`))
```
{: .invert }

That is the entire example! Now that we are done with it, you might want to do some cleanup of your database. To remove the tables, do:

```sql
DROP TABLE phones, offices;
SHOW TABLES;
```

```
Empty set (0.01 sec)
```
{: .invert }

If you want to load this entire example all at once, use:

```sql
SOURCE /home/public/cs272/sql/intro.sql
```
