# SQL Injection Notes

## What is SQL Injection?
SQL Injection is a web security vulnerability that allows an attacker to interfere with the database queries made by an application.

## Possible Impact
- Viewing unauthorized data
- Modifying or deleting data
- Bypassing application logic
- In some cases, affecting the back-end infrastructure

## What I Learned
- SQL Injection happens when user input is inserted into SQL queries without proper validation or parameterization.
- It can affect confidentiality, integrity, and availability.
- Secure coding practices are needed to prevent it.

## Lab Practice

### Lab 01: WHERE Clause SQL Injection
In this lab, I practiced how SQL injection can modify a WHERE clause and change the query logic to reveal hidden data. The main lesson was that insecure handling of user input can allow unauthorized access to restricted information.

## Lab 02: Login Bypass
In this lab, I practiced how SQL injection can affect authentication logic. The main lesson was that insecure handling of login input can allow unauthorized access to privileged accounts. This showed why login forms must use parameterized queries and secure authentication checks.

## Lab 03: UNION-Based SQL Injection

UNION-based SQL injection is used when the results of the injected query are returned in the application's response.

The UNION operator combines the results of two SELECT queries. However, both SELECT queries must return the same number of columns.

A column is like a field in a database table. For example, a products table may contain columns such as:

```text
id | name | price

## Lab 04: Finding a Text-Compatible Column in UNION-Based SQL Injection

After determining the number of columns returned by the original query, the next step is to identify which column can display string data.

This is important because different database columns may have different data types, such as:

```text
id | name | price
