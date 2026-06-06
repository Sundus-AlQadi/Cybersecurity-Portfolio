# Lab 05: SQL Injection UNION Attack - Retrieving Data from Other Tables

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
SQL Injection / UNION-Based SQL Injection / Data Retrieval

## Lab Status
Solved

## Objective
The goal of this lab was to use a UNION-based SQL injection vulnerability to retrieve usernames and passwords from a different database table, then use the retrieved administrator credentials to log in as the administrator user in a controlled lab environment.

## Simple Explanation
The application had a SQL injection vulnerability in the product category filter.

The page normally displayed product information from a products table. However, because the query results were shown in the application's response, it was possible to use UNION SELECT to include results from another table.

In this lab, the database contained a table named:

```text
users
