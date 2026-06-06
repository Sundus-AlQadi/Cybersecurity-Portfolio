# Lab 06: SQL Injection UNION Attack - Retrieving Multiple Values in a Single Column

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
SQL Injection / UNION-Based SQL Injection / String Concatenation

## Lab Status
Solved

## Objective
The goal of this lab was to use a UNION-based SQL injection vulnerability to retrieve usernames and passwords from the users table when only one returned column was compatible with text data.

## Simple Explanation
In previous UNION-based SQL injection labs, I retrieved values using separate text-compatible columns.

In this lab, the query returned two columns, but only one column was compatible with string/text data. This meant that I could not place the username in one text column and the password in another text column.

To solve this, I combined the username and password into a single text value using string concatenation.

The values were combined in the following format:

```text
username~password
