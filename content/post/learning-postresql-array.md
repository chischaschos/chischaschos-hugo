+++
categories = ["sql"]
date = "2017-05-26T10:24:42-07:00"
title = "learning postgresql array"
draft = true
+++

Learn postgresql, learn sql, solve the following questions:

> Given one `User` can belong to many `UserGroups`
> Compare a design where `users(id)` and `users_groups(group_id int, member_id int)`
> Versus `users(id)` and `users_groups(group_id int,  members int[]`

TODO: Does unique index for two columns is enough? or should I index per column as well
