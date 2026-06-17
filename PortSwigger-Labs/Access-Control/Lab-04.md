# Lab 04: User Role Can Be Modified in User Profile

## Platform

PortSwigger Web Security Academy

## Difficulty

Apprentice

## Topic

Access Control / Privilege Escalation / Mass Assignment

## Lab Status

Solved

## Objective

The goal of this lab was to gain administrative privileges by modifying role-related data through a user profile update request and then use the admin panel to delete the user Carlos.

## Simple Explanation

The application allowed users to update profile information such as their email address.

During testing, it was discovered that additional parameters could be added to the profile update request. By adding a role-related parameter, it was possible to change the account's role from a normal user to an administrator.

## Vulnerability Description

The application accepted sensitive fields that should not have been modifiable by regular users.

By modifying the request and adding a role-related parameter, unauthorized privilege escalation became possible.

This is an example of improper access control and insecure handling of user-supplied data.

## Key Concept

Users should only be allowed to modify authorized profile fields.

Sensitive attributes such as roles, permissions, administrator status, and privilege levels should never be accepted from client-controlled requests.

## Steps Taken

1. Logged in using the provided credentials:

   * Username: wiener
   * Password: peter

2. Accessed the account page.

3. Updated the email address associated with the account.

4. Intercepted the profile update request using Burp Suite.

5. Sent the request to Burp Repeater.

6. Added an additional role-related field to the JSON request body.

7. Resent the modified request.

8. Confirmed that the account role changed to an administrator role.

9. Accessed the administrative panel.

10. Deleted the user Carlos.

11. The lab was successfully solved.

## Result

Administrative privileges were obtained through manipulation of profile update data, allowing access to the admin panel and deletion of the user Carlos.

## What I Learned

* Sensitive user attributes must not be modifiable by regular users.
* Access control must be enforced on the server side.
* Profile update functionality can introduce privilege escalation vulnerabilities.
* Hidden or undocumented parameters should be properly validated.
* Role management should never be controlled through client-supplied data.

## Security Impact

In a real-world application, this vulnerability could allow attackers to elevate their privileges, gain administrative access, modify permissions, or take complete control of sensitive application functionality.

## Mitigation

To prevent this vulnerability, developers should:

* Use allowlists for profile update fields.
* Prevent modification of sensitive attributes such as roles and permissions.
* Perform server-side authorization checks.
* Separate profile management from privilege management functionality.
* Validate all incoming request parameters.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
