## Lab 18: SQL Injection with Filter Bypass via XML Encoding

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

SQL Injection / UNION-Based SQL Injection / XML Encoding / Filter Bypass

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a SQL injection vulnerability in the stock check feature, bypass the application's filter using XML encoding, retrieve the administrator user's credentials, and log in as the administrator.

### Simple Explanation

In this lab, the SQL injection vulnerability was located in the stock check feature.

The application sent the `productId` and `storeId` values in XML format.

The vulnerable input was the `storeId` value.

A normal SQL injection payload was blocked by the application's filter because it detected keywords such as `UNION` and `SELECT`.

To bypass this filter, the payload was encoded using XML entities.

The application decoded the XML entities before processing the SQL query, allowing the SQL injection payload to execute successfully.

### Vulnerability Description

The application accepted XML input in the stock check feature and inserted the `storeId` value into a SQL query without secure handling.

Although the application attempted to block SQL injection keywords, the filter could be bypassed using XML entity encoding.

This allowed a UNION-based SQL injection attack to retrieve data from the `users` table.

### Key Concept

Filters that only check raw input can be bypassed if the application later decodes or transforms the input before using it in a SQL query.

In this lab, XML entities were used to hide SQL keywords from the filter.

For example, encoded XML characters can be decoded by the application before reaching the database.

This allowed the payload to bypass the filter and execute as SQL.

### Steps Taken

Opened the lab using Burp Suite's built-in browser.

Used the product stock check feature.

Located the `POST /product/stock` request in Burp Proxy HTTP history.

Sent the request to Burp Repeater.

Identified that the request body used XML format.

Tested the `storeId` parameter with a mathematical expression.

Confirmed that the input was being evaluated by the backend.

Tried a basic UNION SELECT payload.

Observed that the request was blocked by the application's filter.

Used XML entity encoding to obfuscate the SQL injection payload.

Confirmed that the encoded payload bypassed the filter.

Determined that the original query returned one column.

Used string concatenation to combine usernames and passwords into a single column.

Retrieved usernames and passwords from the `users` table.

Identified the administrator user's password.

Logged in as the administrator user.

Successfully solved the lab.

### Important Payload Logic

The original SQL injection payload was:

```sql
1 UNION SELECT username || '~' || password FROM users
```

Because the filter blocked SQL keywords, the payload was encoded using XML entities before being inserted into the `storeId` element.

The final encoded payload was placed inside:

```xml
<storeId>ENCODED_PAYLOAD_HERE</storeId>
```

### Example Vulnerable Request Area

The vulnerable XML input was:

```xml
<storeId>1</storeId>
```

A basic test was performed using:

```xml
<storeId>1+1</storeId>
```

This confirmed that the input was being evaluated by the backend.

### Filter Bypass

A normal UNION payload was blocked:

```xml
<storeId>1 UNION SELECT NULL</storeId>
```

To bypass the filter, XML entity encoding was used.

The encoded payload was decoded by the application before reaching the database, allowing the SQL query to execute.

### Data Extraction

Since the query returned only one column, the username and password values were combined using string concatenation:

```sql
username || '~' || password
```

This produced output in the following format:

```text
username~password
```

### Recovered Password

The administrator password was successfully recovered:

```text
802nu5z7rwsb05z3mni4
```

### Result

The administrator credentials were successfully retrieved using UNION-based SQL injection with XML encoding filter bypass.

The recovered password was used to log in as the administrator user and solve the lab.

### What I Learned

SQL injection can exist in XML-based input, not only in URL parameters or form fields.

Filters can sometimes be bypassed using encoding techniques.

XML entities can hide SQL keywords from weak input filters.

Applications may decode encoded input before sending it to the database.

UNION-based SQL injection can retrieve data when query results are returned in the response.

When only one column is returned, concatenation can combine multiple values into a single output.

`username || '~' || password` can be used to display credentials together in one column.

Security filters should not be the main defense against SQL injection.

### Security Impact

In a real-world application, this vulnerability could allow attackers to bypass input filters and extract sensitive information from the database.

This could include usernames, passwords, session tokens, API keys, or personal data.

It also shows that weak filtering is not a reliable defense if user input is still inserted directly into SQL queries.

### Mitigation

To prevent this vulnerability, developers should:

Use parameterized queries or prepared statements.

Avoid inserting user-controlled input directly into SQL queries.

Validate and sanitize XML input securely.

Do not rely only on keyword-based filters or blacklists.

Normalize and decode input before validation.

Use secure XML parsing configurations.

Apply least-privilege database permissions.

Monitor suspicious input patterns and blocked requests.

Use allowlist validation where possible.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Web Browser
