# Lab 01: Unprotected Admin Functionality

## Platform
PortSwigger Web Security Academy

## Difficulty
Apprentice

## Topic
Access Control / Unprotected Admin Functionality

## Lab Status
Solved

## Objective
The goal of this lab was to identify an unprotected admin panel and use it to delete the user Carlos in a controlled lab environment.

## Simple Explanation
The application had an admin panel that was accessible without proper authorization checks.

Normally, admin functionality should only be available to authenticated admin users. In this lab, the admin panel was exposed and could be accessed directly.

## Vulnerability Description
The application failed to protect administrative functionality with proper access control.

Because the admin panel was not restricted, an unauthorized user could access sensitive administrative actions, including deleting users.

## Key Concept
Access control determines what actions a user is allowed to perform.

Even if a page is hidden from the normal interface, it must still be protected on the server side. Security should not rely on obscurity or hidden URLs.

## Steps Taken
1. Opened the lab from PortSwigger Web Security Academy.
2. Explored the application for exposed administrative functionality.
3. Identified an unprotected admin panel.
4. Accessed the admin panel directly.
5. Used the admin functionality to delete the user Carlos.
6. The lab was successfully solved.

## Result
The user Carlos was deleted through an unprotected admin panel.

## What I Learned
- Admin functionality must be protected with proper authorization checks.
- Hidden admin pages are not secure if they can be accessed directly.
- Access control must be enforced on the server side.
- Exposed admin panels can lead to serious privilege abuse.

## Security Impact
In a real-world application, this vulnerability could allow unauthorized users to perform administrative actions, delete accounts, view sensitive data, or modify application settings.

## Mitigation
To prevent this vulnerability, developers should:

- Enforce server-side authorization checks.
- Restrict admin functionality to authorized admin users only.
- Avoid relying on hidden URLs for security.
- Apply role-based access control.
- Log and monitor access to administrative endpoints.

## Tools Used
- PortSwigger Web Security Academy
- Web browser
- Burp Suite Community Edition
