## Lab 17: Reflected XSS in Canonical Link Tag

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Reflected XSS / Attribute Injection / Canonical Link Tag

### Lab Status

Solved

### Objective

The goal of this lab was to exploit reflected XSS inside a canonical link tag by injecting attributes that call the `alert()` function.

### Simple Explanation

This lab reflected user-controlled input inside a canonical link tag.

A canonical link tag is usually placed inside the page `<head>` and is used to define the main URL of the page:

```html
<link rel="canonical" href="https://example.com/">
```

The application escaped angle brackets, so injecting a new HTML tag such as `<script>` was not possible.

Instead, the solution was to inject new attributes into the existing canonical link tag.

The payload added an `accesskey` attribute and an `onclick` event handler.

### Vulnerability Description

The application reflected part of the URL inside the `href` attribute of a canonical link tag.

Although angle brackets were escaped, the application did not safely handle single quotes in the attribute context.

This allowed attribute injection inside the existing canonical link tag.

By injecting `accesskey` and `onclick`, JavaScript could be executed when the access key was activated.

### Key Concept

This lab did not require creating a new HTML tag.

Instead, the attack worked by injecting attributes into an existing HTML tag.

In this lab:

```text
Injection context: canonical link href attribute
Injected attribute: accesskey
Event handler: onclick
Executed function: alert(1)
```

### Payload Used

The payload added directly to the URL was:

```text
?%27accesskey=%27x%27onclick=%27alert(1)
```

Decoded, it becomes:

```text
?'accesskey='x'onclick='alert(1)
```

This injected the following attributes:

```html
accesskey='x'
onclick='alert(1)'
```

### Payload Explanation

The payload used `%27`, which represents a single quote:

```text
%27 = '
```

The single quote broke out of the existing `href` attribute value.

Then the payload injected:

```html
accesskey='x'
```

This assigned the `x` key as an access key.

It also injected:

```html
onclick='alert(1)'
```

This created an event handler that executes JavaScript when the access key activates the element.

### Steps Taken

Opened the lab in Chrome.

Added the following payload directly to the URL:

```text
?%27accesskey=%27x%27onclick=%27alert(1)
```

Loaded the modified URL in the browser.

The payload was reflected into the canonical link tag.

The injected attributes added an access key and an onclick event handler:

```html
accesskey='x'
onclick='alert(1)'
```

The lab was solved after the payload was loaded in the browser.

This confirmed that attribute injection was possible inside the canonical link tag.

### Result

The lab was solved after loading the URL containing the injected attributes.

This confirmed that the application was vulnerable to reflected XSS through attribute injection inside the canonical link tag.

### What I Learned

I learned that XSS can happen inside existing HTML tags by injecting attributes, even when angle brackets are escaped.

### Security Impact

An attacker could craft a malicious URL that injects JavaScript into the page through an existing tag.

### Mitigation

Use context-aware output encoding for HTML attributes and avoid reflecting raw URL input inside tag attributes.

### Tools Used

PortSwigger Web Security Academy

Chrome Browser

Browser Developer Tools
