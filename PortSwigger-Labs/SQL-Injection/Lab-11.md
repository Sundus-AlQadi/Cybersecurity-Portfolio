# Lab 11: Blind SQL Injection with Conditional Responses

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

SQL Injection / Blind SQL Injection / Boolean-Based SQL Injection

## Lab Status

Solved

## Objective

The goal of this lab was to exploit a blind SQL injection vulnerability and recover the administrator user's password by observing differences in application responses.

## Simple Explanation

Unlike previous SQL injection labs, the application did not display query results or database errors.

Instead, it displayed a "Welcome back" message whenever a SQL condition evaluated to true.

By asking the database a series of true/false questions, it was possible to determine the password length and then extract each character individually.

## Vulnerability Description

The TrackingId cookie was incorporated into a SQL query without secure handling.

Although query results were not displayed, the application's response changed depending on whether the injected condition evaluated to true or false.

This created a Boolean-based blind SQL injection vulnerability.

## Key Concept

Blind SQL injection relies on indirect feedback from the application.

Instead of retrieving data directly, attackers infer information by observing differences in responses to true and false conditions.

## Steps Taken

1. Intercepted the request containing the TrackingId cookie.
2. Confirmed that injected boolean conditions affected the application's response.
3. Verified the existence of the users table.
4. Verified the existence of the administrator user.
5. Determined the length of the administrator password.
6. Used the SUBSTRING() function to test individual password characters.
7. Used Burp Intruder to automate character testing.
8. Recovered the administrator password one character at a time.
9. Logged in as the administrator user.
10. Successfully solved the lab.

## Result

The administrator password was successfully extracted through Boolean-based blind SQL injection and used to access the administrator account.

## What I Learned

- Blind SQL injection does not require visible query results.
- Application behavior can leak information.
- Boolean conditions can be used to extract sensitive data.
- LENGTH() can be used to determine string length.
- SUBSTRING() can be used to extract individual characters.
- Burp Intruder can automate repetitive testing.

## Security Impact

In a real-world application, attackers could extract sensitive information such as credentials, API keys, session tokens, or personal data even when query results are hidden.

## Mitigation

To prevent this vulnerability, developers should:

- Use parameterized queries or prepared statements.
- Avoid directly inserting user input into SQL queries.
- Validate and sanitize user-controlled input.
- Apply least-privilege database permissions.
- Monitor suspicious request patterns.
- Avoid response differences that reveal query outcomes.

## Tools Used

- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Burp Proxy
- Burp Repeater
- Burp Intruder
- Web Browser
