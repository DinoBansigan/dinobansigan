Title: Querying JSON Array in SQL Server
Published: 01/23/2019
Tags:
   - SQL Server
   - Scripts
   - Database
   - JSON
---
One of the best additions to SQL Server 2016 is [native support for JSON](https://docs.microsoft.com/en-us/sql/relational-databases/json/json-data-sql-server?view=sql-server-2017). There a number of articles already covering how to query JSON in SQL Server, so I won't cover those. What particularly gives me headaches though is querying a JSON Array in SQL Server. Hopefully the scripts below can help someone else. 

The SELECT script below can be used when you are working with a very basic JSON array that doesn't even have a Property for the array. In this example, we have an array of OrderIds.
```
DECLARE @OrderIdsAsJSON VARCHAR(MAX) = '["1", "2", "3"]';

SELECT [value] AS OrderId
FROM OPENJSON(@OrderIdsAsJSON)
WITH
(
       [value] BIGINT '$'
);
```

The second SELECT script below can be used when you are working with a JSON array that does have a Property defined for the array. In this example, we have an array of OrderIds with a defined OrderId property.
```
DECLARE @OrderIdsAsJSON VARCHAR(MAX) = '{"OrderId":["4", "5", "6"]}';

SELECT [value] AS OrderId
FROM OPENJSON(@OrderIdsAsJSON, '$.OrderId')
WITH
(
       [value] BIGINT '$'
);
```
<hr />

*<h4>Update: 2019-07-17</h4>*

Adding another example for a JSON array that came from an Int list in C#. This is how you could query it in SQL Server.
```
DECLARE @OrderIds VARCHAR(MAX) = '{"$type":"System.Int64[], mscorlib","$values":[4, 5, 7, 999]}';

SELECT [value] AS OrderId
FROM OPENJSON(@OrderIds, '$."$values"');
```