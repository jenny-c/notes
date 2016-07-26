# SQL - Structured Query Language
* a language for interacting with data in tables
* 4 main operations - CRUD
  * create
  * read
  * update
  * delete

# creating tables
``` SQL
CREATE TABLE person (
  -- each column has a 'name' and a 'type'
  id INTEGER PRIMARY KEY, -- to id each row
  first_name TEXT,
  last_name TEXT,
  age INTEGER,
);
```

# constraints
* PRIMARY KEY
  * each value in this column will be unique
  * each table should have (but only 1) a PRIMARY KEY
