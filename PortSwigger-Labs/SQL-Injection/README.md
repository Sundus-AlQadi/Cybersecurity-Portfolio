# SQL Injection Vulnerabilities

This folder contains selected documentation from the SQL Injection learning path in PortSwigger Web Security Academy.

The learning path covered SQL injection testing concepts and practical labs related to authentication bypass, UNION-based SQL injection, database enumeration, Oracle-specific SQL injection, blind SQL injection, error-based SQL injection, time-based SQL injection, out-of-band SQL injection concepts, and filter bypass using XML encoding.

## Topics Covered

SQL Injection

WHERE clause manipulation

Authentication bypass using SQL injection

UNION-based SQL injection

Finding the number of returned columns

Finding text-compatible columns

Retrieving data from other database tables

Retrieving multiple values in a single column

String concatenation

Database type and version enumeration

Non-Oracle database enumeration

Oracle database enumeration

Oracle metadata tables

Blind SQL Injection

Boolean-based blind SQL injection

Conditional responses

Conditional errors

Visible error-based SQL injection

Time-based blind SQL injection

Time-based information retrieval

Out-of-band SQL injection concepts

Burp Collaborator tool requirement

XML-based input injection

Filter bypass using XML encoding

## Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Burp Intruder

Burp Decoder

Web Browser

GitHub

## Skills Practiced

Identifying SQL injection points

Testing user-controlled input

Manipulating HTTP requests

Testing cookies, URL parameters, form fields, and XML input

Using UNION SELECT attacks

Determining column count

Identifying text-compatible columns

Extracting usernames and passwords

Querying database version information

Enumerating database tables and columns

Using Oracle-specific syntax

Using metadata tables such as `information_schema`, `all_tables`, and `all_tab_columns`

Using blind SQL injection logic

Testing true and false SQL conditions

Using response differences to infer data

Using errors as indirect feedback

Using time delays as indirect feedback

Using `LENGTH()` to determine password length

Using `SUBSTRING()` and `SUBSTR()` to extract data character by character

Using Burp Intruder for repetitive payload testing

Analyzing response status codes

Analyzing response timing

Bypassing weak filters using XML encoding

Documenting findings, impact, and mitigations

## Completed Labs

* Lab 01: SQL Injection Vulnerability in WHERE Clause
* Lab 02: SQL Injection Vulnerability Allowing Login Bypass
* Lab 03: SQL Injection UNION Attack - Determining the Number of Columns
* Lab 04: SQL Injection UNION Attack - Finding a Text-Compatible Column
* Lab 05: SQL Injection UNION Attack - Retrieving Data from Other Tables
* Lab 06: SQL Injection UNION Attack - Retrieving Multiple Values in a Single Column
* Lab 07: SQL Injection Attack - Querying the Database Type and Version
* Lab 08: SQL Injection Attack - Listing Database Contents on Non-Oracle Databases
* Lab 09: SQL Injection UNION Attack - Querying the Database Type and Version on Oracle
* Lab 10: SQL Injection UNION Attack - Listing Database Contents on Oracle
* Lab 11: Blind SQL Injection with Conditional Responses
* Lab 12: Blind SQL Injection with Conditional Errors
* Lab 13: Visible Error-Based SQL Injection
* Lab 14: Blind SQL Injection with Time Delays
* Lab 15: Blind SQL Injection with Time Delays and Information Retrieval
* Lab 18: SQL Injection with Filter Bypass via XML Encoding

## Pending Labs

* Lab 16: Blind SQL Injection with Out-of-Band Interaction
* Lab 17: Blind SQL Injection with Out-of-Band Data Exfiltration

These labs were reviewed but marked as pending because they require Burp Collaborator access to verify DNS or HTTP out-of-band interactions.

The current setup uses Burp Suite Community Edition, so these labs will be revisited later when Collaborator access is available.

## What I Learned

SQL injection happens when user-controlled input is inserted into SQL queries without secure handling.

SQL injection can affect confidentiality, integrity, and availability.

Authentication logic can be bypassed if login inputs are not handled securely.

UNION-based SQL injection can retrieve data directly when query results are visible in the application's response.

The `UNION` operator requires both queries to return the same number of columns.

Text-compatible columns are needed to display string data such as usernames and passwords.

When only one text-compatible column is available, multiple values can be combined using concatenation.

Database enumeration helps identify the database type, version, table names, and column names.

Different database systems use different syntax and metadata locations.

Non-Oracle databases often use `information_schema` for metadata.

Oracle databases use metadata sources such as `all_tables`, `all_tab_columns`, `v$version`, and `dual`.

Blind SQL injection can still extract sensitive data even when query results are hidden.

Boolean-based blind SQL injection uses true and false application behavior.

Conditional error-based SQL injection uses database errors as true or false indicators.

Visible error-based SQL injection can leak sensitive values directly through verbose database errors.

Time-based blind SQL injection uses response delay as an indirect signal.

The PostgreSQL `pg_sleep()` function can be used to confirm SQL execution and infer data through timing.

`LENGTH()` can determine the length of sensitive values.

`SUBSTRING()` or `SUBSTR()` can extract sensitive values one character at a time.

Burp Intruder is useful for automating repetitive character testing.

Timing-based attacks require careful request handling, such as using one concurrent request.

Out-of-band SQL injection can be used when the application gives no visible feedback.

Burp Collaborator is used to detect DNS or HTTP interactions in out-of-band labs.

Input filters and WAF-style keyword blocking are not reliable defenses against SQL injection.

XML entity encoding can bypass weak filters if the application decodes input before processing it.

Parameterized queries are the correct defense against SQL injection.

## Security Impact

SQL injection vulnerabilities can allow attackers to:

View unauthorized data

Bypass authentication

Extract usernames and passwords

Access administrator accounts

Enumerate database structure

Retrieve sensitive personal or business data

Modify or delete data

Cause application errors or delays

Potentially affect backend systems depending on database permissions and configuration

## Mitigation Summary

To prevent SQL injection, developers should:

Use parameterized queries or prepared statements.

Avoid concatenating user input into SQL queries.

Validate and sanitize all user-controlled input.

Use allowlist validation where possible.

Avoid relying only on keyword filters or blacklists.

Normalize and decode input before validation.

Disable verbose database errors in production.

Apply least-privilege database permissions.

Use secure XML parsing configurations.

Monitor suspicious SQL patterns and repeated attack attempts.

Restrict unnecessary outbound network access from backend systems.

## Notes

All labs were completed in controlled PortSwigger Web Security Academy environments for educational purposes.

The out-of-band labs are currently marked as pending due to Burp Collaborator access requirements and will be revisited later.
