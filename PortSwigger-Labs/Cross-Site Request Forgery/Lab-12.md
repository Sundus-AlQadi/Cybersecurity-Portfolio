## Lab 12: CSRF with Broken Referer Validation

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / Broken Referer Validation

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability by bypassing weak `Referer` header validation.

### Simple Explanation

The application tried to protect the email change function by checking the `Referer` header.

The `Referer` header tells the server where the request came from.

The weakness was that the application accepted the request if the expected lab domain appeared anywhere inside the `Referer` value.

This means the `Referer` did not need to actually start with the trusted domain.

### Vulnerability Description

The application rejected a clearly external `Referer`, such as:

```text id="bhmud5"
Referer: https://evil.com
```

However, it accepted a malicious `Referer` that only contained the target domain in the query string:

```text id="o6f40h"
Referer: https://evil.com?YOUR-LAB-ID.web-security-academy.net
```

This showed that the validation was based on a weak string match instead of proper origin validation.

### Exploit Used

Head section:

```text id="ci7qb5"
Referrer-Policy: unsafe-url
```

Body section:

```html id="iabgbj"
<script>
    history.pushState("", "", "/?YOUR-LAB-ID.web-security-academy.net");
</script>

<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="broken-referer@test.com">
</form>

<script>
    document.forms[0].submit();
</script>
```

### Exploit Explanation

The `history.pushState()` function changed the exploit page URL in the browser to include the target lab domain in the query string.

This caused the `Referer` header to contain the expected domain.

The `Referrer-Policy: unsafe-url` header was added so the browser would include the full URL, including the query string, in the `Referer` header.

The form then submitted a POST request to change the victim's email.

Because the target domain appeared somewhere in the `Referer`, the application accepted the request.

### Steps Taken

Logged in using:

```text id="ej9xp4"
Username: wiener
Password: peter
```

Changed the email normally and captured the request in Burp.

Sent the request to Burp Repeater.

Changed the `Referer` header to an external domain and confirmed that the request was rejected.

Changed the `Referer` header so it contained the lab domain inside a query string.

The request was accepted.

Created a CSRF exploit using a hidden email change form.

Used `history.pushState()` to add the lab domain to the exploit page URL.

Added `Referrer-Policy: unsafe-url` in the Exploit Server head section.

Stored the exploit.

Tested the exploit on my own account.

Changed the email value to a new victim email.

Delivered the exploit to the victim.

The lab was solved.

### Result

The victim's email was changed because the application accepted a broken `Referer` validation bypass.

### What I Learned

I learned that checking whether a trusted domain appears anywhere in the `Referer` header is not secure.

### Security Impact

An attacker could bypass weak Referer checks and perform CSRF attacks against sensitive functions.

### Mitigation

Use proper CSRF tokens and validate origins correctly instead of relying on weak string matching in the `Referer` header.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Repeater

Exploit Server

Web Browser
