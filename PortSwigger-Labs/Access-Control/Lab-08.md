# Lab 08: User ID Controlled by Request Parameter, with Unpredictable User IDs

## Platform

PortSwigger Web Security Academy

## Difficulty

Apprentice

## Topic

Access Control / IDOR / Horizontal Privilege Escalation

## Lab Status

Solved

## Objective

The goal of this lab was to obtain Carlos's API key by exploiting a horizontal privilege escalation vulnerability that used GUID-based user identifiers.

## Simple Explanation

The application used GUIDs instead of predictable usernames or numeric IDs to identify users.

Although the identifiers were difficult to guess, they were exposed elsewhere in the application.

After obtaining Carlos's GUID from a public blog post, it was possible to access his account information and retrieve his API key.

## Vulnerability Description

The application relied on user-supplied identifiers to determine which account data should be displayed.

While GUIDs reduced identifier predictability, the application failed to verify whether the authenticated user was authorized to access the requested account.

This resulted in an Insecure Direct Object Reference (IDOR) vulnerability.

## Key Concept

Using unpredictable identifiers does not replace proper access control.

Applications must validate that users are authorized to access requested resources regardless of how difficult identifiers are to guess.

## Steps Taken

1. Identified a blog post written by Carlos.
2. Opened Carlos's author profile.
3. Observed that the profile URL exposed Carlos's GUID.
4. Recorded the GUID value.
5. Logged in using:

   * Username: wiener
   * Password: peter
6. Accessed the account page.
7. Modified the user identifier parameter to Carlos's GUID.
8. Retrieved Carlos's account information.
9. Obtained Carlos's API key.
10. Submitted the API key to solve the lab.

## Result

Sensitive information belonging to another user was accessed by modifying a user-controlled identifier parameter.

## What I Learned

* GUIDs are not access control mechanisms.
* Hidden or unpredictable identifiers can still be discovered.
* Applications must enforce authorization checks on every request.
* IDOR vulnerabilities remain exploitable even when identifiers are difficult to guess.
* Publicly exposed information can assist attackers in discovering sensitive identifiers.

## Security Impact

In a real-world application, attackers could access sensitive user information, API keys, personal data, or account details belonging to other users.

## Mitigation

To prevent this vulnerability:

* Enforce server-side authorization checks.
* Verify ownership of requested resources.
* Avoid relying on identifier secrecy.
* Implement object-level access control validation.
* Follow the principle of least privilege.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Web Browser
