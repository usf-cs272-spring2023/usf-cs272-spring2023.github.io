---
title: 'SQL Intro: Getting Started'
navbar: Guides
layout: guides
key: 1.0
bump: true

tags:
  - text: 'New'
    type: 'is-primary'
---

This demo will introduce various basic SQL concepts and statements via example. The tables we will be building in this demo are:

| Office | Phone Number   | Description |
|:-------|----------------|:------------|
| CASA   | (415) 422-5050 | Office |
| SLE    | (415) 422-7256 | Office |
| HPS    | (415) 422-5797 | Office |
| HPS    | (888) 471-2290 | Fax |
| CAPS   | (415) 422-6351 | Office |
| CAPS   | (415) 422-6352 | Office |
| CAPS   | (855) 531-0761 | After Hours |
{: .table .is-hoverable .is-auto }

### Database Design

Notice how an office could have between 1 to 3 numbers associated with it? That is a "one to many" (one office to many phone numbers) relationship.

We will often split those values into separate tables so we can have exactly 1 row per item (one row per office in an `offices` table, and one row per number in an `phones` table).

The first table `offices` will capture the unique offices:

| office_id | office |
|:---------:|:-------|
| 1 | HPS |
| 2 | SLE |
| 3 | CASA |
| 4 | CAPS |
{: .table .is-hoverable .is-auto }

And then the next table `phones` will capture the office numbers:

| phone_id | area | phone   | description | office_id |
|:---:|:----:|:---------|:------------|:---:|
| 1   | 415  | 422-5050 | Office      | 3   |
| 2   | 415  | 422-7256 | Office      | 2   |
| 3   | 415  | 422-5797 | Office      | 1   |
| 4   | 888  | 471-2290 | Fax         | 1   |
| 5   | 415  | 422-6351 | Office      | 4   |
| 6   | 415  | 422-6352 | Office      | 4   |
| 7   | 855  | 531-0761 | After Hours | 4   |
{: .table .is-hoverable .is-auto }

The exact tables will depend on which point you are at in the demo script.

<a href="sql-intro-creating.html" class="button is-primary"><span>Next: Creating Tables</span>&nbsp;<i class="fas fa-arrow-alt-right"></i></a>
