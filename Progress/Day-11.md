# Day 11 Progress - Oracle Database Enumeration and Blind SQL Injection

## Completed Labs

* SQL Injection Lab 09: Querying the Database Type and Version on Oracle
* SQL Injection Lab 10: Listing the Database Contents on Oracle
* SQL Injection Lab 11: Blind SQL Injection with Conditional Responses

## Topics Covered

* SQL Injection
* UNION-based SQL Injection
* Oracle database enumeration
* Oracle metadata tables
* Querying database version information
* Listing database tables on Oracle
* Listing database columns on Oracle
* Extracting usernames and passwords
* Blind SQL Injection
* Boolean-based SQL Injection
* Conditional responses
* Tracking cookie injection
* True/false condition testing
* Password length enumeration
* Character-by-character data extraction
* Burp Intruder payload testing
* Grep Match response analysis

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Burp Intruder
* Web Browser
* GitHub

## Reflection

Today I continued the SQL Injection section and completed three labs focused on Oracle database enumeration and Blind SQL Injection.

In the first lab, I learned how SQL injection can be used to identify the database type and version on Oracle. I used Oracle-specific metadata by querying `v$version` and retrieving the version details from the `BANNER` column. This helped reinforce that different database systems use different syntax and metadata locations.

In the second lab, I practiced listing database contents on Oracle. Unlike non-Oracle databases that use `information_schema`, Oracle stores metadata in tables such as `all_tables` and `all_tab_columns`. I used these metadata tables to discover the user credentials table, identify its username and password columns, and retrieve administrator credentials.

In the third lab, I completed my first Blind SQL Injection lab. Unlike UNION-based SQL injection, the application did not display query results or database errors. Instead, it displayed a `Welcome back` message when an injected condition was true. I used this behavior to ask the database true/false questions, determine the administrator password length, and extract the password one character at a time using `SUBSTRING()` and Burp Intruder.

These labs helped me understand the difference between visible SQL injection and blind SQL injection. They also showed how attackers can still extract sensitive data even when query results are hidden, as long as the application behavior changes based on database conditions.

## Current Progress

* Completed SQL Injection fundamentals
* Completed UNION-based SQL Injection labs
* Completed database type and version enumeration labs
* Completed database content listing on non-Oracle and Oracle databases
* Completed first Blind SQL Injection lab
* Completed 12 Authentication labs (all Apprentice and Practitioner labs completed; 2 Expert labs remaining)
* Completed all 13 Access Control labs

## Key Takeaways

* Oracle uses different metadata tables than non-Oracle databases.
* `v$version` can be used to retrieve Oracle database version information.
* `all_tables` can be used to discover table names in Oracle.
* `all_tab_columns` can be used to discover column names in Oracle.
* UNION-based SQL injection retrieves data directly when query output is visible.
* Blind SQL injection relies on indirect application behavior.
* Conditional responses can reveal whether injected SQL conditions are true or false.
* `LENGTH()` can help determine the length of sensitive values.
* `SUBSTRING()` can extract sensitive values one character at a time.
* Burp Intruder is useful for automating repetitive character testing.

## Milestone Achieved

* Completed Oracle database enumeration labs
* Completed first Blind SQL Injection lab

Topics Strengthened:

* Database enumeration
* Oracle SQL injection syntax
* Metadata extraction
* Boolean-based blind SQL injection
* Burp Intruder automation
* Response-based inference

## Next Step

Continue the SQL Injection section, focusing on Blind SQL Injection with conditional errors, visible error-based SQL injection, time-delay techniques, and out-of-band SQL injection.
