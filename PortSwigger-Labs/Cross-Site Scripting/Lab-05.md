## Lab 05: DOM XSS in jQuery Anchor `href` Attribute Sink Using `location.search`

### Platform

PortSwigger Web Security Academy

### Difficulty

Apprentice

### Topic

Cross-Site Scripting / DOM XSS / jQuery / `href` Attribute / `location.search`

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a DOM-based XSS vulnerability in the submit feedback page by controlling the `href` attribute of the Back link and making it execute JavaScript.

### Simple Explanation

In this lab, the vulnerability existed in the feedback page.

The page used JavaScript with jQuery to read the `returnPath` value from the URL.

Then it placed that value inside the `href` attribute of the Back link.

Because the application did not validate the value before inserting it into the `href`, it was possible to set the link to a JavaScript URL.

When the Back link was clicked, the browser executed the JavaScript instead of navigating to a normal page.

### Vulnerability Description

The application used `location.search` as a source of user-controlled input.

The value of the `returnPath` query parameter was assigned to the `href` attribute of the Back link using jQuery:

```javascript
$('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
```

Because the value was not safely validated, an attacker could set `returnPath` to a JavaScript URL.

This created a DOM-based XSS vulnerability.

### Key Concept

DOM XSS can occur when client-side JavaScript places user-controlled data into a dangerous location.

In this lab:

```text
Source: location.search
Sink: jQuery attr("href", ...)
```

The dangerous behavior happened because user-controlled input was inserted into an anchor tag's `href` attribute.

The final payload was URL-encoded:

```text
javascript%3Aalert(document.cookie)
```

After decoding, this becomes:

```javascript
javascript:alert(document.cookie)
```

### Payload Explanation

The payload used was:

```text
javascript%3Aalert(document.cookie)
```

It can be understood as follows:

```text
javascript:
```

Tells the browser to execute JavaScript when the link is clicked.

```javascript
alert(document.cookie)
```

Displays the page cookies in an alert box.

```text
%3A
```

Is the URL-encoded version of the colon character `:`.

The encoded payload was used because the unencoded version did not work correctly in the URL.

### Steps Taken

Opened the Submit feedback page.

Changed the `returnPath` query parameter to a test value.

Inspected the Back link and confirmed that the test value was inserted into the `href` attribute.

Changed the `returnPath` value to a JavaScript URL payload.

Used the encoded version of the payload:

```text
javascript%3Aalert(document.cookie)
```

Loaded the modified URL.

Clicked the Back link on the page.

Observed that the browser executed the JavaScript and displayed the cookie in an alert.

Successfully solved the lab.

### Final URL Pattern

The final URL used this structure:

```text
/feedback?returnPath=javascript%3Aalert(document.cookie)
```

### Result

The Back link executed JavaScript when clicked.

This confirmed that the application was vulnerable to DOM-based XSS through the jQuery `href` attribute sink.

### What I Learned

DOM XSS can happen when JavaScript reads data from the URL and writes it into the page unsafely.

`location.search` can be a source of user-controlled input.

jQuery's `.attr()` function can be dangerous if it assigns untrusted data to sensitive attributes.

The `href` attribute can execute JavaScript if it is set to a `javascript:` URL.

Sometimes URL encoding is needed for the payload to work correctly in a query parameter.

The change to the `href` attribute may not appear in View Source because it happens dynamically in the DOM after the page loads.

Browser Developer Tools should be used to inspect DOM changes.

### Security Impact

In a real-world application, this vulnerability could allow attackers to execute JavaScript when a victim clicks a crafted link.

This could lead to session theft, unauthorized actions, phishing, page manipulation, or abuse of the victim's authenticated session.

### Mitigation

To prevent this vulnerability, developers should:

Validate and restrict redirect or return path values.

Do not allow `javascript:` URLs in user-controlled link targets.

Use allowlists for safe return paths, such as internal relative URLs only.

Avoid assigning untrusted input directly to sensitive attributes like `href`.

Use safe URL parsing and validation.

Apply Content Security Policy as an additional defense.

Use secure client-side coding practices when handling URL parameters.

### Tools Used

PortSwigger Web Security Academy

Web Browser

Browser Developer Tools
