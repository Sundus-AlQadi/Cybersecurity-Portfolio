# Lab 04: SQL Injection UNION Attack - Finding a Column Containing Text

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
SQL Injection / UNION-Based SQL Injection

## Lab Status
Solved

## Objective
The goal of this lab was to identify which column returned by the original SQL query was compatible with string data.

This is an important step in UNION-based SQL injection because text values can only be displayed correctly in columns that support string data types.

## Simple Explanation

In the previous lab, I determined that the original query returned three columns.

However, knowing the number of columns is not enough. To display text using UNION SELECT, I also needed to find which one of the returned columns could accept string/text data.

For example, a product query may return columns such as:

```text
id | name | price
