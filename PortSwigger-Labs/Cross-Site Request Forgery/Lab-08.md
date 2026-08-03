## Lab 08: SameSite Strict Bypass via Client-Side Redirect

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / SameSite Strict Bypass / Client-Side Redirect

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability by bypassing `SameSite=Strict` using a client-side redirect gadget.

### Simple Explanation

The session cookie was protected with `SameSite=Strict`, which normally prevents the cookie from being sent in cross-site requests.

However, the site had a client-side redirect page:

```text
/post/comment/confirmation?postId=
```

The `postId` parameter was used by JavaScript to build a redirect path.

By controlling `postId`, I could make the browser redirect from inside the target site to the email change endpoint.

This caused the request to be treated as same-site, so the session cookie was included.

### Vulnerability Description

The email change function did not use a CSRF token.

The site relied on `SameSite=Strict` cookies to reduce CSRF risk.

However, a client-side redirect gadget allowed an attacker to trigger a same-site navigation to a sensitive endpoint.

The vulnerable redirect path was controlled through:

```text
postId
```

### Exploit Used

```html
<script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=victim-strict-bypass%40test.com%26submit=1";
</script>
```

### Exploit Explanation

The exploit first sends the victim to the comment confirmation page.

The `postId` value contains path traversal:

```text
1/../../my-account/change-email
```

This makes the client-side redirect point to the email change endpoint.

The email parameter is included in the redirect target:

```text
email=victim-strict-bypass@test.com
```

The ampersand was encoded as:

```text
%26
```

so it stayed inside the `postId` parameter.

### Steps Taken

Logged in using:

```text
Username: wiener
Password: peter
```

Changed the email normally and confirmed that the request did not contain a CSRF token.

Tested that the email change endpoint accepted a GET request.

Checked the comment confirmation page and found that it redirected using the `postId` parameter.

Tested the redirect gadget using:

```text
/post/comment/confirmation?postId=../my-account
```

Confirmed that it redirected to the account page.

Created the exploit using the confirmation page and path traversal to reach the email change endpoint.

Stored the exploit.

Viewed the exploit and confirmed that my own email changed successfully.

Changed the email value to a new victim email.

Delivered the exploit to the victim.

The lab was solved.

### Result

The victim's email was changed by using a client-side redirect to bypass `SameSite=Strict`.

### What I Learned

I learned that `SameSite=Strict` can still be bypassed if the site has a same-site redirect gadget.

### Security Impact

An attacker could perform sensitive actions if a site relies only on SameSite cookies and has unsafe redirects.

### Mitigation

Use CSRF tokens for sensitive actions and avoid client-side redirects based on untrusted input.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Repeater

Exploit Server

Web Browser
