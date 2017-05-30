+++
date = "2017-05-29T23:00:00-07:00"
title = "Lessons learned from creating a postgresql data playground"
+++

## Objective
Create a script that produces a postgresql database. The structure will be fixed and the contents of the db may vary. The shape of the data should be mostly the same so that every time I rebuild the db I can expect similar queries across rebuilds to output similar data.

## End result
This world is built and feed with [fake-messaging-app-pg](https://github.com/chischaschos/fake-messaging-app-pg). I expect to keep changing it but I can call the first iteration completed.

## Lessons learned

### [Shebangs](https://en.wikipedia.org/wiki/Shebang_(Unix))
I put it at the beginning of my script, but why?
  ```
  #!/usr/bin/env sh
  ```

So after some wikipedia this is what I got:

- It is for Unix-like operating systems.
- The [*program loader*](https://en.wikipedia.org/wiki/Loader_(computing)) parses the rest of the script as an [*interpreter directive*](https://en.wikipedia.org/wiki/Interpreter_directive). The specified interpreter program is run instead passing to it the path initially used.
- Must be an absolute path for portability, though it is common to edit this line after copying scripts.
- It's syntax is `#!interpreter [optional-arg]` accepting at most one argument.
- It is a [*magic number*](https://en.wikipedia.org/wiki/Magic_number_(programming)), `0x23 0x21`, the two-character encoding in ASCII of `#!`.

## Bash variables
- First, there are many ways to do it.
- They can be lower case, or upper case.
- When you define one do not put spaces around the `=`:
  ```bash
  a = asdf # won't work
  a=asdf   # works
  ```

- Remember to use them by prefixing them with a `$`:
  ```bash
  a=asdf
  echo $a
  ```

- One way to create a multi line string is by using [here-docs](http://tldp.org/LDP/abs/html/here-docs.html) and [read](https://ss64.com/bash/read.html). `read` can receive a delimiter `-d ''` and maybe avoiding backslash as scape character `-r`:
   ```bash
   read -d '' -r command <<COMMAND
   hi
   there
   COMMAND
   ```

## Ruby syntax
When passing a block and invoking a single method in that block as in:
  ```ruby
  conn.exec_prepared('insert_users_groups', [group_id, baseuser.id]) do |result|
    result.check
  end
  ```

You can instead do:
  ```ruby
  conn.exec_prepared('insert_users_groups', [group_id, baseuser.id], :&check)
  ```

## Ruby pg interface
This was a good opportunity to learn more about the [pg](https://rubygems.org/gems/pg/versions/0.18.4) gem.

- Do a local connection with:
  ```ruby
  conn = PG.connect('dbname=fake_messaging_app')
  ```

- Create prepared statements with:
  ```ruby
  conn.prepare('insert_users', 'insert into users (name) values ($1)', &:check)
  ```

- Execute prepared statements with:
  ```ruby
  conn.exec_prepared('insert_users', [Faker::Name.name], &:check)
  ```

- Execute statements with:
  ```ruby
  conn.exec('select group_id as g, count(*) as c from users_groups group by 1 order by 1')
  ```

- Results are [PG::Result](https://deveiate.org/code/pg/PG/Result.html), iterate them with:
  ```ruby
  result.map { |row| row['g'] }
  ```

- Decode postgresql array strings with [PG::TextDecoder](https://deveiate.org/code/pg/PG/TextDecoder/Array.html) as in:
  ```ruby
   conn.exec('select array_agg(member_id) as mids from users_groups where group_id = $1', [group_id]) do |result|
    decoder = PG::TextDecoder::Array.new
    result.map { |row| decoder.decode(row['mids']).map(&:to_i) }.first
  end
  ```
