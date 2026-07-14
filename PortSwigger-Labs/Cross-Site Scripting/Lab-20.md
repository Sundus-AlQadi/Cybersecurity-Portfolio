## Lab 20: Stored XSS into `onclick` Event with Angle Brackets and Double Quotes HTML-Encoded and Single Quotes and Backslash Escaped

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Stored XSS / JavaScript Event Handler Context / Encoding Bypass

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a stored XSS vulnerability in the comment functionality and execute `alert(1)` when the comment author name was clicked.

### Simple Explanation

The vulnerability was in the comment Website field.

The application stored the Website value and later placed it inside an `onclick` event handler for the comment author's name.

The application encoded angle brackets and double quotes, and escaped single quotes and backslashes.

Because direct quote injection was blocked, the payload used the HTML entity `&apos;`, which is interpreted by the browser as a single quote.

This allowed the payload to break out of the JavaScript string inside the `onclick` handler and execute `alert(1)`.

### Vulnerability Description

The Website input was stored by the application and later reflected inside an `onclick` event handler.

The application attempted to protect the context by encoding and escaping several characters.

However, the use of `&apos;` allowed a single quote to be introduced into the JavaScript event handler context after browser parsing.

This made it possible to break out of the string and execute JavaScript when the author name was clicked.

### Key Concept

This lab involved stored XSS inside a JavaScript event handler attribute.

In this lab:

```text
Stored input: Website field
Execution context: onclick event handler
Bypass technique: HTML entity &apos;
Executed function: alert(1)
```

### Payload Used

The payload used in the Website field was:

```text
http://foo?&apos;-alert(1)-&apos;
```

After browser interpretation, `&apos;` becomes:

```text
'
```

So the JavaScript-breaking part becomes similar to:

```javascript
'-alert(1)-'
```

### Payload Explanation

The payload was:

```text
http://foo?&apos;-alert(1)-&apos;
```

It can be understood as follows:

```text
http://foo?
```

Provides a URL-like value for the Website field.

```html
&apos;
```

Represents a single quote character.

```javascript
-alert(1)-
```

Uses JavaScript syntax to call `alert(1)` after breaking out of the string.

The final `&apos;` helps balance the remaining JavaScript context.

### Steps Taken

Opened a blog post in the lab.

Submitted a test comment and placed a random value in the Website field.

Viewed the blog post and confirmed that the Website value was used inside an `onclick` event handler.

Submitted another comment using the following payload in the Website field:

```text
http://foo?&apos;-alert(1)-&apos;
```

Filled in the required comment, name, and email fields.

Posted the comment.

Clicked the comment author's name.

The browser executed the payload and displayed an alert.

Successfully solved the lab.

### Result

The stored payload executed when the comment author name was clicked.

This confirmed that the application was vulnerable to stored XSS inside an `onclick` event handler.

### What I Learned

I learned how HTML entities such as `&apos;` can bypass escaping in JavaScript event handler contexts.

### Security Impact

An attacker could store a malicious comment that executes JavaScript when another user clicks the comment author name.

### Mitigation

Avoid placing user-controlled input inside JavaScript event handlers, and apply proper context-aware encoding for event handler contexts.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Repeater

Web Browser

Browser Developer Tools
