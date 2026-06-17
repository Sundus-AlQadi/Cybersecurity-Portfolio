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
