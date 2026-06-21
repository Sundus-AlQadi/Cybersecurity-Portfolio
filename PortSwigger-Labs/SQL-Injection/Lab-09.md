# Lab 09: SQL Injection Attack - Querying the Database Type and Version on Oracle

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

SQL Injection / UNION-Based SQL Injection / Database Enumeration

## Lab Status

Solved

## Objective

The goal of this lab was to use a UNION-based SQL injection vulnerability to identify the Oracle database version string in a controlled lab environment.

## Simple Explanation

In previous labs, I learned how to determine the number of columns returned by a query and identify which columns could display text data.

In this lab, I used those techniques to retrieve information about the database itself. Since the target was an Oracle database, I queried Oracle's version information table and displayed the database version string in the application's response.

## Vulnerability Description

The product category filter was vulnerable to SQL injection because user-controlled input was included in a SQL query without secure handling.

Since the query results were displayed in the application's response, it was possible to use a UNION-based SQL injection attack to retrieve information from Oracle system tables.

## Key Concept

Database enumeration is the process of gathering information about a database system.

This may include identifying:

* Database type
* Database version
* Table names
* Column names
* Database structure

Different database systems use different syntax and metadata locations.

Oracle stores version information in:

```text
v$version
```

The version details are stored in the:

```text
BANNER
```

column.

## Steps Taken

1. Opened the lab from PortSwigger Web Security Academy.
2. Selected a product category and observed the category parameter.
3. Used Burp Suite to intercept and modify the request.
4. Confirmed that the query returned two columns.
5. Confirmed that the first column could display text data.
6. Used an Oracle-specific UNION-based SQL injection payload.
7. Queried the `v$version` table.
8. Retrieved the value stored in the `BANNER` column.
9. Displayed the Oracle database version string.
10. The lab was successfully solved.

## Result

The Oracle database version information was successfully retrieved and displayed in the application's response.

## What I Learned

* SQL injection can be used to identify database type and version.
* Oracle stores version information in `v$version`.
* The `BANNER` column contains version details.
* Oracle commonly uses the `dual` table for standalone SELECT statements.
* Database enumeration is an important step before performing more advanced SQL injection attacks.

## Security Impact

In a real-world application, attackers could identify the exact database technology and version in use.

This information may help attackers select database-specific payloads, identify known vulnerabilities, and plan further attacks.

## Mitigation

To prevent this vulnerability, developers should:

* Use parameterized queries or prepared statements.
* Avoid directly inserting user input into SQL queries.
* Validate and sanitize user-controlled parameters.
* Restrict database account permissions.
* Avoid exposing database query results to users.
* Monitor suspicious database queries and unusual request patterns.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Web Browser
