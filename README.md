# sql-cheatsheet
Personal cheat sheet for querying relational database in SQL SERVER

### Basic SELECT Statement
```sql
SELECT select_list 
[ FROM table_source ]
[ WHERE search_condition ] 
[ GROUP BY group_by_expression ] 
[ HAVING search_condition ] 
[ ORDER BY order_expression [ ASC | DESC ] ] 
```

### WHERE Conditions
```sql
-- AND 
SELECT column_name FROM table_name
WHERE condition1 AND condition2
```

```sql
-- OR
SELECT column_name FROM table_name
WHERE condition1 OR condition2
```

```sql
-- EXISTS
SELECT column_name FROM table_name
WHERE EXISTS (SELECT column_name FROM table_name)
```

```sql
-- ANY
SELECT column_name FROM table_name
WHERE column = ANY (SELECT column_name FROM table_name)
```

```sql
-- ALL
SELECT column_name FROM table_name
WHERE column = ALL (SELECT column_name FROM table_name)
```

```sql
-- WHERE NOT
SELECT column_name FROM table_name
WHERE NOT condition
```

### CASE Statement
```sql
CASE
    WHEN condition THEN 'true' 
    ELSE 'false'
END
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
SELECT TOP [ number | percent ] column_name
FROM table_name
WHERE condition;
```

###### MIN/MAX
*Returns the smallest/biggest value in selected column*
```sql
SELECT [ MIN | MAX ] (column_name)
FROM table_name
WHERE condition;
```

###### COUNT/AVG/SUM
```sql
SELECT [ COUNT | AVG | SUM] (column_name)
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

### STUFF 
###### The STUFF() function deletes a part of a string and then inserts another part into the string, starting at a specified position
```sql
-- Syntax
STUFF (character_expression, start, length, new_string )

-- Example: deletes the second digit of the product ID in the Productstable and replaces it with the characters '000'
SELECT STUFF([Product_ID], 2,1, '000')
FROM Products
-- OUTPUT: 20 becomes 2000
```

### REPLACE 
```sql
-- Syntax
REPLACE(string, old_string, new_string)

-- Replaces 'A' with 'C'
SELECT REPLACE('AB AB', 'A','C')
```

### COALESCE
###### Returns the first non-null value in a list:
```sql
SELECT COALESCE(NULL, NULL, NULL, 'JigJun', NULL, 1);
-- OUTPUT: 'JigJun'
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
    column data_type NOT NULL FOREIGN KEY REFERENCES table_name(column) -- foreign key
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
DROP COLUMN column_name datatype;
```

```sql
-- ALTER COLUMN
ALTER TABLE table_name
ALTER COLUMN column_name datatype;
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

###### IF ELSE Statement
```sql
IF (@variable = 1)
BEGIN
    --insert code here
END
ELSE
BEGIN
    --insert code here
END
```

### Declare Temporary Table
```sql
DECLARE @Temp TABLE
    (
        column INT,
        column2 VARCHAR(10)
    );
```

### Stored Procedure Template (GET)
```sql
SET QUOTED_IDENTIFIER ON
SET ANSI_NULLS ON
GO

/******************************************************************************   
** Change History  
**  
** CID    Date				Author			Description   
** -----  ---------- ---------- -----------------------------------------------  
** CH001  08/10/2019		N.Sun		    Initial Version 
*******************************************************************************/
-- exec [SP_Template_Get] 1
CREATE PROCEDURE [dbo].[SP_Template_Get]
	@Parameter INT
AS
BEGIN
    DECLARE @Result TABLE
    (
        column INT,
        column2 VARCHAR(10)
    );

	INSERT INTO @Result
	(
	    column,
		column2,
	)
	SELECT column,
		   @column2
	FROM dbo.table_name A 
	INNER JOIN dbo.table_name B 
        ON A.column = B.column
	WHERE A.column = @Parameter
  
	SELECT 
        column,
        column2
	FROM @Result;
END;

GO
```

