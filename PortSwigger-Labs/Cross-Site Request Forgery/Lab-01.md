## Lab 01: CSRF Vulnerability with No Defenses

### Platform

PortSwigger Web Security Academy

### Difficulty

Apprentice

### Topic

Cross-Site Request Forgery / Email Change Functionality

### Lab Status

Solved

### Objective

Exploit a CSRF vulnerability to change the victim user's email address.

### Simple Explanation

The email change function did not use a CSRF token.

This means an attacker can create a malicious page that submits the email change request automatically.

If the victim opens the exploit while logged in, the browser sends the request with the victim's session cookie, and the email gets changed.

### Vulnerability Description

The vulnerable endpoint was:

```text
/my-account/change-email
```

It accepted a POST request with only the `email` parameter.

Because there was no CSRF protection, the request could be triggered from another website.

### Exploit Used

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf-new@test.com">
</form>

<script>
    document.forms[0].submit();
</script>
```

### Exploit Explanation

The form sends a POST request to the email change endpoint.

The hidden input contains the new email address.

The JavaScript automatically submits the form without requiring the victim to click anything.

### Steps Taken

Logged in using the provided credentials:

```text
Username: wiener
Password: peter
```

Copied the correct lab domain.

Created an auto-submitting form in the Exploit Server body.

Clicked **Store**.

Clicked **View exploit** to test it on my own account.

Confirmed that my email changed successfully.

Changed the email value in the exploit to a new victim email.

Clicked **Deliver to victim**.

The lab was solved.

### Result

The victim's email was changed through a forged request.

This confirmed that the email change functionality was vulnerable to CSRF.

### What I Learned

I learned that CSRF can force a logged-in user's browser to perform an unwanted action.

### Security Impact

An attacker could change sensitive account information if no CSRF protection is used.

### Mitigation

Use CSRF tokens and SameSite cookie protection for state-changing requests.

### Tools Used

PortSwigger Web Security Academy

Exploit Server

Web Browser

Burp Suite Community Edition
