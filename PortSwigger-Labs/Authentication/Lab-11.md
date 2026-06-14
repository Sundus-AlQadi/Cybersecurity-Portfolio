# Lab 11: Password Reset Poisoning via Middleware

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

Authentication Vulnerabilities / Password Reset Poisoning

## Lab Status

Solved

## Objective

The goal of this lab was to exploit a password reset poisoning vulnerability, obtain the victim user's password reset token, reset the victim's password, and access the victim account.

## Purpose of the Lab

This lab demonstrates how applications can become vulnerable when they trust user-controlled HTTP headers while generating password reset links.

Password reset links should always be generated using trusted server-side configuration. If an application uses attacker-controlled values to construct these links, an attacker may be able to steal password reset tokens and compromise user accounts.

## Simple Explanation

The application generated password reset emails using the value provided in the X-Forwarded-Host header.

By modifying this header, it was possible to force the application to send a password reset email containing a link that pointed to an attacker-controlled domain.

When the victim clicked the reset link, the password reset token was sent to the attacker-controlled server and could then be used to reset the victim's password.

## Vulnerability Description

The application trusted the X-Forwarded-Host header when generating password reset URLs.

Because this header was user-controlled, an attacker could manipulate the destination of password reset links sent to other users.

This allowed the attacker to capture valid password reset tokens and perform account takeover.

## Key Concept

Password reset links must be generated using trusted server-side values.

Applications should never trust user-supplied headers when constructing sensitive URLs.

Password reset tokens should be treated as highly sensitive credentials because possession of a valid token may allow complete account compromise.

## Steps Taken

1. Investigated the password reset functionality.
2. Requested a password reset for a test account.
3. Observed the password reset email structure.
4. Captured the POST /forgot-password request in Burp Suite.
5. Sent the request to Burp Repeater.
6. Added a custom X-Forwarded-Host header containing the exploit server domain.
7. Changed the username parameter to the victim account.
8. Sent the modified request.
9. Opened the exploit server access log.
10. Captured the victim's password reset token from the logged request.
11. Generated a legitimate password reset link for the test account.
12. Replaced the token in the legitimate reset link with the victim's stolen token.
13. Opened the modified password reset URL.
14. Set a new password for the victim account.
15. Logged in as the victim user.
16. Successfully solved the lab.

## Result

A valid password reset token belonging to the victim user was stolen through password reset poisoning.

The token was used to reset the victim's password and gain access to the victim account.

## What I Learned

* Password reset links are security-sensitive assets.
* Password reset tokens can be as valuable as passwords.
* Applications should never trust user-controlled HTTP headers when generating security-related URLs.
* X-Forwarded-Host can become dangerous when improperly trusted.
* Password reset poisoning can lead directly to account takeover.
* Secure password reset functionality must be generated using trusted server-side configuration.

## Security Impact

In a real-world application, this vulnerability could allow attackers to hijack user accounts without knowing their passwords.

An attacker could steal password reset tokens and perform unauthorized password changes, leading to full account compromise.

## Mitigation

To prevent this vulnerability, developers should:

* Generate password reset URLs using trusted server-side configuration.
* Ignore user-controlled Host-related headers when creating sensitive links.
* Validate forwarded headers through trusted reverse proxies only.
* Use short-lived reset tokens.
* Invalidate tokens after use.
* Monitor unusual password reset activity.
* Notify users whenever password reset requests occur.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Exploit Server
* Email Client
* Web Browser
