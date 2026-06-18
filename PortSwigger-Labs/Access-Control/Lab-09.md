# Lab 09: User ID Controlled by Request Parameter with Data Leakage in Redirect

## Platform

PortSwigger Web Security Academy

## Difficulty

Apprentice

## Topic

Access Control / IDOR / Horizontal Privilege Escalation / Information Disclosure

## Lab Status

Solved

## Objective

The goal of this lab was to obtain Carlos's API key by exploiting an access control vulnerability where sensitive data was leaked in the body of a redirect response.

## Simple Explanation

The application used a user-controlled `id` parameter to determine which user's account information should be loaded.

When the `id` parameter was changed to another user, the application redirected the request back to another page. However, the redirect response still contained sensitive information in the response body.

This allowed the API key for Carlos to be retrieved even though the browser followed the redirect.

## Vulnerability Description

The application attempted to prevent unauthorized access by redirecting the user away from another user's account page.

However, before redirecting, the response body still included sensitive account data.

This created an information disclosure vulnerability combined with broken access control.

## Key Concept

Redirecting a user is not the same as enforcing access control.

Sensitive data must not be included in responses unless the user is authorized to access it.

## Steps Taken

1. Logged in using the provided credentials:

   * Username: wiener
   * Password: peter

2. Accessed the account page.

3. Sent the account page request to Burp Repeater.

4. Modified the `id` parameter to target another user.

5. Sent the modified request.

6. Observed that the response returned a redirect.

7. Inspected the response body in Burp Repeater.

8. Found sensitive information belonging to Carlos inside the redirect response body.

9. Retrieved Carlos's API key.

10. Submitted the API key to solve the lab.

## Result

Carlos's API key was retrieved from the body of a redirect response.

## What I Learned

* Redirects do not replace proper authorization checks.
* Sensitive data can still leak in redirect response bodies.
* Burp Repeater can reveal response bodies that browsers may hide due to redirects.
* Applications must check authorization before generating sensitive content.
* IDOR vulnerabilities can appear even when the UI redirects users away.

## Security Impact

In a real-world application, this vulnerability could expose sensitive user data such as API keys, profile information, personal records, or account details.

Attackers could use tools like Burp Suite to inspect redirect responses and extract sensitive information.

## Mitigation

To prevent this vulnerability:

* Enforce authorization checks before generating any sensitive response content.
* Do not include sensitive data in redirect response bodies.
* Return proper access denied responses when authorization fails.
* Avoid relying on browser behavior to protect sensitive data.
* Test access control using proxy tools that show full responses.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Web Browser
