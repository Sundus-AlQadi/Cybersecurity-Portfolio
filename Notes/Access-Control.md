# Access Control Notes

## Unprotected Admin Functionality

Unprotected admin functionality occurs when administrative pages or actions are accessible without proper authorization checks.

Even if an admin page is hidden from the normal user interface, it must still be protected on the server side.

### Lab 01: Unprotected Admin Functionality

In this lab, I identified an exposed admin panel that was not protected by proper access control.

The admin panel allowed user management actions without verifying whether the user had administrator privileges.

### Key Takeaway
Access control must be enforced on the server side. Hidden URLs or unlinked pages should never be treated as a security control.


## Unprotected Admin Functionality with Unpredictable URL

Some applications hide admin panels at unusual or unpredictable URLs.

However, hiding the URL does not provide real access control. If the URL is exposed in client-side JavaScript or discovered by another method, unauthorized users may be able to access the admin functionality.

### Lab 02: Unprotected Admin Functionality with Unpredictable URL

In this lab, the admin panel was located at an unpredictable URL.

The URL was disclosed in the page source through client-side JavaScript. After identifying the URL, I accessed the admin panel and deleted the target user.

### Key Takeaway

## User Role Controlled by Client-Side Data

Some applications store role-related information in client-controlled data such as cookies, parameters, or hidden fields.

If the application trusts these values without proper validation, an attacker may be able to modify them and gain elevated privileges.

### Lab 03: User Role Controlled by Request Parameter

In this lab, administrative access was determined using role-related information that could be modified by the client.

By manipulating the role value, I gained access to administrative functionality and deleted the target user.

### Key Takeaway

Authorization decisions must be enforced on the server side.

Client-controlled values should never determine whether a user is an administrator.

Security by obscurity is not access control. Admin functionality must be protected by server-side authorization checks, even if the URL is hidden or difficult to guess.

## User Role Modification Through Profile Updates

Some applications expose profile update functionality that accepts more parameters than intended.

If sensitive fields such as role identifiers are accepted from user-controlled requests, attackers may be able to elevate their privileges.

### Lab 04: User Role Can Be Modified in User Profile

In this lab, a profile update request exposed role-related information.

By modifying the JSON request and adding a role-related field, administrative privileges were obtained and the admin panel became accessible.

### Key Takeaway

Profile update functionality should only accept approved fields.

Sensitive attributes such as role IDs, permissions, and administrator flags must be protected from user modification.


## User ID Controlled by Request Parameter (IDOR)

Applications often use identifiers in URLs to retrieve user-specific resources.

If the application does not verify that the authenticated user owns the requested resource, attackers may be able to access data belonging to other users simply by modifying the identifier.

### Lab 05: User ID Controlled by Request Parameter

In this lab, the account page used a user identifier parameter to determine which account information should be displayed.

By changing the identifier from my own username to another user's username, I was able to access Carlos's account information and retrieve his API key.

### Key Takeaway

Authorization checks must validate resource ownership, not just authentication status.

Changing a user identifier should never allow access to another user's data.

## Reference: Front-End and Back-End Request Handling

Modern web applications may use more than one layer before the request reaches the actual application.

```text
User
 ↓
Nginx / Reverse Proxy / Load Balancer
 ↓
Application
```

### Nginx

Nginx is a web server and reverse proxy that can sit in front of web applications.

It can handle requests, forward traffic, block certain paths, serve static files, or apply basic filtering rules.

### Reverse Proxy

A reverse proxy receives requests from users and forwards them to internal applications.

The user communicates with the reverse proxy, not directly with the internal application.

### Load Balancer

A load balancer distributes requests across multiple servers.

This helps applications handle more traffic and improves availability.

### Important Security Concept

If the front-end system and back-end application interpret requests differently, access control bypasses may become possible.

For example, the front-end may block access to `/admin`, but the back-end may still process `/admin` if it receives that path through a trusted header such as `X-Original-URL`.

## Front-End and Back-End Access Control Mismatches

Many modern applications use multiple layers such as reverse proxies, load balancers, and back-end applications.

Sometimes these systems interpret requests differently.

If access control is enforced only at the front-end layer, attackers may be able to reach protected functionality through alternative request paths or headers.

### X-Original-URL Header

Some frameworks support special headers such as:

* X-Original-URL
* X-Rewrite-URL

These headers may influence how the back-end processes requests.

If improperly trusted, they can allow access control bypasses.

### Lab 06: URL-Based Access Control Can Be Circumvented

In this lab, the front-end blocked access to administrative URLs.

However, the back-end trusted the X-Original-URL header and processed requests based on its value.

By supplying administrative paths through this header, protected functionality became accessible.

### Key Takeaway

Access control should always be enforced by the application itself.

## Method-Based Access Control

Some applications apply authorization checks differently depending on the HTTP method being used.

For example, a POST request may be protected while an equivalent GET request performs the same action without proper authorization checks.

### Common HTTP Methods

* GET → Retrieve data
* POST → Submit or create data
* PUT → Update data
* DELETE → Remove data

### Lab 07: Method-Based Access Control Can Be Circumvented

In this lab, administrative functionality was protected when accessed using POST requests.

However, the same functionality became accessible after converting the request to a GET request.

This allowed privilege escalation and administrative access.

### Key Takeaway

Authorization should be based on user permissions, not on the HTTP method being used.

Changing the request method should never bypass security controls.


Front-end filtering should never be the only protection mechanism.

## IDOR with Unpredictable Identifiers

Developers sometimes replace sequential IDs with GUIDs to make resource identifiers harder to guess.

While this improves unpredictability, it does not solve authorization problems.

If attackers can obtain a valid GUID through public content, logs, APIs, or other exposed resources, the vulnerability remains exploitable.

### Lab 08: User ID Controlled by Request Parameter, with Unpredictable User IDs

In this lab, user accounts were identified using GUIDs instead of predictable usernames.

Carlos's GUID was disclosed through publicly accessible content.

After obtaining the GUID, I modified the account identifier and accessed Carlos's API key.

### Key Takeaway

GUIDs improve identifier unpredictability but do not replace proper access control.

Authorization checks must always verify whether a user is allowed to access a resource.

