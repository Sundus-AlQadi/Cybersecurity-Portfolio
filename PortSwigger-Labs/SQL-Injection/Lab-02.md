# Lab 02: SQL Injection Vulnerability Allowing Login Bypass

## Platform
PortSwigger Web Security Academy

## Difficulty
Apprentice

## Topic
SQL Injection / Authentication Bypass

## Lab Status
Solved

## Objective
The goal of this lab was to identify a SQL injection vulnerability in the login function and use it to log in as the administrator user in a controlled lab environment.

## Vulnerability Description
The login function was vulnerable because user input in the username field was not handled securely before being included in the SQL query.

This allowed the query logic to be modified so that the application authenticated the administrator user without needing the correct password.

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Accessed the login page.
3. Used Burp Suite to intercept the login request.
4. Modified the username parameter in the intercepted request.
5. Sent the modified request.
6. The application logged in as the administrator user.
7. The lab was successfully solved.

## What I Learned
- SQL injection can affect authentication mechanisms.
- Login forms are high-risk areas because they directly interact with user credentials.
- SQL comments can be used to ignore the remaining part of a vulnerable query in a lab environment.
- Authentication bypass can happen when user input is directly included in SQL queries.

## Security Impact
In a real-world application, this type of vulnerability could allow unauthorized users to access privileged accounts, including administrator accounts. This can lead to data exposure, account takeover, and full application compromise.

## Mitigation
To prevent this vulnerability, developers should:
- Use parameterized queries or prepared statements.
- Never concatenate user input directly into SQL queries.
- Apply secure authentication logic.
- Implement proper input validation.
- Use least privilege for database accounts.
- Monitor and log suspicious login attempts.

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite
- Burp Proxy
- Web browser
