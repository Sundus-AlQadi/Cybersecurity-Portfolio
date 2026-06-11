# Lab 07: 2FA Broken Logic

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

Authentication Vulnerabilities / Two-Factor Authentication / Broken Logic

## Lab Status

Solved

## Objective

The goal of this lab was to exploit flawed two-factor authentication logic and access the victim user's account page in a controlled lab environment.

## Simple Explanation

The application used two-factor authentication after username and password login.

However, the application relied on a user-controlled value to decide which user's 2FA process was being verified. By changing the `verify` value from the logged-in user to the victim username, it was possible to generate and test a 2FA code for the victim account.

## Vulnerability Description

The application was vulnerable because the 2FA verification process trusted a client-controlled value.

The `verify` value was used to determine which user's 2FA code should be generated or checked. Because this value could be modified, the 2FA flow could be manipulated to target another user.

This allowed brute-forcing the victim user's 2FA code and accessing the victim account.

## Key Concept

2FA verification must be securely linked to the authenticated session on the server side.

Applications should not trust client-side parameters or cookies to decide which user's 2FA code is being verified.

## Steps Taken

1. Opened the lab from PortSwigger Web Security Academy.
2. Logged in using the provided personal credentials.
3. Observed the 2FA verification process in Burp Suite.
4. Identified that the application used a `verify` value during the 2FA process.
5. Sent the 2FA request to Burp Repeater.
6. Changed the `verify` value to the victim username.
7. Sent the modified request to generate a temporary 2FA code for the victim user.
8. Logged in again using the personal account credentials.
9. Submitted an invalid 2FA code and captured the `POST /login2` request.
10. Modified the request so that the `verify` value targeted the victim user.
11. Used Turbo Intruder to brute-force the 4-digit 2FA code.
12. Identified the correct 2FA code through a response difference.
13. Submitted the valid 2FA code with the modified `verify` value.
14. Accessed the victim user's account page.
15. The lab was successfully solved.

## Result

The victim user's account page was accessed by exploiting flawed 2FA logic.

The application allowed the 2FA verification target to be controlled by the client, which made it possible to generate and brute-force a 2FA code for another user.

## What I Learned

* 2FA logic must be enforced securely on the server side.
* Client-controlled values should not determine which user's 2FA code is verified.
* Cookies and parameters can reveal authentication flow weaknesses.
* Turbo Intruder can be useful for high-volume testing in controlled lab environments.
* A valid username and flawed 2FA logic can lead to unauthorized account access.

## Security Impact

In a real-world application, this vulnerability could allow attackers to bypass or abuse 2FA protections.

If an attacker has a valid username and can manipulate the 2FA verification target, they may be able to brute-force another user's 2FA code and access that account.

This weakens the purpose of two-factor authentication and can lead to account takeover.

## Mitigation

To prevent this vulnerability, developers should:

* Bind 2FA verification to the authenticated session on the server side.
* Avoid trusting client-controlled parameters or cookies for user identity.
* Ensure that 2FA codes are generated and validated only for the correct logged-in user.
* Apply rate limiting to 2FA code attempts.
* Expire 2FA codes quickly.
* Monitor repeated failed 2FA attempts.
* Use secure session management throughout the authentication flow.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Turbo Intruder
* Web browser
