# Lab 03: User Role Controlled by Request Parameter

## Platform
PortSwigger Web Security Academy

## Difficulty
Apprentice

## Topic
Access Control / Role Manipulation / Privilege Escalation

## Lab Status
Solved

## Objective
The goal of this lab was to gain administrative access by manipulating a user role parameter and use the admin functionality to delete the user Carlos.

## Simple Explanation
The application determined whether a user was an administrator based on a value that was sent to the client.

By modifying this value from a normal user role to an administrator role, it was possible to access administrative functionality without proper authorization.

## Vulnerability Description
The application trusted a client-controlled value to determine administrative privileges.

Because the role information could be modified by the user, an attacker could elevate their privileges and gain unauthorized access to administrative functions.

## Key Concept
User roles should never be controlled by client-side data.

Authorization decisions must be enforced on the server side using trusted data sources such as server-side sessions or database records.

## Steps Taken
1. Logged in using the provided credentials:
   - Username: wiener
   - Password: peter

2. Explored the application and identified the admin functionality.

3. Intercepted and analyzed requests using Burp Suite.

4. Modified the role-related value from a normal user role to an administrator role.

5. Accessed the administrative functionality.

6. Sent a request to:

   /admin/delete?username=carlos

7. Deleted the user Carlos.

8. The lab was successfully solved.

## Result
Administrative functionality was accessed by manipulating role-related data, allowing the deletion of the user Carlos.

## What I Learned
- User roles must never be trusted when controlled by the client.
- Authorization checks must always be performed on the server side.
- Privilege escalation can occur when role information is exposed and modifiable.
- Administrative actions should be protected independently from client-supplied values.

## Security Impact
In a real-world application, this vulnerability could allow attackers to gain administrative privileges, access sensitive functionality, modify data, or delete user accounts.

## Mitigation
To prevent this vulnerability, developers should:

- Store role information on the server side.
- Enforce authorization checks on every sensitive request.
- Never trust role values supplied by the client.
- Use secure session management.
- Apply role-based access control (RBAC).

## Tools Used
- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Burp Proxy
- Burp Repeater
