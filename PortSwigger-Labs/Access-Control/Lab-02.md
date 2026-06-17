# Lab 02: Unprotected Admin Functionality with Unpredictable URL

## Platform

PortSwigger Web Security Academy

## Difficulty

Apprentice

## Topic

Access Control / Unprotected Admin Functionality / Security by Obscurity

## Lab Status

Solved

## Objective

The goal of this lab was to identify an unprotected admin panel located at an unpredictable URL and use it to delete the user Carlos in a controlled lab environment.

## Simple Explanation

The application had an admin panel located at a non-obvious URL.

Although the URL was difficult to guess, it was disclosed in the application's client-side JavaScript. After finding the admin panel URL in the page source, it was possible to access the admin functionality and delete a user.

## Vulnerability Description

The application relied on hiding the admin panel URL instead of enforcing proper authorization checks.

The admin panel was not linked directly in the user interface, but its location was exposed in the client-side source code. Since the server did not properly restrict access, anyone who discovered the URL could access the admin panel.

## Key Concept

Hiding a URL is not access control.

Administrative functionality must be protected by server-side authorization checks. Client-side code, hidden links, or unpredictable URLs should not be relied on as a security control.

## Steps Taken

1. Opened the lab from PortSwigger Web Security Academy.
2. Reviewed the application's page source.
3. Identified JavaScript that disclosed the admin panel URL.
4. Opened the disclosed admin panel URL in the browser.
5. Accessed the admin functionality.
6. Deleted the user Carlos.
7. The lab was successfully solved.

## Result

The user Carlos was deleted through an unprotected admin panel whose URL was disclosed in client-side JavaScript.

## What I Learned

* Unpredictable URLs do not provide real security.
* Client-side JavaScript can disclose sensitive application paths.
* Admin functionality must be protected by server-side access control checks.
* Security by obscurity is not a reliable access control mechanism.
* Sensitive functionality should never be accessible based only on knowing a URL.

## Security Impact

In a real-world application, this vulnerability could allow unauthorized users to discover and access administrative functionality.

This may lead to user deletion, privilege abuse, data exposure, or unauthorized system changes.

## Mitigation

To prevent this vulnerability, developers should:

* Enforce server-side authorization checks for all admin functionality.
* Restrict admin endpoints to authorized admin users only.
* Avoid relying on hidden or unpredictable URLs for security.
* Avoid exposing sensitive endpoint paths in client-side JavaScript.
* Monitor and log access to administrative routes.
* Apply role-based access control to sensitive functions.

## Tools Used

* PortSwigger Web Security Academy
* Web browser
* Browser page source / developer tools
* Burp Suite Community Edition
