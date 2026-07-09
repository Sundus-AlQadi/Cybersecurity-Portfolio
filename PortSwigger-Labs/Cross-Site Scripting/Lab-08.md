## Lab 08: Stored XSS into Anchor `href` Attribute with Double Quotes HTML-Encoded

### Platform

PortSwigger Web Security Academy

### Difficulty

Apprentice

### Topic

Cross-Site Scripting / Stored XSS / HTML Attribute Context / Anchor `href` Injection

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a stored XSS vulnerability in the comment functionality by injecting a JavaScript URL into the comment author's website link.

### Simple Explanation

In this lab, the vulnerability existed in the blog comment functionality.

The application allowed users to submit a website URL when posting a comment.

The submitted website value was later used inside the `href` attribute of the comment author's name.

Because the application allowed a `javascript:` URL inside the `href` attribute, clicking the comment author's name executed JavaScript in the browser.

### Vulnerability Description

The application stored user-controlled input from the Website field.

When the blog post was viewed, this value was inserted into an anchor tag's `href` attribute.

Although double quotes were HTML-encoded, the application did not validate the URL scheme.

This allowed a dangerous JavaScript URL to be stored:

```javascript
javascript:alert(1)
```

When a user clicked the comment author's name, the browser executed the JavaScript.

This created a stored XSS vulnerability.

### Key Concept

Stored XSS occurs when malicious input is saved by the application and later displayed to users.

In this lab, the payload was not placed in the comment body. It was placed in the Website field.

The dangerous location was the anchor tag's `href` attribute.

The payload used was:

```javascript
javascript:alert(1)
```

### Payload Explanation

The payload was:

```javascript
javascript:alert(1)
```

It can be understood as follows:

```text
javascript:
```

Tells the browser to execute JavaScript when the link is clicked.

```javascript
alert(1)
```

Executes the alert function as proof of JavaScript execution.

The payload worked because the application accepted a dangerous URL scheme inside the `href` attribute.

### Steps Taken

Opened a blog post in the lab.

Submitted a test comment with a random value in the Website field.

Viewed the blog post and inspected the comment author's name.

Confirmed that the Website value was reflected inside an anchor `href` attribute.

Submitted another comment and entered the following payload in the Website field:

```javascript
javascript:alert(1)
```

Entered the required name, email, and comment fields.

Posted the comment.

Returned to the blog post.

Clicked the comment author's name.

Observed that the browser executed JavaScript and displayed an alert.

Successfully solved the lab.

### Result

The stored payload executed when the comment author's name was clicked.

This confirmed that the application was vulnerable to stored XSS through an anchor `href` attribute.

### What I Learned

Stored XSS can occur in fields other than the visible comment text.

The Website field can be dangerous if it is inserted into an anchor `href` attribute without validation.

Encoding double quotes helps prevent attribute breakout, but it does not prevent dangerous URL schemes.

The `href` attribute can execute JavaScript if it contains a `javascript:` URL.

XSS prevention must include URL validation, not only HTML encoding.

Context matters when creating and preventing XSS payloads.

### Security Impact

In a real-world application, this vulnerability could allow attackers to store a malicious link in a comment.

When another user clicks the comment author's name, JavaScript could execute in their browser.

This could lead to phishing, unauthorized actions, page manipulation, or abuse of the victim's authenticated session.

### Mitigation

To prevent this vulnerability, developers should:

Validate URL inputs before storing or displaying them.

Allow only safe URL schemes such as `http` and `https`.

Block dangerous schemes such as `javascript:`, `data:`, and `vbscript:`.

Apply context-aware output encoding.

Avoid trusting user-supplied URLs.

Use safe link generation logic.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Web Browser

Browser Developer Tools
