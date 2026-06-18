# Lab 05: User ID Controlled by Request Parameter

## Platform

PortSwigger Web Security Academy

## Difficulty

Apprentice

## Topic

Access Control / Horizontal Privilege Escalation / IDOR

## Lab Status

Solved

## Objective

The goal of this lab was to exploit a horizontal privilege escalation vulnerability and obtain the API key belonging to the user Carlos.

## Simple Explanation

The application used a user-controlled parameter in the URL to determine which user's account information should be displayed.

By modifying the user identifier from my own username to another user's username, I was able to access information that should only have been available to Carlos.

## Vulnerability Description

The application trusted the value supplied in the URL parameter without verifying whether the authenticated user was authorized to access the requested account.

As a result, any authenticated user could modify the identifier and access another user's sensitive information.

This is a classic example of an Insecure Direct Object Reference (IDOR) vulnerability and horizontal privilege escalation.

## Key Concept

Users should only be able to access their own resources.

Applications must verify that the authenticated user is authorized to access the requested object, regardless of any identifiers supplied by the client.

## Steps Taken

1. Logged in using the provided credentials:

   * Username: wiener
   * Password: peter

2. Accessed the account page.

3. Observed that the URL contained a user identifier parameter.

4. Sent the request to Burp Repeater.

5. Modified the identifier parameter from:

   id=wiener

   to:

   id=carlos

6. Sent the modified request.

7. Retrieved Carlos's account information, including the API key.

8. Submitted the API key to solve the lab.

9. The lab was successfully solved.

## Result

Sensitive information belonging to another user was accessed by modifying a user-controlled identifier parameter.

## What I Learned

* User identifiers should never be trusted for authorization decisions.
* Applications must verify ownership of requested resources.
* Horizontal privilege escalation allows access to data belonging to users with the same privilege level.
* IDOR vulnerabilities often occur when applications expose predictable identifiers.
* Sensitive information such as API keys should only be accessible to authorized users.

## Security Impact

In a real-world application, this vulnerability could allow attackers to access personal information, API keys, account details, documents, or other sensitive data belonging to other users.

## Mitigation

To prevent this vulnerability, developers should:

* Perform server-side authorization checks on every request.
* Verify resource ownership before returning sensitive data.
* Avoid relying solely on user-supplied identifiers.
* Implement access control validation for all direct object references.
* Apply the principle of least privilege.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Web Browser
