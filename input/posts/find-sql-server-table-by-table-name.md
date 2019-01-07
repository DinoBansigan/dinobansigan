Title: Find SQL Server Table by Table Name
Published: 01/07/2019
Tags:
   - SQL Server
   - Scripts
   - Database
---
The scripts below will help you find a table in a SQL Server database based on a given "table name" filter. This is helpful if you are not familiar with a rather large database and didn't want to waste time looking for a table manually. In my case, I usually use these scripts to check for an existing table in a database, before I create a new one. 

Most of the time when working with a large database, the table that you think you need to add, was already created by someone else. Oftentimes it is named differently than what you would have named it, but the contents and use of the table would have been the same. So it pays to double check before creating an already existing table.

```
SELECT *
FROM SYS.tables
WHERE NAME LIKE '%tableName%'

-- The script below will give you a result in the "schema.tablename" format, which can help you quickly locate the table.
SELECT (SELECT [Name] FROM SYS.schemas AS S WHERE S.schema_id = T.schema_id) + '.' +  T.Name AS 'TableName'
FROM SYS.tables AS T
WHERE NAME LIKE '%tableName%'
```