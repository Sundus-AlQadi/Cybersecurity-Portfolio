# Lab 05: Username Enumeration via Account Lock

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
Authentication Vulnerabilities / Username Enumeration / Account Locking / Brute Force

## Lab Status
Solved

## Objective
The goal of this lab was to enumerate a valid username through account lock behavior, brute-force the user's password using a provided wordlist, and access the account page in a controlled lab environment.

## Simple Explanation
The application used account locking as a brute-force protection mechanism.

However, the account lock behavior created a username enumeration issue. When repeated failed login attempts were made against a valid username, the application returned a different response indicating that too many incorrect login attempts had been made.

Invalid usernames did not trigger the same account lock behavior.

This difference allowed the valid username to be identified. After that, the password was tested using the provided password list.

## Vulnerability Description
The application was vulnerable because its account lock mechanism behaved differently for valid and invalid usernames.

A valid username could be identified by repeatedly submitting failed login attempts and observing the account lock response.

This created a username enumeration vulnerability even though the application attempted to implement brute-force protection.

## Key Concept
Account lock mechanisms can introduce username enumeration if they behave differently depending on whether the username exists.

A secure authentication system should avoid revealing whether an account exists through:

- Error messages
- Account lock messages
- Response length
- Response timing
- Status codes

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Submitted an invalid username and password.
3. Captured the `POST /login` request in Burp Suite.
4. Sent the request to Burp Intruder.
5. Repeated candidate usernames multiple times to trigger account lock behavior.
6. Used Burp Intruder with payload positions on the username and password parameters.
7. Used a repeated invalid password to test account lock behavior.
8. Added Grep-Match rules to identify responses containing account lock messages.
9. Identified the valid username by finding the response that contained the account lock message.
10. Started a new Intruder attack using the identified username.
11. Tested the candidate password list against the valid username.
12. Identified the correct password by finding the response that differed from invalid or locked responses.
13. Waited for the account lock to reset.
14. Logged in using the identified credentials.
15. The lab was successfully solved.

## Result
The valid username was identified through account lock behavior.

The correct password was identified from the response that differed from the normal invalid-password and account-lock responses.

## What I Learned
- Account locking can accidentally create username enumeration.
- A security feature can become a vulnerability if implemented with flawed logic.
- Grep-Match in Burp Intruder can help identify specific response messages.
- Response differences such as account lock messages can reveal valid usernames.
- Brute-force protection should be designed carefully to avoid leaking account existence.

## Security Impact
In a real-world application, attackers could use account lock behavior to identify valid usernames.

Once valid usernames are known, attackers may perform password guessing, credential stuffing, or targeted phishing attacks.

Account locking can also be abused to intentionally lock legitimate users out of their accounts.

## Mitigation
To prevent this vulnerability, developers should:

- Use generic and consistent authentication responses.
- Avoid revealing whether an account exists through lock messages.
- Apply rate limiting carefully across users and IP addresses.
- Use account lockout policies that do not leak account validity.
- Monitor repeated failed login attempts.
- Use multi-factor authentication.
- Implement progressive delays instead of obvious account lock messages.
- Avoid allowing attackers to trigger account lockouts against other users easily.

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Burp Proxy
- Burp Intruder
- Grep-Match
- Resource Pool
- Web browser
