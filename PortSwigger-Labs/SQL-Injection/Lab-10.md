# Lab 10: SQL Injection Attack - Listing the Database Contents on Oracle

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

SQL Injection / UNION-Based SQL Injection / Database Enumeration

## Lab Status

Solved

## Objective

The goal of this lab was to enumerate the Oracle database structure, identify the table containing user credentials, retrieve usernames and passwords, and log in as the administrator user.

## Simple Explanation

Unlike previous labs where table and column names were already known, this lab required discovering the database structure first.

Since the target database was Oracle, Oracle-specific metadata tables were used to identify table names and column names.

After identifying the table that stored user credentials and the columns containing usernames and passwords, the data was retrieved and used to log in as the administrator user.

## Vulnerability Description

The product category filter was vulnerable to SQL injection because user-controlled input was included in a SQL query without secure handling.

Since query results were displayed in the application's response, UNION-based SQL injection could be used to retrieve database metadata and sensitive information from other tables.

## Key Concept

Database enumeration is the process of discovering the structure of a database before extracting sensitive information.

This may include identifying:

* Table names
* Column names
* User credential storage locations
* Sensitive data sources

Oracle stores metadata in system tables such as:

```text
all_tables
all_tab_columns
```

## Steps Taken

1. Opened the lab from PortSwigger Web Security Academy.
2. Confirmed that the query returned two columns.
3. Confirmed that both columns accepted text data.
4. Queried `all_tables` to retrieve database table names.
5. Identified the table containing user credentials.
6. Queried `all_tab_columns` to retrieve column names from the identified table.
7. Located the username and password columns.
8. Retrieved usernames and passwords using a UNION-based SQL injection payload.
9. Identified the administrator user's credentials.
10. Logged in as the administrator user.
11. The lab was successfully solved.

## Result

The Oracle database structure was successfully enumerated.

The user credentials table and its columns were identified, allowing the administrator credentials to be retrieved.

## What I Learned

* Oracle uses different metadata tables than non-Oracle databases.
* `all_tables` stores table names.
* `all_tab_columns` stores column names.
* Database enumeration is often required before extracting sensitive data.
* UNION-based SQL injection can be used to retrieve both metadata and actual database contents.

## Security Impact

In a real-world application, attackers could enumerate database structure, discover sensitive tables, retrieve credentials, and potentially gain privileged access to the application.

## Mitigation

To prevent this vulnerability, developers should:

* Use parameterized queries or prepared statements.
* Avoid directly inserting user input into SQL queries.
* Validate and sanitize user-controlled parameters.
* Restrict database permissions.
* Avoid exposing query results to users.
* Store passwords securely using strong hashing algorithms.
* Monitor suspicious database activity.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Web Browser
