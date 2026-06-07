# Day 03 Progress - SQL Injection Database Enumeration and Authentication Basics

## Completed Labs
- Lab 08: SQL Injection attack - listing the database contents on non-Oracle databases
- Authentication Lab 01: Username enumeration via different responses

## Topics Covered
- Database enumeration using SQL Injection
- Listing database tables
- Listing database columns
- Using `information_schema`
- Retrieving credentials after identifying table and column names
- Authentication vulnerabilities
- Username enumeration
- Password brute-force using a provided wordlist
- Using Burp Intruder
- Analyzing response length and status codes

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Burp Proxy
- Burp Intruder
- GitHub

## Reflection
Today I completed a SQL Injection lab focused on database enumeration and started the Authentication vulnerabilities section.

In the SQL Injection lab, I practiced identifying database tables and columns when the database structure was unknown. In the Authentication lab, I learned how different login responses can reveal valid usernames and how response length and status codes can help identify successful authentication attempts.

This helped me understand how authentication weaknesses can support account attacks if proper protections are not implemented.

## Current Progress
- Completed SQL Injection basics
- Completed UNION-based SQL Injection labs
- Completed database enumeration basics
- Started Authentication vulnerability labs
- Practiced Burp Intruder for username enumeration and password testing

## Next Step
Continue Authentication vulnerabilities, focusing on login weaknesses, brute-force protections, and password reset vulnerabilities.
