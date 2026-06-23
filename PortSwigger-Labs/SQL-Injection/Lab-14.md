## Lab 14: Blind SQL Injection with Time Delays

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

SQL Injection / Blind SQL Injection / Time-Based SQL Injection

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a blind SQL injection vulnerability in the `TrackingId` cookie and cause the application to delay its response for 10 seconds.

### Simple Explanation

In this lab, the application did not display SQL query results.

It also did not respond differently when a condition was true or false, and it did not show useful database errors.

However, the SQL query was executed synchronously, meaning the application waited for the database query to finish before sending the response.

By injecting a database sleep function, it was possible to make the application delay its response.

If the response took around 10 seconds, this confirmed that the injected SQL command was executed.

### Vulnerability Description

The application used the value of the `TrackingId` cookie inside a SQL query without secure handling.

Although the query results were hidden, the application response time could be manipulated by injecting a time delay function.

This created a time-based blind SQL injection vulnerability.

### Key Concept

Time-based blind SQL injection relies on response delay instead of visible output, error messages, or response differences.

The attacker injects a command that makes the database wait for a specific amount of time.

If the application response is delayed, this confirms that the SQL injection payload was executed by the database.

### Steps Taken

Opened the lab using Burp Suite's built-in browser.

Visited the front page of the shop.

Intercepted the request containing the `TrackingId` cookie.

Sent the request to Burp Repeater.

Modified the `TrackingId` cookie value.

Injected a PostgreSQL time delay function using `pg_sleep(10)`.

Sent the modified request.

Observed that the application response was delayed by approximately 10 seconds.

Confirmed that the SQL injection payload was successfully executed.

Successfully solved the lab.

### Important Payload

The payload used was:

```sql
x'||pg_sleep(10)--
```

The full cookie value looked like:

```http
TrackingId=x'||pg_sleep(10)--
```

### Payload Explanation

The payload can be broken down as follows:

```text
x
```

A normal value used as part of the original `TrackingId`.

```text
'
```

Closes the original single-quoted string inside the SQL query.

```text
||
```

Concatenates the injected function with the original query in PostgreSQL.

```sql
pg_sleep(10)
```

Tells the PostgreSQL database to wait for 10 seconds.

```text
--
```

Comments out the remaining part of the original SQL query to avoid syntax errors.

### Result

The application response was delayed by approximately 10 seconds.

This confirmed that the SQL injection payload was executed successfully.

The lab was solved.

### What I Learned

Blind SQL injection can be detected even when there are no visible query results.

Not all blind SQL injection depends on true or false responses.

Response time can be used as an indirect indicator of successful SQL injection.

PostgreSQL supports the `pg_sleep()` function to delay query execution.

Time-based SQL injection is useful when the application hides errors and does not change its visible response.

Burp Repeater can be used to observe response timing and confirm time delays.

### Security Impact

In a real-world application, time-based blind SQL injection could allow attackers to confirm SQL injection vulnerabilities and potentially extract sensitive data by asking conditional time-delay questions.

Even if the application hides errors and query results, response timing can still leak information from the database.

### Mitigation

To prevent this vulnerability, developers should:

Use parameterized queries or prepared statements.

Avoid inserting user-controlled input directly into SQL queries.

Validate and sanitize all user-controlled input, including cookies.

Apply least-privilege database permissions.

Monitor unusual delays and repetitive suspicious requests.

Use secure error handling and avoid exposing backend behavior.

Set reasonable database query timeout limits.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Web Browser

