# Lab 02: Username Enumeration via Subtly Different Responses

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
Authentication Vulnerabilities / Username Enumeration / Password Brute Force

## Lab Status
Solved

## Objective
The goal of this lab was to identify a valid username through subtle differences in login error responses, brute-force the user's password using a provided wordlist, and access the user account page in a controlled lab environment.

## Simple Explanation
In this lab, the application returned almost identical error messages for invalid login attempts.

The difference was very small, such as a typo, punctuation difference, or trailing space in the error message. Because the difference was not obvious visually, Burp Intruder's Grep - Extract feature was used to extract and compare the error message across responses.

After identifying the valid username, the password was tested using the provided password wordlist. The successful login attempt returned a redirect response.

## Vulnerability Description
The application was vulnerable because it returned subtly different responses depending on whether the username was valid or invalid.

Even though the difference was small, it still allowed username enumeration.

## Key Concept
Username enumeration can happen even when the application appears to use a generic error message.

Small differences can reveal valid usernames, such as:

- Missing punctuation
- Extra spaces
- Different response lengths
- Slightly different wording
- Different status codes

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Submitted an invalid username and password.
3. Found the `POST /login` request in Burp Suite HTTP history.
4. Sent the request to Burp Intruder.
5. Set the username parameter as the payload position.
6. Added the candidate usernames list as the payload list.
7. Used Grep - Extract to extract the login error message from each response.
8. Started the Intruder attack.
9. Compared the extracted error messages.
10. Identified one response with a subtle difference in the error message.
11. Used the corresponding payload as the valid username.
12. Set the valid username as a static value.
13. Set the password parameter as the payload position.
14. Used the candidate passwords list.
15. Identified the successful login attempt by finding the response with a `302` status code.
16. Logged in using the identified credentials.
17. The lab was successfully solved.

## Result
A valid username was identified by comparing subtle differences in login error messages.

The correct password was identified by analyzing the Intruder results and finding the request that returned a `302` response.

## What I Learned
- Username enumeration can happen through very small response differences.
- Generic error messages must be implemented consistently.
- Burp Intruder's Grep - Extract feature helps compare response content efficiently.
- Response status codes can help identify successful authentication attempts.
- Subtle authentication flaws can still create serious security risks.

## Security Impact
In a real-world application, subtle response differences can allow attackers to identify valid usernames.

Once valid usernames are known, attackers can target those accounts with password guessing, brute-force attacks, or credential stuffing.

## Mitigation
To prevent this vulnerability, developers should:

- Use fully generic and consistent login error messages.
- Ensure responses are identical for invalid usernames and incorrect passwords.
- Apply rate limiting.
- Implement account lockout where appropriate.
- Monitor repeated failed login attempts.
- Use multi-factor authentication.
- Avoid subtle differences in response body, length, timing, or status code.

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Burp Proxy
- Burp Intruder
- Grep - Extract
- Web browser
