## Lab 13: Stored DOM XSS

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Stored DOM XSS / Client-Side Filtering Bypass

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a stored DOM XSS vulnerability in the blog comment functionality and execute the `alert()` function.

### Simple Explanation

This lab contained a stored DOM XSS vulnerability in the comment functionality.

The application stored the user's comment and later displayed it on the blog post page.

The website attempted to prevent XSS by using JavaScript to encode angle brackets.

However, the filtering was implemented incorrectly because the JavaScript `replace()` function only replaced the first occurrence when used with a normal string argument.

By adding an extra pair of angle brackets at the beginning of the comment, the first angle brackets were encoded, while the later malicious HTML remained active.

### Vulnerability Description

The vulnerable functionality was the blog comment feature.

The application attempted to encode dangerous characters such as `<` and `>` before inserting the comment into the DOM.

However, the protection was incomplete.

The JavaScript `replace()` function was used in a way that only encoded the first occurrence of angle brackets.

This allowed an attacker to bypass the filter by placing harmless angle brackets first, followed by a real HTML payload.

### Key Concept

Stored DOM XSS happens when malicious input is stored by the application and later processed unsafely by client-side JavaScript.

In this lab:

```text
Stored input: Blog comment
Client-side issue: Incomplete angle bracket replacement
Dangerous result: HTML injection into the DOM
Payload: <><img src=1 onerror=alert(1)>
```

### Payload Used

The payload used was:

```html
<><img src=1 onerror=alert(1)>
```

### Payload Explanation

The payload was:

```html
<><img src=1 onerror=alert(1)>
```

It can be understood as follows:

```html
<>
```

This extra pair of angle brackets is placed at the beginning so the weak filter encodes only these first characters.

```html
<img src=1 onerror=alert(1)>
```

This is the real XSS payload.

The image has an invalid source, so the browser triggers the `onerror` event and executes:

```javascript
alert(1)
```

### Steps Taken

Opened a blog post in the lab.

Scrolled to the comment form.

Entered the following payload in the comment field:

```html
<><img src=1 onerror=alert(1)>
```

Filled in the required name, email, and website fields.

Submitted the comment.

The comment was stored by the application.

When the comment was displayed on the page, the browser executed the injected HTML payload.

The `alert(1)` function was triggered.

Successfully solved the lab.

### Result

The stored payload executed successfully when the comment was displayed.

This confirmed that the application was vulnerable to stored DOM XSS due to incomplete client-side filtering.

### What I Learned

Stored DOM XSS can occur when stored user input is later processed unsafely by JavaScript.

Client-side filtering is not reliable when implemented incorrectly.

The JavaScript `replace()` function only replaces the first occurrence when the first argument is a normal string.

Adding extra characters before the real payload can sometimes bypass weak filters.

The `<img>` tag with an invalid `src` can trigger JavaScript using the `onerror` event.

XSS prevention should not rely on simple string replacement.

### Security Impact

In a real-world application, an attacker could store a malicious comment that executes JavaScript whenever another user views the blog post.

This could lead to session abuse, page manipulation, phishing, unauthorized actions, or exposure of sensitive information depending on the application's protections.

### Mitigation

To prevent this vulnerability, developers should:

Avoid inserting user-controlled comments into the DOM using unsafe methods.

Use safe DOM APIs such as `textContent` when displaying user content as text.

Do not rely on simple JavaScript string replacement for XSS prevention.

Use proper sanitization libraries such as DOMPurify when HTML is required.

Encode all dangerous characters, not only the first occurrence.

Use `replaceAll()` or a global regular expression only when appropriate, but prefer safe rendering methods.

Validate and sanitize user input on the server side.

Apply context-aware output encoding.

Use Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Web Browser

Browser Developer Tools
