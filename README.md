# SQL_Perf_Notes
concepts and strategies that can help you enhance your app's performance on SQL Server

## Resource

The following info was documented from [Linkedin Learning - SQL Performance for Developers](https://www.linkedin.com/learning/sql-server-performance-for-developers/execution-plans-introduction)


## Query Execution

- Multiple Plans are created the first time a query is ran

- You can See the execution plans and are put into an XML file

- Read plans from RIGHT TO LEFT and TOP TO BOTTOM

- All Operators Cost something

- click on Show execution plan in toolbar of SSMS, check out 

    Actual Number of Rows vs. Estimated Number of Rows

    This will let us know we might need to read ahead, or tune our query if the two numbers differ

Examples:

SET STATISTICS IO, The number of IO's sql server will do to complete our query. Logical reads from memory and Physical reads by disk

TIME ON, actual execution time 

*Before the query select*
```tsql
SET STATISTICS IO, TIME ON
```

## What NOT to DO with a SQL Server

