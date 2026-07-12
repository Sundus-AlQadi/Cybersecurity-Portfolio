## Lab 11: DOM XSS in AngularJS Expression with Angle Brackets and Double Quotes HTML-Encoded

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / DOM XSS / AngularJS Expression Injection

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a DOM-based XSS vulnerability inside an AngularJS expression and execute the `alert()` function.

### Simple Explanation

In this lab, normal HTML-based XSS payloads were limited because angle brackets and double quotes were HTML-encoded.

This means payloads such as `<script>alert(1)</script>` would not execute as HTML.

However, the page used AngularJS with the `ng-app` directive.

AngularJS can evaluate expressions inside double curly braces, such as `{{ }}`.

By injecting an AngularJS expression into the search functionality, it was possible to execute JavaScript without needing HTML tags.

### Vulnerability Description

The application reflected the search input into a page area controlled by AngularJS.

Because the page used the `ng-app` directive, AngularJS scanned the content and evaluated expressions inside double curly braces.

The application did not safely handle user-controlled input before it was processed by AngularJS.

This allowed an attacker to inject an AngularJS expression that executed JavaScript.

### Key Concept

AngularJS expressions can become dangerous when user-controlled input is placed inside an AngularJS-controlled part of the page.

In this lab, angle brackets and double quotes were encoded, but this did not prevent XSS because the attack did not require injecting HTML tags.

The payload used was:

```javascript
{{$on.constructor('alert(1)')()}}
```

### Payload Explanation

The payload was:

```javascript
{{$on.constructor('alert(1)')()}}
```

It can be understood as follows:

```text
{{ }}
```

Tells AngularJS to evaluate the expression inside.

```javascript
$on.constructor('alert(1)')
```

Uses JavaScript function construction behavior to create a function containing `alert(1)`.

```javascript
()
```

Executes the generated function.

This caused the browser to run:

```javascript
alert(1)
```

### Steps Taken

Opened the lab in the browser.

Entered a random alphanumeric string into the search box.

Viewed the page source and confirmed that the reflected search value appeared inside an AngularJS-controlled area using `ng-app`.

Entered the following AngularJS expression into the search box:

```javascript
{{$on.constructor('alert(1)')()}}
```

Clicked Search.

Observed that the browser executed JavaScript and displayed an alert.

Successfully solved the lab.

### Result

The AngularJS expression executed successfully and triggered `alert(1)`.

This confirmed that the application was vulnerable to DOM-based XSS through AngularJS expression injection.

### What I Learned

AngularJS can evaluate expressions inside `{{ }}` when the page uses `ng-app`.

XSS does not always require `<script>` tags.

Encoding angle brackets can block HTML tag injection, but it does not fully prevent XSS in AngularJS contexts.

User input inside AngularJS-controlled HTML can be dangerous.

The correct XSS payload depends on the context where input appears.

AngularJS expression injection can execute JavaScript when user input is evaluated as an expression.

### Security Impact

In a real-world application, this vulnerability could allow attackers to execute JavaScript in a victim's browser.

This could lead to session abuse, unauthorized actions, phishing, page manipulation, or exposure of sensitive information depending on the application's security controls.

### Mitigation

To prevent this vulnerability, developers should:

Avoid placing user-controlled input inside AngularJS expression contexts.

Do not allow untrusted input to be evaluated by AngularJS.

Use safe output encoding based on the correct context.

Avoid using outdated AngularJS versions.

Use framework features that safely bind text instead of evaluating expressions.

Sanitize user-controlled input before rendering it.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Web Browser

Browser Developer Tools
