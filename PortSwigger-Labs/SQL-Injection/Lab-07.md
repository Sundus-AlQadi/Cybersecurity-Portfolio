# Lab 07: SQL Injection Attack - Querying the Database Type and Version on MySQL and Microsoft

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
SQL Injection / UNION-Based SQL Injection / Database Enumeration

## Lab Status
Solved

## Objective
The goal of this lab was to use a UNION-based SQL injection vulnerability to display the database version string in a controlled lab environment.

## Simple Explanation
The application had a SQL injection vulnerability in the product category filter.

In previous labs, I used UNION SELECT to retrieve data from other tables. In this lab, the goal was different: instead of retrieving user data, I queried the database itself to identify its type and version.

For MySQL and Microsoft SQL Server, the database version can be displayed using:

```text
@@version
