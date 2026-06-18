# Lab 06: URL-Based Access Control Can Be Circumvented

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

Access Control / URL-Based Access Control Bypass

## Lab Status

Solved

## Objective

The goal of this lab was to bypass URL-based access controls and gain access to administrative functionality in order to delete the user Carlos.

## Simple Explanation

The application blocked direct access to the administrative interface.

However, the back-end application trusted the X-Original-URL header and processed requests using the value supplied in this header.

By manipulating the header, administrative functionality became accessible despite the front-end restrictions.

## Vulnerability Description

The front-end system attempted to restrict access to administrative URLs.

However, the back-end application interpreted requests differently and trusted the X-Original-URL header.

This inconsistency allowed an attacker to bypass access controls and access protected functionality.

## Key Concept

Security controls should be enforced consistently across all application layers.

If the front-end and back-end interpret requests differently, access control bypasses may become possible.

## Steps Taken

1. Attempted to access the administrative panel directly.
2. Observed that access was blocked.
3. Sent the request to Burp Repeater.
4. Added the X-Original-URL header.
5. Confirmed that the back-end processed URLs supplied through the header.
6. Accessed the admin functionality.
7. Sent a request to delete Carlos.
8. Successfully solved the lab.

## Result

Administrative functionality was accessed by bypassing URL-based access controls through the X-Original-URL header.

## What I Learned

* Front-end filtering alone is not sufficient for access control.
* Different systems may interpret the same request differently.
* Custom headers can introduce unexpected attack surfaces.
* Access controls should be enforced by the application itself.
* Security assumptions between components can create vulnerabilities.

## Security Impact

In a real-world application, attackers could gain access to restricted administrative functionality, sensitive information, or privileged operations by exploiting differences between front-end and back-end request handling.

## Mitigation

To prevent this vulnerability:

* Enforce authorization checks in the back-end application.
* Avoid trusting client-supplied routing headers.
* Validate and sanitize custom headers.
* Ensure consistent URL handling across all application layers.
* Apply defense-in-depth access controls.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Web Browser
