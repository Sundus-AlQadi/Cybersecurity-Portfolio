# Day 05 Progress - Authentication Brute-Force Protection and Account Lock Enumeration

## Completed Labs
- Authentication Lab 04: Broken brute-force protection - IP block
- Authentication Lab 05: Username enumeration via account lock

## Topics Covered
- Brute-force protection logic flaws
- IP-based blocking
- Failed login counters
- Counter reset behavior
- Account lock logic flaws
- Username enumeration through account lock behavior
- Account lock messages
- Burp Intruder Pitchfork attack
- Burp Intruder Cluster bomb attack
- Payload alignment
- Repeated payload testing
- Grep-Match rules
- Resource pool configuration
- Identifying successful login attempts using `302` responses
- Identifying valid usernames through response behavior

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Burp Proxy
- Burp Intruder
- Grep-Match
- Resource Pool
- GitHub

## Reflection
Today I completed two authentication labs focused on brute-force protection and account lock behavior.

In the first lab, I learned how brute-force protection can fail when the failed login counter is reset after a successful login. I used Burp Intruder with a Pitchfork attack to alternate between a valid login and password attempts against the victim account.

In the second lab, I learned how account lock behavior can reveal valid usernames. Although account locking is intended to protect accounts, inconsistent lock responses can create username enumeration vulnerabilities. I used repeated payloads, Grep-Match rules, and resource pool settings to identify the valid username and then find the correct password.

These labs showed me that authentication protections must be carefully designed because security controls can become vulnerabilities if their behavior leaks information.

## Current Progress
- Completed SQL Injection basics and UNION-based labs
- Completed SQL Injection database enumeration basics
- Started Authentication vulnerabilities
- Completed username enumeration labs using:
  - Different responses
  - Subtle response differences
  - Response timing
  - Account lock behavior
- Completed brute-force protection logic flaw labs

## Next Step
Continue Authentication vulnerabilities, focusing on 2FA bypasses and password reset logic vulnerabilities.
