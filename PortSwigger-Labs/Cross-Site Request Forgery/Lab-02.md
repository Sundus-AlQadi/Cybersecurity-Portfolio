## Lab 02: CSRF Where Token Validation Depends on Request Method

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / Method-Based CSRF Bypass

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability by changing the request method from POST to GET.

### Simple Explanation

The email change function used a CSRF token for POST requests.

However, when the same action was sent as a GET request, the application did not validate the CSRF token.

This allowed an attacker to create a malicious page that submits a GET request to change the victim's email.

### Vulnerability Description

The application protected the email change function only for POST requests.

When the request method was changed to GET, the CSRF token was no longer required.

This made the email change function vulnerable to CSRF.

### Exploit Used

```html
<form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-method@test.com">
</form>

<script>
    document.forms[0].submit();
</script>
```

### Exploit Explanation

The form does not include a `method` attribute.

By default, HTML forms use GET.

This sends a request like:

```text
/my-account/change-email?email=csrf-method@test.com
```

Because the application does not validate CSRF tokens on GET requests, the email change is accepted.

### Steps Taken

Logged in using the provided credentials:

```text
Username: wiener
Password: peter
```

Changed the email normally and captured the request.

Confirmed that the POST request used a CSRF token.

Changed the request method to GET and observed that the token was no longer required.

Created an auto-submitting GET form in the Exploit Server.

Clicked **Store**.

Clicked **View exploit** to test it on my own account.

Changed the email value to a new address.

Clicked **Deliver to victim**.

The lab was solved.

### Result

The victim's email was changed using a GET request without a valid CSRF token.

### What I Learned

I learned that CSRF protection must be applied to all state-changing request methods.

### Security Impact

An attacker could change account data if sensitive actions are allowed through unprotected GET requests.

### Mitigation

Validate CSRF tokens for all state-changing requests and do not use GET for actions that modify data.

### Tools Used

PortSwigger Web Security Academy

Exploit Server

Burp Suite Community Edition

Burp Repeater

Web Browser
