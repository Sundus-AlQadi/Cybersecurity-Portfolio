# Day 12 Progress - Error-Based and Time-Based Blind SQL Injection

## Completed Labs

* SQL Injection Lab 12: Blind SQL Injection with Conditional Errors
* SQL Injection Lab 13: Visible Error-Based SQL Injection
* SQL Injection Lab 14: Blind SQL Injection with Time Delays
* SQL Injection Lab 15: Blind SQL Injection with Time Delays and Information Retrieval

## Topics Covered

* SQL Injection
* Blind SQL Injection
* Conditional error-based SQL injection
* Visible error-based SQL injection
* Time-based blind SQL injection
* Tracking cookie injection
* SQL error behavior analysis
* Triggering database errors conditionally
* Using errors as true/false indicators
* Using response time as an inference method
* PostgreSQL `pg_sleep()` function
* PostgreSQL time-delay payloads
* `CASE WHEN` conditional logic
* Password length enumeration
* Character-by-character password extraction
* `LENGTH()` function
* `SUBSTRING()` function
* Burp Intruder timing analysis
* Single-threaded Intruder attacks
* Response timing comparison
* Sensitive data extraction without visible query output

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Burp Intruder
* Web Browser
* GitHub

## Reflection

Today I continued the SQL Injection section and completed four labs focused on advanced blind SQL injection techniques, error-based SQL injection, and time-based information retrieval.

In the first lab, I worked on Blind SQL Injection with Conditional Errors. The application did not display query results or normal true/false differences, but it returned an error when the SQL query caused a database error. I used `CASE WHEN` logic with a deliberate error to make the application return an error only when a condition was true. This helped me understand how database errors can be used as an indirect signal to extract hidden information.

In the second lab, I completed a Visible Error-Based SQL Injection lab. Unlike blind SQL injection, the application displayed detailed database error messages. I used the `CAST()` function to force the database to convert text values into integers. Since usernames and passwords are text, the conversion failed and the error message leaked the sensitive value directly. This showed how dangerous verbose database errors can be in real applications.

In the third lab, I practiced Blind SQL Injection with Time Delays. The goal was to confirm the vulnerability by making the application delay its response for 10 seconds. I used the PostgreSQL `pg_sleep(10)` function inside the `TrackingId` cookie. When the application response was delayed, it confirmed that the injected SQL was executed successfully.

In the fourth lab, I completed Blind SQL Injection with Time Delays and Information Retrieval. This lab used response time as the only feedback mechanism. I used conditional `pg_sleep()` payloads to ask the database true/false questions. If the condition was true, the response was delayed by 10 seconds. If the condition was false, the response returned immediately. I used this method to determine the administrator password length and then extracted the password one character at a time using `SUBSTRING()` and Burp Intruder.

These labs helped me understand that SQL injection can still be exploited even when query results, errors, and visible response differences are hidden. Application behavior, database errors, and response timing can all leak sensitive information if user input is not handled securely.

## Current Progress

* Completed SQL Injection fundamentals
* Completed UNION-based SQL Injection labs
* Completed database type and version enumeration labs
* Completed database content listing on non-Oracle and Oracle databases
* Completed Blind SQL Injection with conditional responses
* Completed Blind SQL Injection with conditional errors
* Completed Visible Error-Based SQL Injection
* Completed Blind SQL Injection with time delays
* Completed Blind SQL Injection with time delays and information retrieval
* Completed 12 Authentication labs
* Completed all 13 Access Control labs

## Key Takeaways

* Blind SQL injection does not require visible query results.
* Application errors can be used as an indirect signal.
* Conditional errors can reveal whether SQL conditions are true or false.
* Verbose database errors can leak sensitive values directly.
* `CAST()` can be abused to trigger type conversion errors.
* Time-based blind SQL injection uses response delay as feedback.
* `pg_sleep()` can be used in PostgreSQL to delay query execution.
* `CASE WHEN` can control whether a delay or error is triggered.
* `LENGTH()` helps determine the length of sensitive values.
* `SUBSTRING()` helps extract sensitive values one character at a time.
* Burp Intruder can automate repetitive password character testing.
* Timing-based attacks require careful configuration, such as using one concurrent request.
* Even hidden database behavior can expose sensitive information if input is not handled securely.

## Milestone Achieved

Completed advanced Blind SQL Injection labs covering:

* Conditional error-based SQL injection
* Visible error-based SQL injection
* Time-based SQL injection
* Time-based information retrieval
* Password extraction using response timing
* Burp Intruder automation for blind data extraction

## Topics Strengthened

* Blind SQL Injection
* Error-based SQL Injection
* Time-based SQL Injection
* PostgreSQL injection techniques
* Conditional SQL logic
* Response-based inference
* Timing-based inference
* Burp Repeater testing
* Burp Intruder automation
* Secure coding awareness

## Next Step

Continue the SQL Injection section, focusing on out-of-band SQL injection techniques and any remaining advanced SQL injection labs.
