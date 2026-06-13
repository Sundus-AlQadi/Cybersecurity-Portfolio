# Lab 10: Password Reset Broken Logic

## Platform

PortSwigger Web Security Academy

## Difficulty

Apprentice

## Topic

Authentication Vulnerabilities / Password Reset Logic

## Lab Status

Solved

## Objective

The goal of this lab was to exploit a broken password reset flow, reset the victim user's password, and access the victim account page in a controlled lab environment.

## Purpose of the Lab

This lab demonstrates how insecure password reset logic can allow attackers to reset another user's password.

Password reset functionality must securely verify that the reset token belongs to the correct user before allowing a password change.

## Simple Explanation

The application sent a password reset link containing a temporary token.

However, when submitting the new password, the application also included the username as a hidden input field.

The issue was that the application did not properly validate the reset token when the password change request was submitted. This allowed the username value to be changed to another user, causing that user's password to be reset.

## Vulnerability Description

The password reset function trusted a client-controlled username parameter.

Although a reset token was used in the reset link, the application did not properly enforce the token during the final password change request.

Because of this logic flaw, it was possible to change the username parameter and reset another user's password.

## Key Concept

Password reset tokens must be securely tied to the correct user account on the server side.

Applications should not trust hidden input fields or client-controlled parameters to determine which user's password is being changed.

## Steps Taken

1. Opened the lab from PortSwigger Web Security Academy.
2. Used the forgot password function for the personal account.
3. Opened the password reset email using the lab email client.
4. Followed the reset link and submitted a new password.
5. Captured the password reset request in Burp Suite.
6. Sent the password reset request to Burp Repeater.
7. Observed that the request contained a username parameter.
8. Removed the reset token value from the request.
9. Changed the username parameter from the personal account to the victim username.
10. Set a new password value.
11. Sent the modified request.
12. Logged in as the victim user using the newly set password.
13. Accessed the victim user's account page.
14. The lab was successfully solved.

## Result

The victim user's password was reset successfully due to broken password reset logic.

The victim account was accessed using the new password set through the vulnerable reset request.

## What I Learned

* Password reset functionality must validate reset tokens properly.
* Hidden form fields should not be trusted for sensitive account actions.
* Reset tokens must be tied to the correct user on the server side.
* Broken password reset logic can lead to account takeover.
* Client-controlled parameters can create serious authentication vulnerabilities.

## Security Impact

In a real-world application, this vulnerability could allow attackers to reset another user's password and take over their account.

This could lead to unauthorized access, data exposure, and loss of account control.

## Mitigation

To prevent this vulnerability, developers should:

* Validate password reset tokens on the server side.
* Bind reset tokens securely to the correct user account.
* Do not trust client-controlled username fields during password reset.
* Expire reset tokens after use.
* Use short-lived, random, high-entropy reset tokens.
* Invalidate old reset tokens when a new one is issued.
* Notify users when their password is changed.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Email client provided by the lab
* Web browser
