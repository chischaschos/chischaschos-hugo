+++
date = "2017-02-10T12:00:00-07:00"
title = "Life without an SQL IDE"
draft = true

+++

## Motivation and background
I've used some SQL IDEs, a few good, some bad. Most of my day is spent on the commandl line. I just don't want to open
an IDE to do simple queries. Additionally and when trying to use `psql` or `mysql` I didn't remeber the syntax for doing
simple operations such as connections or dumping data. I started feeling that I was getting dummer, I no longer was
able to proficiently use the related command lines (psql, mysql, sqlplus etc).

So I decided to stop using SQL IDEs for a while and see what happened.

This is a summary of my findings, it has tips to achieve common operations plus additional notes on tooling. I'm
treating this list as my own SQL command line cheat sheet.


## Common operations
Most of the time this are the common operations I would do:

- Connect/Disconnect to a DB
- Reuse connections, I don't want to remember them
- Querying, getting a nice view of the results
- Store queries and reuse them
- Export result sets to text, csv or xml
- Local connections to databases in servers where I only have ssh access

Now let's see how different command line tools do connections.

## Basics
Just ask for help:

```
mysql --help
psql --help
sqlplus --help
```

## Connection
```
mysql --user=USER --host=HOST --database=DB --port=PORT
psql --username=USER --host=HOST --dbname=DB --port=PORT
```

## Output data
- Output data to standard output:
  ```
  mysql --user=USER --host=HOST --database=DB --port=PORT --execute=COMMAND
  psql --username=USER --host=HOST --dbname=DB --port=PORT --command=COMMAND
  ```

- Output data to a file:
  ```
  mysql --user=USER --host=HOST --database=DB --port=PORT --execute=COMMAND > file.txt
  psql --username=USER --host=HOST --dbname=DB --port=PORT --command=COMMAND > file.txt
  ```

- In mysql create a csv, when you have write access to the server:
  ```
  COMMAND="SELECT QUERY INTO OUTFILE 'file_name.csv' FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n';"
  mysql --user=USER --host=HOST --database=DB --port=PORT --execute=$COMMAND > file.txt
  ```

- In mysql create a csvm when you don't have write acccess to the server:
  ```
  COMMAND='query using contact_ws'
  mysql --user=USER --host=HOST --database=DB --port=PORT --execute=$COMMAND > file.txt
  ```

- In postgresql create a csv, when you have write access to the server:
- In postgresql create a csv, when you don't have write access to the server:
  ```
  psql --host=HOST \
    --dbname=DB \
    --username=USER \
    --command="COPY(your query) TO STDOUT DELIMITER ',' HEADER CSV;"
  ```

- In postgresql, do a usually faster full table dump:
  ```
  pg_dump --host host
    --dbname dbname \
    --username username \
    --verbose \
    --file $file_name \
    --data-only \
    --no-privileges \
    --table table1 \
    --table table2
    # --format=custom
    # --exclude-table-data
  ```

- In postgresql, output a table's schema:
  ```
  pg_dump --format=plain \
    --table=TABLE_NAME \
    --host HOST \
    --schema-only \
    --username USER DB
  ```

## Input data

- In postgresql, restoring a table from standard input:
  ```
  psql --host HOST\
    --dbname DB \
    --username USER \
    --command "COPY table (columns) FROM STDIN DELIMITER ',' HEADER CSV;"
  ```

- In postgresql, restoring a dump:
  ```
  pg_restore --verbose \
    --clean \
    --no-acl \
    --no-owner \
    --host HOST \
    --username USER \
    --database DB dump_file
  ```
