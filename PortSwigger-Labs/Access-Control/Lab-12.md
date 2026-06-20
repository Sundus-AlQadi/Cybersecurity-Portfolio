# Lab 12: Multi-Step Process with No Access Control on One Step

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

Access Control / Multi-Step Process / Privilege Escalation

## Lab Status

Solved

## Objective

The goal of this lab was to exploit a flawed multi-step administrative process and promote a normal user account to administrator privileges.

## Simple Explanation

The application used a multi-step workflow for promoting users to administrator status.

Although the initial step was protected, the final confirmation step lacked proper authorization checks.

By replaying the confirmation request using a normal user session, administrative privileges could be obtained.

## Vulnerability Description

The application enforced access control on the first step of the workflow but failed to enforce the same controls on a later step.

As a result, an attacker could bypass the protected step and directly access the unprotected confirmation request.

This allowed unauthorized users to perform administrative actions.

## Key Concept

Every step in a sensitive workflow must perform its own authorization checks.

Protecting only the first step is not sufficient because attackers can directly invoke later steps.

## Steps Taken

1. Logged in using administrator credentials.
2. Accessed the administrative panel.
3. Initiated the user promotion process.
4. Captured the final confirmation request.
5. Sent the request to Burp Repeater.
6. Logged in using a normal user account.
7. Replaced the administrator session cookie with the normal user's session cookie.
8. Modified the request to target my own account.
9. Replayed the confirmation request.
10. Successfully obtained administrator privileges.
11. The lab was successfully solved.

## Result

Administrator privileges were obtained through an unprotected confirmation step within a multi-step workflow.

## What I Learned

* Multi-step workflows require authorization checks at every step.
* Confirmation requests can become attack targets.
* Access control should not rely on workflow assumptions.
* Attackers can bypass protected steps and invoke later steps directly.
* Privilege escalation may occur when later workflow stages are not validated.

## Security Impact

In a real-world application, attackers could bypass approval processes, authorize transactions, approve requests, modify permissions, or gain administrative privileges.

## Mitigation

To prevent this vulnerability:

* Enforce authorization checks at every workflow step.
* Validate user permissions on all sensitive actions.
* Avoid relying on previous workflow stages for security.
* Treat confirmation requests as security-sensitive endpoints.
* Implement server-side access control consistently.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Web Browser
