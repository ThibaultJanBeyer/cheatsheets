[back to overwiev](/../..)

# SQL Cheatsheet

## Table of Contents

- [SELECT](#select)
- [LIKE](#like)
- [CREATE](#create)
- [INSERT](#insert)
- [ALTER](#alter)
- [UPDATE](#update)
- [DELETE](#delete)
- [Constraints](#constraints)

## SELECT

```SQL
SELECT <column_name>, <column_name> FROM <table_name>;

SELECT * FROM celebs;
SELECT name AS 'Title' FROM celebs;
SELECT DISTINCT name FROM celebs;
SELECT * FROM movies WHERE imdb_rating > 8;
```

- `SELECT` is a clause that indicates that the statement is a query. You will use SELECT every time you query data from a database.
- `column_name` specifies the column to query data from. (* for all)
- `FROM` specifies the name of the table to query data from. In this statement, data is queried from the celebs table.
- `AS`: rename a column or table using an alias. Although it’s not always necessary, it’s best practice to surround your aliases with single quotes. When using AS, the columns are not being renamed in the table. The aliases only appear in the result.
- `DISTINCT`: return unique values in the output. It filters out all duplicate values in the specified column(s).
- `WHERE`: only include rows where the following condition is true.

### LIKE

```SQL
SELECT * FROM movies
WHERE name LIKE 'Se_en';
```
- `LIKE`: search for a specific pattern in a column.
- `'Se_en'`: pattern with a wildcard character.


## CREATE

```SQL
CREATE TABLE <table_name> ( <column> <datatype>, <column> <datatype> );
```

## INSERT

```SQL
INSERT INTO <table_name> (<row_1>, <row_2>, <row_3>) 
VALUES (1, 'Justin Bieber', 22); 
```

## ALTER

The ALTER TABLE statement adds a new column to a table. You can use this command when you want to add columns to a table. The statement below adds a new column twitter_handle to the celebs table.

```SQL
ALTER TABLE <table_name>
ADD COLUMN <column_name> TEXT;
```

- `ALTER TABLE`: clause that lets you make the specified changes.
- `ADD COLUMN`: clause that lets you add a new column to a table:
- `TEXT`: data type for the new column

## UPDATE

The UPDATE statement edits a row in a table. You can use the UPDATE statement when you want to change existing records. 

```SQL
UPDATE <table_name>
SET <column_name> = <column_name_value>
WHERE <row_name> = <row_value>;

UPDATE celebs 
SET twitter_handle = '@taylorswift13' 
WHERE id = 4; 
```

- `UPDATE`: edits a row in the table.
- `SET`: indicates the column to edit.
- `<column_name>`: name of the column that is going to be updated
- `<column_name_value>`: new value that is going to be inserted into the column.
- `WHERE`: indicates which row(s) to update with the new column value. 
- Here the row with a 4 in the id column is the row that will have the twitter_handle updated to @taylorswift13.

## DELETE

```SQL
DELETE FROM <table_name> 
WHERE <column_name> IS NULL;
```
- `DELETE FROM`: lets you delete rows from a table.
- `WHERE`: lets you select which rows you want to delete. Here we want to delete all of the rows where the `<column_name>` IS NULL.

## Constraints

```SQL
CREATE TABLE <table_name> (
   <column_name> INTEGER PRIMARY KEY, 
   <column_name> TEXT UNIQUE,
   <column_name> TEXT NOT NULL,
   <column_name> TEXT DEFAULT <default_value>
);
```
- `PRIMARY KEY` columns can be used to uniquely identify the row. Attempts to insert a row with an identical value to a row already in the table will result in a constraint violation which will not allow you to insert the new row.
- `UNIQUE` columns have a different value for every row. This is similar to PRIMARY KEY except a table can have many different UNIQUE columns.
- `NOT NULL` columns must have a value. Attempts to insert a row without a value for a NOT NULL column will result in a constraint violation and the new row will not be inserted.
- `DEFAULT` columns take an additional argument that will be the assumed value for an inserted row if the new row does not specify a value for that column.

## DROP

```SQL
DROP TABLE <table_name>;
```

## MERGE

```SQL
MERGE <target_table> USING <source_table>
ON <merge_condition>
WHEN MATCHED
    THEN <update_statement>
WHEN NOT MATCHED BY TARGET
    THEN <insert_statement>
WHEN NOT MATCHED BY SOURCE
    THEN DELETE;
```

Example:

```SQL
MERGE sales.category t 
    USING sales.category_staging s
ON (s.category_id = t.category_id)
WHEN MATCHED
    THEN UPDATE SET 
        t.category_name = s.category_name,
        t.amount = s.amount
WHEN NOT MATCHED BY TARGET 
    THEN INSERT (category_id, category_name, amount)
         VALUES (s.category_id, s.category_name, s.amount)
WHEN NOT MATCHED BY SOURCE 
    THEN DELETE;
```

## UNION
```SQL
SELECT <field_1>, <field_2> FROM <table_1>
UNION ALL
SELECT <field_1>, <field_2> FROM <table_2>
```

- `UNION ALL` will keep duplicates, just `UNION` will ignore duplicated

## JOIN

```SQL
SELECT <field_1>, <field_2>
FROM <table_1>
INNER JOIN <table_2> ON <table_1>.<field_1>=<table_2>.<field_2>;
```

- INNER JOIN: Returns records that have matching values in both tables
- LEFT (OUTER) JOIN: Returns all records from the left table, and the matched records from the right table
- RIGHT (OUTER) JOIN: Returns all records from the right table, and the matched records from the left table
- FULL (OUTER) JOIN: Returns all records when there is a match in either left or right table
