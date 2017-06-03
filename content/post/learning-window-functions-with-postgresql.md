+++
date = "2017-06-03T03:15:00-07:00"
title = "Learning window functions with postgresql"
tags = ["sql", "postgresql"]

+++

## Objective
Learn what [window functions](https://www.postgresql.org/docs/9.1/static/tutorial-window.html) are and how they are used.

This example uses the [fake messaging app playgroung](https://github.com/chischaschos/fake-messaging-app-pg). What we are going to do is to get the messages from groups a user is part of, only the top 3 messages of each group sorted by creation date are returned.

This blog is written using commit [cf971929380a91730dffb9b87177f664bd87aa1a](https://github.com/chischaschos/fake-messaging-app-pg/commit/cf971929380a91730dffb9b87177f664bd87aa1a) of the fake messaging app playground.

## An example use case
What we are going to do is **to obtain the latest 3 messages a user has from each group**.  To start let's build the database with `create-myapp-db.sh`:
```bash
➜  fake-messaging-app git:(master) ./create-myapp-db.sh
CREATE TABLE
Our base user #<OpenStruct id="1", name="Conner Cummings">
1, 2
2, 5
3, 10
4, 17
5, 26
6, 37
7, 50
8, 65
9, 82
10, 101
11, 122
12, 145
13, 170
14, 197
15, 226
16, 257
17, 290
18, 325
19, 362
20, 401
```

Now let's connect to the db:
```bash
➜  fake-messaging-app git:(master) psql --dbname fake_messaging_app --username postgres
psql (9.6.2)
Type "help" for help.

fake_messaging_app=>
```

Let's get all the messages for group 2 of size 5:
```sql
SELECT id, created_at, row_number() OVER (
  partition BY group_id ORDER BY created_at DESC)
FROM messages
WHERE group_id = 2;
```
```bash
 id |     created_at      | row_number
----+---------------------+------------
  7 | 2017-01-01 00:00:04 |          1
  6 | 2017-01-01 00:00:03 |          2
  5 | 2017-01-01 00:00:02 |          3
  4 | 2017-01-01 00:00:01 |          4
  3 | 2017-01-01 00:00:00 |          5
(5 rows)
```

```sql
... row_number() OVER (PARTITION BY group_id ORDER BY created_at DESC) ...
```

> NOTE: Window functions are permitted only in the SELECT list and the ORDER BY clause of the query. They are forbidden elsewhere, such as in GROUP BY, HAVING and WHERE clauses.

Now only pick 3:
```sql
SELECT *
FROM (
  SELECT id, created_at, row_number() OVER (
    partition BY group_id ORDER BY created_at DESC)
  FROM messages
  WHERE group_id = 2) t1
WHERE row_number <= 3;
```
```bash
 id |     created_at      | row_number
----+---------------------+------------
  7 | 2017-01-01 00:00:04 |          1
  6 | 2017-01-01 00:00:03 |          2
  5 | 2017-01-01 00:00:02 |          3
(3 rows)
```

And now do it for all the groups from one specific user:
```sql
SELECT group_id, id AS message_id, user_id, created_at, row_number
FROM (
  SELECT m.id, m.user_id, m.message, m.created_at, m.group_id, row_number() OVER (
    partition BY m.group_id ORDER by m.created_at desc)
  FROM messages m
  INNER JOIN users_groups ug USING(group_id)
  WHERE ug.member_id = 1) t1
WHERE row_number <= 3;
```
```bash
----------+------------+---------+---------------------+------------
        1 |          2 |     626 | 2017-01-01 00:00:01 |          1
        1 |          1 |       1 | 2017-01-01 00:00:00 |          2
        2 |          7 |     784 | 2017-01-01 00:00:04 |          1
        2 |          6 |     143 | 2017-01-01 00:00:03 |          2
        2 |          5 |     456 | 2017-01-01 00:00:02 |          3
        3 |         17 |      34 | 2017-01-01 00:00:09 |          1
        3 |         16 |     582 | 2017-01-01 00:00:08 |          2
        3 |         15 |     342 | 2017-01-01 00:00:07 |          3
        4 |         34 |     877 | 2017-01-01 00:00:16 |          1
        4 |         33 |     778 | 2017-01-01 00:00:15 |          2
        4 |         32 |      53 | 2017-01-01 00:00:14 |          3
        5 |         60 |     215 | 2017-01-01 00:00:25 |          1
        5 |         59 |     421 | 2017-01-01 00:00:24 |          2
        5 |         58 |     641 | 2017-01-01 00:00:23 |          3
....
....
       19 |       2489 |     284 | 2017-01-01 00:06:01 |          1
       19 |       2488 |     900 | 2017-01-01 00:06:00 |          2
       19 |       2487 |     554 | 2017-01-01 00:05:59 |          3
       20 |       2890 |     960 | 2017-01-01 00:06:40 |          1
       20 |       2889 |     789 | 2017-01-01 00:06:39 |          2
       20 |       2888 |     382 | 2017-01-01 00:06:38 |          3
(59 rows)
```

Nice!

## Resources
- https://blog.jooq.org/2014/08/12/the-difference-between-row_number-rank-and-dense_rank/