### Stored Procedure Template (SET)
```sql
SET QUOTED_IDENTIFIER ON
SET ANSI_NULLS ON
GO

/******************************************************************************   
** Change History  
**  
** CID    Date				Author			Description   
** -----  ---------- ---------- -----------------------------------------------  
** CH001  08/10/2019		N.Sun		    Initial Version 
*******************************************************************************/
-- exec [SP_Template_Set] 1
CREATE PROCEDURE [dbo].[SP_Template_Set]
	@Parameter INT
AS
BEGIN
     BEGIN TRY
        BEGIN TRANSACTION        
        
        -- Do something here

        COMMIT TRANSACTION
        SELECT 1 As Success 
    END TRY
    BEGIN CATCH

        ROLLBACK TRANSACTION
        SELECT 0 As Success 
    END CATCH
END;

GO
```

### Common Table Expression (CTE) 
```sql
-- Specifies a temporary named result set, known as a common table expression (CTE). 
-- This is derived from a simple query and defined within the execution scope of a single SELECT, INSERT, UPDATE, or DELETE statement.


-- Defining the column list is optional
WITH CTE_Name (column1, column2)  
AS  
-- Define the CTE query.  
(  
    SELECT column1, column2  
    FROM Table1 
    WHERE column1 IS NOT NULL
)  
-- Define the outer query referencing the CTE name.  
SELECT *
FROM CTE_Name  
GROUP BY column1, column2  
ORDER BY column1, column2;  
GO  

```

### Finding text in SP
```sql
SET QUOTED_IDENTIFIER ON
SET ANSI_NULLS ON
GO

CREATE PROCEDURE [dbo].[Find_Text_In_SP]
	@StringToSearch VARCHAR(100),
	@StringToSearch2 VARCHAR(100) = '',
	@StringToSearch3 VARCHAR(100) = '',
	@Name VARCHAR(100) = ''

AS

	SET @StringToSearch = '%' +@StringToSearch + '%'
	SET @StringToSearch2 = '%' +@StringToSearch2 + '%'
	SET @StringToSearch3 = '%' +@StringToSearch3 + '%'
	SET @Name = '%' +@Name + '%'

	SELECT ROUTINE_NAME, LEN(OBJECT_DEFINITION(OBJECT_ID(ROUTINE_NAME))) AS SP_Length
		FROM INFORMATION_SCHEMA.ROUTINES
	WHERE OBJECT_DEFINITION(OBJECT_ID(ROUTINE_NAME)) LIKE @stringtosearch
		AND OBJECT_DEFINITION(OBJECT_ID(ROUTINE_NAME)) LIKE @StringToSearch2
		AND OBJECT_DEFINITION(OBJECT_ID(ROUTINE_NAME)) LIKE @StringToSearch3
		AND (ROUTINE_TYPE='PROCEDURE' OR ROUTINE_TYPE='FUNCTION')
		AND ROUTINE_NAME LIKE @Name
	ORDER BY routine_name

GO
```


### OFFSET FETCH Clause
```sql
-- Skip first 10 rows from the sorted result set and return the remaining rows.
SELECT column1, column2 FROM table_name ORDER BY column1 OFFSET 10 ROWS;
```

```sql
-- Skip first 10 rows from the sorted resultset and return next 5 rows.
SELECT column1, column2 FROM table_name ORDER BY column1 OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;
```


### Renaming a Table
```sql
exec sp_rename '[schema.old_table_name]', 'new_table_name'
```

### Renaming a Column
```sql
exec sp_rename 'table_name.[oldColumName]' , 'newColumName', 'COLUMN'
```

### SCOPE_IDENTITY 
```sql
-- returns the last IDENTITY value inserted into an IDENTITY column in the same scope
-- returns the last identity value generated for any table in the current session and the current scope
-- A scope is a module; a Stored Procedure, trigger, function, or batch

SELECT SCOPE_IDENTITY()

```

