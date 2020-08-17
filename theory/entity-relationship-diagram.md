# Entity Relationship Diagram (ERD)

## Cardinalities

- Are indications how entities relate to another
- 1, many, 1:1, 0:1, 1:many, 0:many, many:many
- A way to display Minimums and Maximums

### Example

[Customer] (1) = (2) [Order]

- (1) What's the maximum amount of customers an order can have?
- - 1
- (2) What's the minimum amount of customers an order can have?
- - 1
Notation: ||-

- (2) What's the minimum amount of orders a customer can have?
- - 0
- (2) What's the maximum amount of orders a customer can have?
- - Many
Notation: -0E

[Customer] ||-=-0E [Order]

## Data types

- You can add a third row to add data types specifications
- i.e. int, varchar(50) (= various characters(limit)), …

## Primary Keys (PK) and Foreign Keys (FK)

### Primary Keys (PK)

- Are unique amongst the rows in the tables and never changing. i.e. an ID of a row.
- Makes sure that "this entry" is unlike any other entry.
- Often used when querying for single elements in the DB.

#### Base rules

1. Unique
2. Never Changing
3. Never Null

#### Composite Primary Key

- If there is no unique PK but a combination of two elements forms a unique PK.
- Just write 2 PKs in the diagram
- Might not be necessary

##### Rules

1. Use fewest number of attributes as possible
2. Don't use attributes that change often

### Foreign Keys (FK)

- Represents a PK from another entity/table
- Align the cardinalities to connect the FKs with their respective PKs

### Rules

- Match a PK in another table
- Don't have to be unique
- Can be multiple in one entity

## Bridge Tables

- Is there anything else I should be recording into the database?
- Do I get all necessary information?
- Sometimes you've two entities connected to another but there is more going on between them
- A table holding interactions between the entities
- Useful to break down many to many relationships

# References

1. https://www.youtube.com/watch?v=QpdhBUYk7Kk
2. https://www.youtube.com/watch?v=-CuY5ADwn24

