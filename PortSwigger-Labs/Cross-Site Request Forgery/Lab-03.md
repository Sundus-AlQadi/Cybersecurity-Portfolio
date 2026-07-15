## Lab 03: CSRF Where Token Validation Depends on Token Being Present

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / Missing Token Validation

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability by submitting the email change request without a CSRF token.

### Simple Explanation

The application checked the CSRF token only when the token parameter was included in the request.

If the token was changed to an invalid value, the request was rejected.

However, if the `csrf` parameter was removed completely, the request was accepted.

This allowed a CSRF attack using a form that only submits the new email address.

### Vulnerability Description

The vulnerable endpoint was:

```text
/my-account/change-email
```

The application expected an email change request to include:

```text
email
csrf
```

However, the CSRF validation logic failed when the token parameter was missing.

Instead of rejecting requests without a token, the application accepted them.

### Exploit Used

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-missing@test.com">
</form>

<script>
    document.forms[0].submit();
</script>
```

### Exploit Explanation

The form sends a POST request to the email change endpoint.

It includes only the `email` parameter and intentionally does not include the `csrf` parameter.

The script automatically submits the form when the victim opens the exploit page.

### Steps Taken

Logged in using the provided credentials:

```text
Username: wiener
Password: peter
```

Changed the email normally and captured the request.

Sent the request to Burp Repeater.

Changed the CSRF token value and confirmed that the request was rejected.

Deleted the `csrf` parameter completely and confirmed that the request was accepted.

Created an auto-submitting POST form in the Exploit Server without the CSRF parameter.

Clicked **Store**.

Clicked **View exploit** to test it on my own account.

Changed the email value to a new address.

Clicked **Deliver to victim**.

The lab was solved.

### Result

The victim's email was changed using a POST request that did not include a CSRF token.

### What I Learned

I learned that CSRF validation must reject requests when the token is missing, not only when it is invalid.

### Security Impact

An attacker could perform sensitive actions if the application accepts requests without CSRF tokens.

### Mitigation

Require a valid CSRF token for every state-changing request and reject requests where the token is missing.

### Tools Used

PortSwigger Web Security Academy

Exploit Server

Burp Suite Community Edition

Burp Repeater

Web Browser
