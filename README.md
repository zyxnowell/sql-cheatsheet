# sql-cheatsheet
Personal cheat sheet for querying relational database in SQL SERVER

## The Basics

### Basic SELECT Statement
```sql
SELECT DISTINCT column FROM table_name 
WHERE condition
ORDER BY column ASC|DESC
```

### WHERE Conditions
```sql
WHERE condition1 AND condition2

WHERE condition1 OR condition2

WHERE NOT condition
```

### INSERT INTO Table
###### Specify columns
```sql
INSERT INTO table_name (column, column)
VALUES (value, value)
```
###### Insert to all columns
```sql
INSERT INTO table_name
VALUES (value, value)
```

### UPDATE Table
```sql
UPDATE table_name
    SET column = value,
        column = value,
        column = value
WHERE condition;
```

### DELETE Table
```sql
DELETE FROM table_name WHERE condition;
```

### TRUNCATE Table
```sql
TRUNCATE TABLE table_name;
```

### Basic Functions
###### SELECT TOP
```sql
SELECT TOP number|percent column_name
FROM table_name
WHERE condition;
```

###### MIN/MAX
*Returns the smallest/biggest value in selected column*
```sql
SELECT MIN|MAX(column_name)
FROM table_name
WHERE condition;
```

###### COUNT/AVG/SUM
```sql
SELECT COUNT|AVG|SUM(column_name)
FROM table_name
WHERE condition;
```

### LIKE Syntax
```sql
SELECT column
FROM table_name
WHERE column LIKE pattern;
```

###### pattern operator
```sql
WHERE column LIKE 'a%'	--Finds any values that start with "a"
```

```sql 
WHERE column LIKE '%a' --Finds any values that end with "a"
```	

```sql 
WHERE column LIKE '%or%' --Finds any values that have "or" in any position
```	

```sql 
WHERE column LIKE '_r%' --Finds any values that have "r" in the second position
```	

```sql 
WHERE column LIKE 'a_%_%' --Finds any values that start with "a" and are at least 3 characters in length
```	

```sql 
WHERE column LIKE 'a%o' --Finds any values that start with "a" and ends with "o"
```	


### JOINS
###### Different types of JOINS
- (INNER) JOIN: Returns records that have matching values in both tables
- LEFT (OUTER) JOIN: Return all records from the left table, and the matched records from the right table
- RIGHT (OUTER) JOIN: Return all records from the right table, and the matched records from the left table
- FULL (OUTER) JOIN: Return all records when there is a match in either left or right table

```sql
SELECT column_name(s)
FROM table A
JOIN table B 
    ON A.column_name = B.column_name;
```


### SQL Statements

###### CREATE Table
```sql
CREATE TABLE table_name (
    column int IDENTITY(1,1) PRIMARY KEY, -- primary key and "IDENTITY(1,1)" for auto increment
    column data_type NOT NULL,            -- normal column
	ListingTypeID int NOT NULL FOREIGN KEY REFERENCES table_name(column) -- foreign key
);
```

###### ALTER Table

```sql
-- ADD Column
ALTER TABLE table_name
ADD column_name datatype;
```

```sql
-- DROP COLUMN
ALTER TABLE table_name
DROP column_name datatype;
```

```sql
-- ALTER COLUMN
ALTER TABLE table_name
ALTER column_name datatype;
```


###### CHECK Constraint
```sql
-- The CHECK constraint is used to limit the value range that can be placed in a column
CREATE TABLE table_name (
    ID int NOT NULL,
    [Percentage] DECIMAL(5,4),
    CHECK ([Percentage] <= 1.0000)
);
``` 

###### DEFAULT Constraint
```sql
-- DEFAULT constraint is used to provide a default value for a column
CREATE TABLE table_name (
    ID int NOT NULL,
    VARCHAR_column varchar(255) DEFAULT 'Text',
    INT_column INT DEFAULT 1
);
```