# Lab 01: Username Enumeration via Different Responses

## Platform
PortSwigger Web Security Academy

## Difficulty
Apprentice

## Topic
Authentication Vulnerabilities / Username Enumeration / Password Brute Force

## Lab Status
Solved

## Objective
The goal of this lab was to identify a valid username through different login responses, brute-force the user's password using a provided wordlist, and access the account page in a controlled lab environment.

## Simple Explanation
The login page returned different error messages depending on whether the username was valid or invalid.

When an invalid username was submitted, the application returned a response indicating that the username was invalid.

When a valid username was submitted with an incorrect password, the application returned a different response indicating that the password was incorrect.

This difference allowed the valid username to be identified. After that, the password was tested using the provided password wordlist until a successful login response was found.

## Vulnerability Description
The application was vulnerable because it revealed different responses for invalid usernames and incorrect passwords.

This behavior allowed username enumeration. Once a valid username was found, the account could be targeted with password brute-force attempts using the provided wordlist.

## Key Concept
Username enumeration happens when an application reveals whether a username exists.

This can happen through:

- Different error messages
- Different response lengths
- Different status codes
- Different response times

In this lab, the application displayed different login responses, which made it possible to identify a valid username.

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Submitted an invalid username and password on the login page.
3. Located the `POST /login` request in Burp Suite HTTP history.
4. Sent the request to Burp Intruder.
5. Set the username parameter as the payload position.
6. Used the provided candidate usernames list as the payload list.
7. Started the Intruder attack and analyzed the responses.
8. Identified the valid username by noticing a different response message and response length.
9. Set the valid username as a static value.
10. Set the password parameter as the payload position.
11. Used the provided candidate passwords list as the payload list.
12. Identified the correct password by finding the response with a `302` status code.
13. Logged in using the identified credentials.
14. The lab was successfully solved.

## Result
A valid username was identified through different login responses.

The correct password was then identified using the provided candidate password list. The successful login attempt returned a `302` response, indicating a redirect after authentication.

## What I Learned
- Different login error messages can reveal whether a username exists.
- Response length can help identify unusual or different responses.
- A `302` status code can indicate a successful login when the application redirects the user after authentication.
- Username enumeration can make password attacks easier.
- Authentication systems should avoid revealing whether the username or password was incorrect.

## Security Impact
In a real-world application, username enumeration can help attackers identify valid accounts.

Once valid usernames are known, attackers can target those accounts with password guessing, brute-force attempts, or credential stuffing attacks.

## Mitigation
To prevent this vulnerability, developers should:

- Use generic login error messages.
- Avoid revealing whether the username or password was incorrect.
- Implement account lockout or rate limiting.
- Add monitoring for repeated failed login attempts.
- Use multi-factor authentication.
- Apply strong password policies.
- Detect and block automated login attempts.

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Burp Proxy
- Burp Intruder
- Web browser
