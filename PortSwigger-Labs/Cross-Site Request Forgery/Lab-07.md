## Lab 07: SameSite Lax Bypass via Method Override

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / SameSite Lax Bypass / Method Override

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability by bypassing SameSite Lax restrictions using a top-level GET request with method override.

### Simple Explanation

The email change function did not use a CSRF token.

The session cookie used the browser's default `SameSite=Lax` behavior.

SameSite Lax blocks cookies on cross-site POST requests, but allows cookies on top-level GET navigation.

The application also supported method override using:

```text
_method=POST
```

This allowed a GET request to be treated as a POST request by the server.

### Vulnerability Description

The normal email change request used POST:

```text
POST /my-account/change-email
```

However, the endpoint accepted a GET request when `_method=POST` was added:

```text
/my-account/change-email?email=test@test.com&_method=POST
```

Because the request was sent as a top-level GET navigation, the browser included the victim's session cookie.

The server then treated the request as a POST because of the `_method=POST` parameter.

### Exploit Used

```html
<script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=victim-samesite%40test.com&_method=POST";
</script>
```

### Exploit Explanation

The `document.location` line forces the victim's browser to navigate to the target URL.

This creates a top-level GET request, so the SameSite Lax cookie is included.

The `_method=POST` parameter makes the server process the request as a POST action.

As a result, the victim's email address is changed.

### Steps Taken

Logged in using:

```text
Username: wiener
Password: peter
```

Changed the email normally and captured the request.

Confirmed that the email change request did not contain a CSRF token.

Sent the request to Burp Repeater.

Changed the request to GET and added:

```text
_method=POST
```

The server accepted the request and returned:

```text
HTTP/2 302 Found
Location: /my-account?id=wiener
```

Created the exploit in the Exploit Server using `document.location`.

Tested the exploit on my own account.

Changed the email value to a new victim email.

Delivered the exploit to the victim.

The lab was solved.

### Result

The victim's email was changed using a SameSite Lax bypass with method override.

### What I Learned

I learned that SameSite Lax can be bypassed when a state-changing action is allowed through top-level GET navigation.

### Security Impact

An attacker could perform sensitive actions if the server supports unsafe method override behavior.

### Mitigation

Do not allow state-changing actions through GET requests, and use proper CSRF tokens for sensitive actions.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Repeater

Exploit Server

Web Browser
