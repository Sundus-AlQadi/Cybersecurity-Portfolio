## Lab 04: CSRF Where Token Is Not Tied to User Session

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / CSRF Token Session Binding

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability where the CSRF token is valid but not tied to a specific user session.

### Simple Explanation

The application uses CSRF tokens, but the token is not linked to the user session.

This means a valid token from one account can be accepted when used in another user's session.

For the final exploit, I used a fresh CSRF token from my own account and placed it inside an auto-submitting form. When the victim opened the exploit, the request was sent with the victim's session cookie and the email was changed.

### Vulnerability Description

The email change function requires a CSRF token.

However, the server only checks if the token is valid. It does not check if the token belongs to the same logged-in user.

Because of this, a token from one account can be reused in another user's request.

### Exploit Used

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-session@test.com">
    <input type="hidden" name="csrf" value="FRESH-CSRF-TOKEN">
</form>

<script>
    document.forms[0].submit();
</script>
```

### Exploit Explanation

The form sends a POST request to:

```text
/my-account/change-email
```

It includes:

```text
email
csrf
```

The CSRF token was taken from my own account, but it worked in the victim's session because the token was not tied to a specific user session.

### Steps Taken

Logged in using the provided account:

```text
Username: wiener
Password: peter
```

Went to `/my-account`.

Turned Intercept on in Burp.

Submitted the Update Email form.

Copied the fresh CSRF token from the intercepted request.

Dropped the request so the token would not be used.

Placed the unused CSRF token inside the exploit form.

Stored the exploit in the Exploit Server.

Delivered the exploit to the victim without using View exploit, because the token may be single-use.

The lab was solved.

### Result

The victim's email was changed using a CSRF token taken from another account.

This confirmed that the CSRF token was not tied to the user session.

### What I Learned

I learned that CSRF tokens must be linked to the user session, not just checked as valid tokens.

### Security Impact

An attacker could use their own valid CSRF token to perform actions in another user's session.

### Mitigation

Bind CSRF tokens to the user's session and reject tokens that do not belong to the current logged-in user.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Exploit Server

Web Browser
