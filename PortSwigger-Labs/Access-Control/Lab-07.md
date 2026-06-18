# Lab 07: Method-Based Access Control Can Be Circumvented

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

Access Control / HTTP Method Manipulation / Privilege Escalation

## Lab Status

Solved

## Objective

The goal of this lab was to exploit flawed method-based access controls and promote a normal user account to administrator privileges.

## Simple Explanation

The application restricted administrative actions based on the HTTP request method.

Administrative functionality was protected when requests used the POST method. However, the same functionality remained accessible when the request method was changed.

By switching the request from POST to GET, it was possible to bypass the access control mechanism and perform an administrative action as a non-administrator.

## Vulnerability Description

The application relied on the HTTP method as part of its authorization logic.

Access control checks were applied differently depending on whether the request used POST or GET.

As a result, changing the request method allowed an attacker to bypass authorization controls and perform privileged actions.

## Key Concept

Authorization checks should be independent of the HTTP method.

Changing a request from POST to GET should never bypass access control restrictions.

## Steps Taken

1. Logged in using administrator credentials.
2. Accessed the admin panel.
3. Captured a request used to promote a user.
4. Sent the request to Burp Repeater.
5. Logged in as a normal user.
6. Replaced the administrator session with the normal user's session.
7. Confirmed that the request was blocked when sent as POST.
8. Converted the request from POST to GET.
9. Modified the target username to my own account.
10. Sent the modified request.
11. Successfully promoted the account to administrator privileges.
12. The lab was successfully solved.

## Result

Administrative privileges were obtained by bypassing access controls through HTTP method manipulation.

## What I Learned

* Access control must not depend on HTTP methods.
* Authorization logic should be applied consistently.
* GET and POST requests should be protected equally.
* Request method manipulation can lead to privilege escalation.
* Administrative actions require proper server-side authorization checks.

## Security Impact

In a real-world application, attackers could perform privileged actions without administrator rights simply by modifying the HTTP method used to access protected functionality.

This could lead to privilege escalation, unauthorized account modifications, or administrative compromise.

## Mitigation

To prevent this vulnerability:

* Apply authorization checks independently of HTTP methods.
* Enforce server-side access control validation.
* Ensure all routes performing sensitive actions require proper authorization.
* Test administrative functionality using different HTTP methods.
* Follow the principle of least privilege.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Web Browser
