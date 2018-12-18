+++
date = "2018-09-12T15:06:00-07:00"
title = "TIL debugging Rails migrations"
draft = false
tags = ["til", "notes"]
+++

## Notes
- If using references, the table you are referring to must already exists.
- You can debug migrations errors by looking at the issued statements in the logs.
- `add_index`, `add_foreign_key` are more flexible than `t.references` and its `foreign_key` option.
  - .. but why?
- Docs https://api.rubyonrails.org/v4.2.8/
- Docs https://dev.mysql.com/doc/refman/5.6/en/alter-table.html
