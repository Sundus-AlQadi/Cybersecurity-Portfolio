# Lab 03: Username Enumeration via Response Timing

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
Authentication Vulnerabilities / Username Enumeration / Response Timing / Password Brute Force

## Lab Status
Solved

## Objective
The goal of this lab was to identify a valid username using response timing differences, brute-force the user's password using a provided wordlist, and access the account page in a controlled lab environment.

## Simple Explanation
In this lab, the application did not reveal the valid username through clear error messages or obvious response differences.

Instead, the valid username was identified by observing response timing. When an invalid username was submitted, the application responded quickly. However, when a valid username was submitted with a long password, the response took longer because the application spent additional time processing the password.

The lab also used IP-based brute-force protection. This was bypassed in the lab environment by adding and changing the `X-Forwarded-For` header in each request.

## Vulnerability Description
The application was vulnerable because its login response time differed depending on whether the username was valid.

Even though the error message appeared generic, the timing difference leaked information about valid usernames.

The application also relied on IP-based brute-force protection, which could be bypassed because it trusted the `X-Forwarded-For` header.

## Key Concept
Username enumeration can happen through timing differences.

A valid username may cause the application to perform additional password processing, especially when a long password is submitted. This can make the response time noticeably longer compared to invalid usernames.

The `X-Forwarded-For` header can sometimes be abused in lab environments if the application trusts it for identifying the client IP address.

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Submitted an invalid username and password.
3. Located the `POST /login` request in Burp Suite HTTP history.
4. Sent the request to Burp Intruder.
5. Added the `X-Forwarded-For` header to the request.
6. Set the attack type to Pitchfork.
7. Added a payload position to the `X-Forwarded-For` header.
8. Added a payload position to the username parameter.
9. Used a long static password to increase timing differences.
10. Used a numbers payload for `X-Forwarded-For` to vary the client IP value.
11. Used the candidate usernames list for the username payload.
12. Started the attack and analyzed response timing columns.
13. Identified the valid username by finding the response with a noticeably longer completion time.
14. Started a second Intruder attack using the identified username.
15. Added payload positions to the `X-Forwarded-For` header and password parameter.
16. Used the candidate passwords list to test the password.
17. Identified the successful login attempt by finding the response with a `302` status code.
18. Logged in using the identified credentials.
19. The lab was successfully solved.

## Result
The valid username was identified through response timing differences.

The correct password was identified from the request that returned a `302` response, indicating a successful login redirect.

## What I Learned
- Username enumeration can happen through response timing differences.
- Generic error messages are not always enough to prevent information leakage.
- Long passwords can increase timing differences when the username is valid.
- Burp Intruder can be used to compare response timing across many requests.
- IP-based brute-force protection can be weak if the application trusts user-controlled headers such as `X-Forwarded-For`.
- A `302` response can indicate successful authentication when the application redirects after login.

## Security Impact
In a real-world application, timing differences can allow attackers to identify valid usernames even when error messages are generic.

Once a valid username is identified, attackers may attempt password guessing, brute-force attacks, or credential stuffing.

If IP-based brute-force protection relies on untrusted headers, attackers may bypass rate limits.

## Mitigation
To prevent this vulnerability, developers should:

- Ensure authentication responses take consistent time for valid and invalid usernames.
- Use generic and consistent error messages.
- Apply rate limiting based on trusted client information.
- Do not trust `X-Forwarded-For` unless it is set by a trusted proxy.
- Implement account lockout or progressive delays.
- Monitor repeated failed login attempts.
- Use multi-factor authentication.
- Enforce strong password policies.

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Burp Proxy
- Burp Intruder
- Web browser
