# Lab 10: User ID Controlled by Request Parameter with Password Disclosure

## Platform

PortSwigger Web Security Academy

## Difficulty

Apprentice

## Topic

Access Control / IDOR / Information Disclosure / Password Exposure

## Lab Status

Solved

## Objective

The goal of this lab was to retrieve the administrator's password by exploiting an access control vulnerability and then use the administrator account to delete the user Carlos.

## Simple Explanation

The application allowed users to view account information based on a user-controlled identifier parameter.

In addition, the account page contained the user's current password prefilled inside a password field.

By changing the account identifier from my own account to the administrator account, I was able to view the administrator's password and use it to log in.

## Vulnerability Description

The application suffered from two security issues:

1. Broken access control that allowed account information to be accessed by modifying a user identifier.
2. Sensitive password disclosure by exposing stored passwords within account pages.

Together, these vulnerabilities allowed full compromise of an administrator account.

## Key Concept

Passwords should never be retrievable by applications.

Even administrators should not be able to view existing passwords because passwords should be stored as secure hashes rather than reversible values.

## Steps Taken

1. Logged in using:

   * Username: wiener
   * Password: peter

2. Accessed the account page.

3. Observed that the URL contained a user identifier parameter.

4. Modified the identifier to target the administrator account.

5. Sent the request and reviewed the response in Burp Suite.

6. Observed that the administrator's password was included in the response.

7. Logged in using the administrator account.

8. Accessed the admin panel.

9. Deleted the user Carlos.

10. The lab was successfully solved.

## Result

The administrator password was disclosed through an insecure account page and used to gain administrative access.

## What I Learned

* Passwords should never be displayed back to users.
* Passwords should be stored using secure hashing algorithms.
* Access control failures can expose highly sensitive information.
* IDOR vulnerabilities can lead directly to account compromise.
* Multiple weaknesses can combine into complete administrative takeover.

## Security Impact

In a real-world application, attackers could obtain passwords belonging to other users or administrators, leading to account takeover, privilege escalation, and full system compromise.

## Mitigation

To prevent this vulnerability:

* Store passwords using secure one-way hashing.
* Never display existing passwords to users.
* Implement proper authorization checks.
* Restrict access to account information.
* Follow secure credential management practices.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Web Browser
