# Lab 03: SQL Injection UNION Attack - Determining the Number of Columns Returned by the Query

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
SQL Injection / UNION-Based SQL Injection

## Lab Status
Solved

## Objective
The goal of this lab was to determine the number of columns returned by the original SQL query using a UNION-based SQL injection technique in a controlled lab environment.

## Vulnerability Description
The product category filter was vulnerable to SQL injection. The application used the category parameter inside a SQL query without secure input handling.

Because the query results were returned in the application's response, it was possible to test UNION SELECT statements and determine how many columns were returned by the original query.

## Key Concept
For a UNION SELECT query to work, both SELECT statements must return the same number of columns.

For example, if the original query returns three columns, the injected UNION SELECT statement must also return three values.

NULL values are commonly used during testing because they are flexible and can usually fit different column data types.

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Selected a product category and observed the category parameter in the request.
3. Used Burp Suite to intercept and modify the request.
4. Tested a UNION SELECT statement with one NULL value.
5. Observed that the application returned an error.
6. Tested again with two NULL values.
7. Observed that the error still appeared.
8. Added a third NULL value.
9. The error disappeared and the response included additional content.
10. Determined that the original query returned three columns.
11. The lab was successfully solved.

## Result
The original SQL query returned:

```text
3 columns
