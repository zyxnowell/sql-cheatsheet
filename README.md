# sql-cheatsheet
Personal cheat sheet for querying relational database

## The Basics

###### Basic SELECT Statement
```
SELECT DISTINCT column FROM table_name 
WHERE condition
ORDER BY column ASC|DESC
```

###### WHERE Conditions
```
WHERE condition1 AND condition2

WHERE condition1 OR condition2

WHERE NOT condition
```

###### INSERT INTO Table
**Specify columns**
```
INSERT INTO table_name (column, column)
VALUES (value, value)
```
**Insert to all columns**
```
INSERT INTO table_name
VALUES (value, value)
```

###### UPDATE Table
```
UPDATE table_name
    SET column = value,
        column = value,
        column = value
WHERE condition;
```

###### DELETE Table
```
DELETE FROM table_name WHERE condition;
```

###### TRUNCATE Table
```
TRUNCATE TABLE table_name;
```

###### Basic Functions
**SELECT TOP**
```
SELECT TOP number|percent column_name
FROM table_name
WHERE condition;
```

**MIN/MAX**
*Returns the smallest/biggest value in selected column*
```
SELECT MIN|MAX(column_name)
FROM table_name
WHERE condition;
```

**COUNT/AVG/SUM**
```
SELECT COUNT|AVG|SUM(column_name)
FROM table_name
WHERE condition;
```

###### LIKE Syntax
```
SELECT column
FROM table_name
WHERE column LIKE pattern;
```

**pattern operator**
```
WHERE column LIKE 'a%'	
```
> Finds any values that start with "a"