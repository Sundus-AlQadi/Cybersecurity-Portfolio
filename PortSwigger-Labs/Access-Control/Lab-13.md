# Lab 13: Referer-Based Access Control

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

Access Control / Referer Header Manipulation / Privilege Escalation

## Lab Status

Solved

## Objective

The goal of this lab was to exploit flawed access controls that relied on the HTTP Referer header and promote a normal user account to administrator privileges.

## Simple Explanation

The application attempted to restrict administrative actions by checking whether requests originated from an administrative page.

Instead of verifying user permissions, the application trusted the Referer header supplied by the browser.

By replaying an administrative request with a non-administrator session while preserving the Referer header, administrative privileges were obtained.

## Vulnerability Description

The application relied on the Referer header as part of its authorization logic.

Since HTTP headers can be modified by attackers, this allowed unauthorized users to perform privileged actions simply by sending requests containing an acceptable Referer value.

## Key Concept

Authorization decisions must be based on user permissions, not on client-controlled HTTP headers.

Headers such as Referer can be modified and should never be trusted for access control.

## Steps Taken

1. Logged in using administrator credentials.
2. Accessed the administrative panel.
3. Promoted a user and captured the request.
4. Sent the request to Burp Repeater.
5. Logged in as a normal user.
6. Replaced the administrator session cookie with the normal user's session.
7. Modified the username parameter to target my own account.
8. Replayed the request while keeping the Referer header intact.
9. Successfully obtained administrator privileges.
10. The lab was successfully solved.

## Result

Administrative privileges were obtained by exploiting access controls that trusted the Referer header.

## What I Learned

* HTTP headers are client-controlled.
* Referer headers should not be trusted for authorization.
* Access control decisions must be based on authenticated user permissions.
* Administrative functionality should always perform server-side authorization checks.
* Attackers can manipulate request metadata to bypass weak security controls.

## Security Impact

In a real-world application, attackers could gain unauthorized access to administrative functions, modify user permissions, perform privileged operations, or compromise application security.

## Mitigation

To prevent this vulnerability:

* Never use Referer headers for authorization decisions.
* Perform server-side permission validation.
* Base access control decisions on authenticated user roles.
* Treat all client-supplied headers as untrusted input.
* Apply defense-in-depth authorization controls.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Web Browser
