## Lab 21: Reflected XSS into a Template Literal with Multiple Characters Escaped

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Reflected XSS / JavaScript Template Literal Context

### Lab Status

Solved

### Objective

Exploit reflected XSS where the search input is reflected inside a JavaScript template literal and execute `alert(1)`.

### Simple Explanation

The search input was reflected inside a JavaScript template literal.

A template literal uses backticks:

```javascript id="ytt1gi"
`text here`
```

Inside a template literal, JavaScript expressions can be executed using:

```javascript id="jmc3y9"
${}
```

Even though several characters were encoded or escaped, the application still allowed expression injection using `${alert(1)}`.

### Vulnerability Description

The application reflected user input inside a JavaScript template string.

Characters such as angle brackets, quotes, backslashes, and backticks were handled, but the application did not prevent JavaScript expression evaluation inside the template literal.

This allowed the payload to execute directly inside the existing template string.

### Key Concept

```text id="k6irn1"
Context: JavaScript template literal
Technique: Expression injection
Payload: ${alert(1)}
```

In a template literal, `${...}` is evaluated as JavaScript.

### Payload Used

```javascript id="t3mvc5"
${alert(1)}
```

### Payload Explanation

The payload uses template literal expression syntax:

```javascript id="896azo"
${alert(1)}
```

The `${}` part tells JavaScript to evaluate the expression inside the template literal.

The expression `alert(1)` is then executed by the browser.

### Steps Taken

Submitted a random search value to identify the reflection point.

Confirmed that the input was reflected inside a JavaScript template literal.

Replaced the search value with:

```javascript id="skxse4"
${alert(1)}
```

The browser executed the expression and displayed an alert.

### Result

The payload executed successfully and triggered `alert(1)`.

### What I Learned

I learned that template literals can execute JavaScript expressions through `${}` even when many common XSS characters are escaped.

### Security Impact

A malicious URL could execute JavaScript in a victim's browser if user input is reflected inside a template literal.

### Mitigation

Avoid placing user input inside JavaScript template literals, or safely encode it for the JavaScript context before rendering.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Repeater

Web Browser

Browser Developer Tools
