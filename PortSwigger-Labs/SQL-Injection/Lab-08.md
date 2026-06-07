# Lab 08: SQL Injection Attack - Listing the Database Contents on Non-Oracle Databases

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
SQL Injection / UNION-Based SQL Injection / Database Enumeration

## Lab Status
Solved

## Objective
The goal of this lab was to use a UNION-based SQL injection vulnerability to enumerate the database structure, identify the table containing user credentials, retrieve usernames and passwords, and log in as the administrator user in a controlled lab environment.

## Simple Explanation
In previous labs, the table and column names were provided. In this lab, they were unknown.

To solve the lab, I first had to discover the database structure. Since this was a non-Oracle database, I used `information_schema` to list table names and column names.

After identifying the users table and its username and password columns, I retrieved the credentials and logged in as the administrator user.

## Vulnerability Description
The product category filter was vulnerable to SQL injection because user-controlled input was included in a SQL query without secure handling.

Since the query results were displayed in the application's response, it was possible to use UNION-based SQL injection to retrieve database metadata and sensitive data from another table.

## Key Concept
Database enumeration is the process of discovering the structure of a database.

This may include identifying:

- Table names
- Column names
- Sensitive data locations

On many non-Oracle databases, `information_schema.tables` can be used to list table names, and `information_schema.columns` can be used to list column names.

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Selected a product category and observed the category parameter.
3. Used Burp Suite to intercept and modify the request.
4. Confirmed that the query returned two columns.
5. Confirmed that both columns were compatible with text data.
6. Queried `information_schema.tables` to list available database tables.
7. Identified the table containing user credentials.
8. Queried `information_schema.columns` to identify the username and password columns.
9. Retrieved usernames and passwords from the identified users table.
10. Located the administrator user's credentials.
11. Logged in as the administrator user.
12. The lab was successfully solved.

## Result
The database structure was successfully enumerated.

The users table and its username and password columns were identified, allowing the administrator credentials to be retrieved in the controlled lab environment.

## What I Learned
- SQL injection can be used to enumerate database structure when table and column names are unknown.
- `information_schema` stores useful metadata about tables and columns in many non-Oracle databases.
- Database enumeration is an important step before retrieving sensitive data.
- UNION-based SQL injection requires the correct number of columns and compatible data types.
- Exposed database metadata can help locate sensitive information.

## Security Impact
In a real-world application, this vulnerability could allow unauthorized users to discover database structure and retrieve sensitive information such as usernames and passwords.

If administrator credentials are exposed, an attacker may gain privileged access to the application.

## Mitigation
To prevent this vulnerability, developers should:

- Use parameterized queries or prepared statements.
- Avoid directly inserting user input into SQL queries.
- Validate and sanitize user-controlled parameters.
- Restrict database account permissions.
- Avoid exposing database errors or query results to users.
- Store passwords securely using strong hashing algorithms.
- Monitor suspicious database queries and unusual request patterns.

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Burp Proxy
- Web browser
