# Access Control Notes

## Unprotected Admin Functionality

Unprotected admin functionality occurs when administrative pages or actions are accessible without proper authorization checks.

Even if an admin page is hidden from the normal user interface, it must still be protected on the server side.

### Lab 01: Unprotected Admin Functionality

In this lab, I identified an exposed admin panel that was not protected by proper access control.

The admin panel allowed user management actions without verifying whether the user had administrator privileges.

### Key Takeaway
Access control must be enforced on the server side. Hidden URLs or unlinked pages should never be treated as a security control.
