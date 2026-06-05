# Lab 01: SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data

## Platform
PortSwigger Web Security Academy

## Difficulty
Apprentice

## Topic
SQL Injection

## Lab Status
Solved

## Objective
The goal of this lab was to identify and exploit a SQL injection vulnerability in the product category filter in a controlled lab environment.

The application uses a query similar to:

SELECT * FROM products WHERE category = 'Gifts' AND released = 1

To solve the lab, the objective was to make the application display unreleased products.

## Vulnerability Description
The product category filter was vulnerable because user input was inserted into the SQL query without proper handling. This allowed the query logic to be modified.

The original query only displays released products because of this condition:

released = 1

By modifying the category parameter, it was possible to change the query logic and retrieve hidden unreleased products.

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Selected a product category and observed that the category value was reflected in the URL.
3. Identified that the category parameter was likely used inside a SQL query.
4. Tested the parameter by modifying the input.
5. Used SQL injection to alter the query condition.
6. The application displayed unreleased products.
7. The lab was successfully solved.

## What I Learned
- SQL injection can happen when user input is directly included in database queries.
- A vulnerable WHERE clause can allow attackers to change the logic of a query.
- SQL injection may expose data that should not normally be visible.
- Input handling and secure query design are important to protect web applications.

## Security Impact
This type of vulnerability can allow unauthorized users to access hidden or restricted data. In a real-world application, this could expose private records, unpublished content, or sensitive business information.

## Mitigation
To prevent this vulnerability, developers should:
- Use parameterized queries or prepared statements.
- Avoid directly concatenating user input into SQL queries.
- Validate and sanitize user input.
- Apply least privilege to database accounts.
- Avoid exposing detailed database errors to users.

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Browser
- Web browser
