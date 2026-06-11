# Lab 06: 2FA Simple Bypass

## Platform
PortSwigger Web Security Academy

## Difficulty
Apprentice

## Topic
Authentication Vulnerabilities / Two-Factor Authentication / Access Control

## Lab Status
Solved

## Objective
The goal of this lab was to bypass a weak two-factor authentication flow and access the victim user's account page in a controlled lab environment.

## Simple Explanation
The application required a username and password followed by a two-factor authentication code.

However, after entering valid username and password credentials, the application did not properly enforce completion of the 2FA step before allowing access to the account page.

By manually navigating to the account page URL, it was possible to bypass the 2FA verification step.

## Vulnerability Description
The application had a flaw in its authentication flow.

Although the user was redirected to a 2FA verification page, the server did not properly check whether the 2FA step had been completed before allowing access to protected pages.

This allowed direct access to the account page after only completing the username and password step.

## Key Concept
Two-factor authentication must be enforced on the server side.

It is not enough to show a 2FA page in the browser. The application must verify that the 2FA step was completed before granting access to protected resources.

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Logged in using the provided personal credentials.
3. Retrieved the 2FA verification code from the email client.
4. Completed the 2FA process for the personal account.
5. Observed that the account page URL was `/my-account`.
6. Logged out of the personal account.
7. Logged in using the victim user's valid username and password.
8. When prompted for the 2FA code, manually changed the URL to `/my-account`.
9. The account page loaded without completing the 2FA verification step.
10. The lab was successfully solved.

## Result
The victim user's account page was accessed by bypassing the 2FA verification step.

The application allowed access to `/my-account` without confirming that two-factor authentication had been completed.

## What I Learned
- 2FA must be enforced server-side.
- A login flow can be insecure if it only redirects users to a 2FA page without protecting restricted pages.
- Access to sensitive pages should require full authentication completion.
- Authentication state should include whether the 2FA step was successfully completed.
- Direct URL navigation can reveal authentication flow weaknesses.

## Security Impact
In a real-world application, this vulnerability could allow attackers with valid usernames and passwords to bypass two-factor authentication.

This would weaken the protection provided by 2FA and could lead to unauthorized account access.

## Mitigation
To prevent this vulnerability, developers should:

- Enforce 2FA verification on the server side.
- Track whether the 2FA step has been completed in the user session.
- Restrict access to protected pages until full authentication is completed.
- Avoid relying only on client-side redirects or page flow.
- Re-check authentication state before serving sensitive pages.
- Use secure session management.

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Web browser
- Email client provided by the lab
