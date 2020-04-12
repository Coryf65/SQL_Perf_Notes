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

**SET STATISTICS IO**, The number of IO's sql server will do to complete our query. Logical reads from memory and Physical reads by disk

**TIME ON**, actual execution time 

*Before the query select*
```tsql
SET STATISTICS IO, TIME ON
```

### Key Lookups

- Generally we would want to remove clustered lookups

### Nested Loops vs Hash Joins

- Hash Joins are a table that gets created in memory to match values together, and bring them together for your join operation.

- Hash joins are very efficient on a large set of data.

- They're less efficient on a very small set of data. Because the process of creating the hash table in memory is rather expensive, relative to a very small number of rows.

*you could hint the join types, COULD IMPACT NEGATIVELY*

example: letting the sql server decide
```tsql
INNER JOIN Person p ON p.ID = User.UserID
```

example: Hinting to use a hash join
```tsql
INNER HASH JOIN Person p ON p.ID = User.UserID
```

- The differed join selection will change from nested loops to hash tables and vice versa based on statistics 
    
    that are available from within the query and this is part of a new set of features in SQL Server 2017 called Automatic Tuning.


### Query Store

- Not enabled by default, can enable using the model database

- The Query Store is, simply put, a data recorder for SQL Server so, it records two main sets of data as its inputs, 
    and that's one-time compilation information so, the inputs that went into generating a query execution plan 
    for your query, those are captured, and then it generates run-time statistics, so how long your query took to run.

### Stored Procedures vs Dynamic SQL

- Stored Procedures

    - Code thats built into the database and schema

    - our plans are cached, which could have better performance

    - sp_exec

    - will out perform in most cases

    - **NOT** Stored as compiled code they are compiled on runtime

- Dynamic SQL

    - Good for Administrative tasks

    - examples: automation purposes, creating backups

### Missing Index Warnings

- more of an Art than a Science

- a missing index warning is for the specific query, does not correalte to the rest of your workload

- Indexs do have a cost, can cause Write amplification

- Index effectively 

- *should* look at the missing index recommendations as a whole.

## What NOT to DO with a SQL Server

- Cursors are bad

- While loops === cursors

- Do the best you can to avoid Cusors    

Set Operations | Cursor Logic
-------------- | ------------
Performs much better in general | Operates row by row
Scales *way better* | Uses more memory
Easier to read and troubleshoot | Creates more Blocking

### What to do instead ?

- try to do JOINS they are your friends!

- try to create a temp table

- create new tables to join

- Table-valued Function and Parameters

- Other Options

    - Consider doing things at Application tier

    - *After*, filtering down to the narrow set of data you need to use

    - Keep in mind Row-By-Row operations are always a bad thing


## Scalar and User Defined Functions are Expensive

- Single threaded Operations

- Interpreted for every call

- challenging to troubleshoot

### What to Do instead ?

- CLR Functions (security risks)

- Put code inline

- Table-Valued Functions (*Best Option*)