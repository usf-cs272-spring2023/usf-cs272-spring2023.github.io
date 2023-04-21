---
title: 'SQL Database Accounts'
navbar: Resources
layout: resources
key: 0.1
bump: true

tags:
  - text: 'New'
    type: 'is-primary'
---

We will use [MariaDB](https://mariadb.org/) as the relational database for this course. MariaDB is an open-source fork of MySQL and we will use the `mysql` client to connect to this database from a CS lab computer.

The [knowledge base](https://mariadb.org/documentation/) for MariaDB has a lot of excellent resources. This includes several [training and tutorials](https://mariadb.com/kb/en/training-tutorials/), including:

  - [Introduction to Relational Databases](https://mariadb.com/kb/en/introduction-to-relational-databases/)
  - [MariaDB Primer](https://mariadb.com/kb/en/a-mariadb-primer/)

And several useful references, including:

  - [SQL Statements](https://mariadb.com/kb/en/sql-statements/)
  - [Builtin Functions](https://mariadb.com/kb/en/built-in-functions/)

### Database Accounts

Everyone has been assigned a database account on our lab MySQL/MariaDB database server.

The database accounts are in the form `user###` where `###` is a number between `010` and `050`. The database for that account is the same as the username.

See which database account you have been assigned on the [Database Accounts]({{ site.data.info.links.canvas.link }}/pages/database-accounts) Canvas page. Your initial password will be your USF username. **Make sure to change your password after logging in for the first time.**

### Connecting to Database

First, log into one of the CS lab computers. See the [Using CS Lab Computers](/guides/general/using-cs-lab-computers.html) guide for details.

Then, you will need to run the `mysql` command to connect to the SQL database on campus. The command looks like:

<input type="text" class="input is-expanded is-family-code" value="mysql -h sql.cs.usfca.edu -u user### -D user### -p"/>

*Hint: You can edit the command above to use your `user###` database account and then copy/paste.*

Here is what each part means:

  - The `mysql` part runs the MySQL command-line tool.

  - The `-h sql.cs.usfca.edu` specifies the hostname of our database server. You can actually type `-h sql` and the `cs.usfca.edu` part will be assumed since you are on the CS lab network.

  - The `-u user###` part will specify the database account username. This is **different** from your CS or USF usernames. See the [Database Accounts]({{ site.data.info.links.canvas.link }}/pages/database-accounts) page for your assigned database account and replace `user###` with your database username.

  - The `-D user###` part specifies the specific database to use. You only have one database for this class, and it has the same name as your database account username.

  - The `-p` (or `--password`) at the end will cause `mysql` to prompt you for a password once you hit <kbd>Enter</kbd>. Remember, you will not see the characters you type for your password for privacy reasons.

### Source Commands

You may want to load some of the examples discussed in class into your own database. This can be done using the [`SOURCE`](https://mariadb.com/kb/en/mysql-command-line-client/) command in the `mysql` client.

#### Introduction Example

```sql
SOURCE /home/public/cs272/sql/intro.sql
```

#### Faculty Example

```sql
SOURCE /home/public/cs272/sql/faculty.sql
```
