## Lab 01: Reflected XSS into HTML Context with Nothing Encoded

### Platform

PortSwigger Web Security Academy

### Difficulty

Apprentice

### Topic

Cross-Site Scripting / Reflected XSS / HTML Context

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a reflected cross-site scripting vulnerability in the search functionality and execute JavaScript in the browser.

### Simple Explanation

The application reflected the search input back into the HTML page without encoding it.

When a normal search term was entered, the application displayed it in the response.

However, when a JavaScript payload was entered, the browser interpreted it as executable code instead of plain text.

This allowed the payload to run in the context of the application.

### Vulnerability Description

The search functionality accepted user input and inserted it into the HTML response without proper output encoding.

Because special characters such as `<` and `>` were not encoded, the browser treated the injected input as HTML and executed the script.

This created a reflected XSS vulnerability.

### Key Concept

Reflected XSS occurs when user input is immediately returned in the application's response and executed by the browser.

In this lab, the input was reflected into an HTML context with no encoding.

The payload used was:

```html
<script>alert(1)</script>
```

This payload triggered an alert box, confirming that JavaScript execution was possible.

### Steps Taken

Opened the lab in the browser.

Located the search functionality.

Entered the following payload into the search box:

```html
<script>alert(1)</script>
```

Clicked the Search button.

Observed that the browser executed the JavaScript and displayed an alert.

Successfully solved the lab.

### Result

The JavaScript payload executed successfully in the browser.

This confirmed that the application was vulnerable to reflected XSS.

### What I Learned

Reflected XSS happens when user input is returned immediately in the response.

If input is inserted into HTML without encoding, the browser may treat it as code.

The `<script>` tag can be used to test whether JavaScript execution is possible.

`alert(1)` is commonly used in labs as a safe proof of JavaScript execution.

Output encoding is important when displaying user-controlled input.

### Security Impact

In a real-world application, reflected XSS could allow attackers to execute JavaScript in a victim's browser.

This could be used to steal session data, perform actions as the victim, modify page content, or create phishing attacks.

### Mitigation

To prevent this vulnerability, developers should:

Use context-aware output encoding.

Encode special HTML characters such as `<`, `>`, `"`, `'`, and `&`.

Avoid inserting user input directly into HTML.

Validate input where appropriate.

Use secure frameworks that automatically escape output.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Web Browser