### FIND WHICH TABLE A CONSTRAINT BELONGS TO
```sql
SELECT
   OBJECT_NAME(o.parent_object_id)
FROM
   sys.objects o
WHERE
   o.name = 'MyConstraintName' AND o.parent_object_id <> 0
``` 

### TRY-CATCH STATEMENT
```sql
BEGIN TRY
    BEGIN TRANSACTION        
    
    -- Do something here

    COMMIT TRANSACTION
    SELECT 1 As Success 
END TRY
BEGIN CATCH
    DECLARE 
        @ErrorMessage NVARCHAR(4000),
        @ErrorSeverity INT,
        @ErrorState INT;
    SELECT 
        @ErrorMessage = ERROR_MESSAGE(),
        @ErrorSeverity = ERROR_SEVERITY(),
        @ErrorState = ERROR_STATE();
    RAISERROR (
        @ErrorMessage,
        @ErrorSeverity,
        @ErrorState    
        );
        
    ROLLBACK TRANSACTION
    SELECT 0 As Success 
END CATCH

```

### OPTIONAL CONDITION VARIABLES IN WHERE CLAUSE
```sql
-- using '=' operator
WHERE Column = IIF(@Variable IS NULL ,@Variable, Column)

-- using 'LIKE, IN, etc.'
WHERE (@Variable IS NULL OR Column LIKE '%' + @Variable + '%' )

```

### INSERT COMMA SEPARATED STRING TO A TABLE
```sql

DECLARE @String = '1, 4, 3'
DECLARE @Tbl TABLE(ID INT);

INSERT INTO @Tbl
(
    ID
)
(SELECT value  
FROM STRING_SPLIT(@String, ',')  
WHERE RTRIM(value) <> '');

```

### UPDATE WITH JOIN
```sql

UPDATE T1
SET T1.ID = 1
FROM Table1 T1
    INNER JOIN Table2
        ON T1.ID = T2.ID

```

### DELETE WITH JOIN
```sql
DELETE A
FROM Table1 A
INNER JOIN Table2 B
  ON B.Id = A.Id
WHERE A.Column = 1 AND B.Column = 2
```

### UPDATE/INSERT IDENTITY COLUMN
```sql

SET IDENTITY_INSERT YourTable ON

   -- UPDATE/INSERT STATEMENT HERE

SET IDENTITY_INSERT YourTable OFF

```


### Find Foreign Key constraint references of a table
```sql
SELECT 
   OBJECT_NAME(f.parent_object_id) TableName,
   COL_NAME(fc.parent_object_id,fc.parent_column_id) ColName
FROM 
   sys.foreign_keys AS f
INNER JOIN 
   sys.foreign_key_columns AS fc 
      ON f.OBJECT_ID = fc.constraint_object_id
INNER JOIN 
   sys.tables t 
      ON t.OBJECT_ID = fc.referenced_object_id
WHERE 
   OBJECT_NAME (f.referenced_object_id) = 'Table_Name'
```

### Parse a JSON file into a table
```sql
-- JSON Data sample: 
-- {
-- "label": "test ",
-- "value": 1
-- },
-- {
-- "label": "test2 ",
-- "value": 2
-- }

DECLARE @tbl TABLE (id INT, label VARCHAR(500));

DECLARE @json VARCHAR(max);

SELECT @json = BulkColumn
 FROM OPENROWSET (BULK 'C:\jsonFile.json', SINGLE_CLOB) as j


 INSERT INTO @tbl (id, label)
 SELECT [value], label
 FROM OPENJSON(@json)
 WITH ([value] int,
       label nvarchar(max))

SELECT * FROM @tbl
```

### Add FK to existing column
```sql
ALTER TABLE [Table1]
ADD CONSTRAINT FK_Table2_Id FOREIGN KEY (Table1_Id)
    REFERENCES Table2(Table2_Id);
```