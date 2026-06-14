# Lab 12: Password Brute-Force via Password Change

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

Authentication Vulnerabilities / Password Enumeration / Password Change Functionality

## Lab Status

Solved

## Objective

The goal of this lab was to identify the victim user's password by exploiting information disclosure within the password change functionality and then access the victim account.

## Purpose of the Lab

This lab demonstrates how authentication-related functionality can leak information through inconsistent error messages.

Different responses based on password validity can allow attackers to identify correct passwords without successfully changing them.

## Simple Explanation

The password change function required users to provide their current password.

The application returned different error messages depending on whether the current password was correct.

By intentionally submitting mismatched new passwords and testing candidate passwords in the current-password field, it was possible to determine which password belonged to the victim account.

## Vulnerability Description

The application revealed authentication information through different password change responses.

An incorrect current password generated one error message, while a correct current password combined with invalid new password input generated a different message.

This allowed password enumeration through response analysis.

## Key Concept

Authentication systems should avoid revealing whether a supplied password is correct through different messages, response lengths, or status codes.

All authentication failures should generate consistent responses.

## Steps Taken

1. Logged in using the provided account.
2. Tested the password change functionality.
3. Observed different responses based on password validity.
4. Captured the password change request in Burp Suite.
5. Sent the request to Burp Intruder.
6. Changed the username parameter to the victim user.
7. Added a payload position to the current-password parameter.
8. Kept the two new password values intentionally different.
9. Loaded the provided candidate password list.
10. Added a Grep-Match rule for the response:
    "New passwords do not match".
11. Started the attack.
12. Identified the password that triggered the matching response.
13. Logged in successfully as the victim user.
14. Accessed the victim account page.
15. Successfully solved the lab.

## Result

The victim user's password was identified through password enumeration within the password change functionality.

The victim account was accessed successfully.

## What I Learned

* Password change functionality can leak authentication information.
* Different error messages can create password enumeration vulnerabilities.
* Authentication failures should produce consistent responses.
* Response analysis is often sufficient to identify valid credentials.
* Security testing should include password reset and password change workflows.

## Security Impact

In a real-world application, attackers could identify valid passwords without triggering traditional login-based brute-force protections.

This could lead to unauthorized account access and account compromise.

## Mitigation

To prevent this vulnerability, developers should:

* Use consistent error messages.
* Avoid revealing whether a password is correct.
* Implement rate limiting on password change endpoints.
* Monitor repeated password change failures.
* Apply account protection mechanisms across all authentication-related functions.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Intruder
* Grep-Match
* Web Browser
