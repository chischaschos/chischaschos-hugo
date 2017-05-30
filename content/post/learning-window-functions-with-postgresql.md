+++
categories = ["sql"]
date = "2017-05-26T10:24:42-07:00"
title = "learning window functions with postgresql"
draft = true

+++

https://blog.jooq.org/2014/08/12/the-difference-between-row_number-rank-and-dense_rank/
## Top 3 message from my groups
Good opportunity to learn about [window functions](https://www.postgresql.org/docs/9.1/static/tutorial-window.html). Let's first get the messages for a single group:
```
fake_messaging_app=> select id, created_at, row_number() over (partition by group_id order by created_at desc) from messages where group_id = 1;
 id |     created_at      | row_number
----+---------------------+------------
 20 | 2017-01-10 02:02:02 |          1
 19 | 2017-01-10 02:02:02 |          2
 18 | 2017-01-09 02:02:02 |          3
 17 | 2017-01-09 02:02:02 |          4
 15 | 2017-01-08 02:02:02 |          5
 16 | 2017-01-08 02:02:02 |          6
 14 | 2017-01-07 02:02:02 |          7
 13 | 2017-01-07 02:02:02 |          8
 11 | 2017-01-06 02:02:02 |          9
 12 | 2017-01-06 02:02:02 |         10
 10 | 2017-01-05 02:02:02 |         11
  9 | 2017-01-05 02:02:02 |         12
  7 | 2017-01-04 02:02:02 |         13
  8 | 2017-01-04 02:02:02 |         14
  6 | 2017-01-03 02:02:02 |         15
  5 | 2017-01-03 02:02:02 |         16
  4 | 2017-01-02 02:02:02 |         17
  3 | 2017-01-02 02:02:02 |         18
  2 | 2017-01-01 02:02:02 |         19
  1 | 2017-01-01 02:02:02 |         20
(20 rows)
```

> NOTE: Window functions are permitted only in the SELECT list and the ORDER BY clause of the query. They are forbidden elsewhere, such as in GROUP BY, HAVING and WHERE clauses.

Now only pick 3:
```
fake_messaging_app=> select * from (select id, created_at, row_number() over (partition by group_id order by created_at desc) from messages where group_id = 1 ) t1 where row_number <= 3;
 id |     created_at      | row_number
----+---------------------+------------
 20 | 2017-01-10 02:02:02 |          1
 19 | 2017-01-10 02:02:02 |          2
 18 | 2017-01-09 02:02:02 |          3
(3 rows)

```

And now do it for all the groups from one specific user:
```
fake_messaging_app=> select group_id, id as message_id, user_id, created_at, row_number from (select m.id, m.user_id, m.message, m.created_at, m.group_id, row_number() over (partition by m.group_id order by m.created_at desc) from messages m inner join users_groups ug using(group_id) where ug.member_id = 1) t1 where row_number <= 3;
 group_id | message_id | user_id |     created_at      | row_number
----------+------------+---------+---------------------+------------
        1 |         20 |      75 | 2017-01-10 02:02:02 |          1
        1 |         19 |       1 | 2017-01-10 02:02:02 |          2
        1 |         17 |       1 | 2017-01-09 02:02:02 |          3
....
....
       19 |      24889 |     619 | 2017-01-10 02:02:02 |          2
       19 |      24888 |     666 | 2017-01-10 02:02:02 |          3
       20 |      28900 |     452 | 2017-01-10 02:02:02 |          1
       20 |      28899 |     340 | 2017-01-10 02:02:02 |          2
       20 |      28898 |     980 | 2017-01-10 02:02:02 |          3
(60 rows)
```





