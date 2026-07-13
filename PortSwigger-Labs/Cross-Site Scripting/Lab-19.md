## Lab 19: Reflected XSS into a JavaScript String with Angle Brackets and Double Quotes HTML-Encoded and Single Quotes Escaped

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Reflected XSS / JavaScript String Context / Escaping Bypass

### Lab Status

Solved

### Objective

The goal of this lab was to exploit reflected XSS inside a JavaScript string where angle brackets and double quotes were HTML-encoded, and single quotes were escaped.

### Simple Explanation

The search input was reflected inside a JavaScript string.

The application encoded angle brackets and double quotes, so injecting HTML tags was not possible.

It also escaped single quotes, which prevented a normal JavaScript string breakout.

However, the application did not escape backslashes.

By adding a backslash before the single quote, the escaping logic was bypassed, allowing the payload to break out of the JavaScript string and execute `alert(1)`.

### Vulnerability Description

The application reflected the search query inside an inline JavaScript string.

The application escaped single quotes by adding a backslash before them, but it did not escape existing backslashes from user input.

This allowed a payload using both a backslash and a single quote to break out of the JavaScript string.

### Key Concept

The vulnerability happened because the application escaped one dangerous character but failed to handle another character that affected JavaScript escaping.

In this lab:

```text id="g82a3z"
Reflection context: JavaScript string
Single quote: escaped
Backslash: not escaped
Payload: \'-alert(1)//
```

### Payload Used

The payload used was:

```javascript id="sgb03w"
\'-alert(1)//
```

### Payload Explanation

The payload was:

```javascript id="06d4zk"
\'-alert(1)//
```

It can be understood as follows:

```text id="omvj7v"
\
```

Uses a backslash that the application does not escape.

```text id="7dylre"
'
```

Breaks out of the JavaScript string after the escaping is bypassed.

```javascript id="j35gvv"
-alert(1)
```

Uses the subtraction operator to keep the JavaScript syntax valid while calling `alert(1)`.

```javascript id="n3pwz1"
//
```

Comments out the rest of the JavaScript line to avoid syntax errors.

### Example

The page originally reflected the input inside a JavaScript string similar to:

```javascript id="tnmi0f"
var searchTerms = 'USER_INPUT';
```

After injecting the payload, the JavaScript became similar to:

```javascript id="v9m5xj"
var searchTerms = '\\'-alert(1)//';
```

The double backslash caused the quote to become unescaped, which closed the string and allowed `alert(1)` to execute.

### Steps Taken

Opened the lab in the browser.

Submitted a random search value to identify where the input was reflected.

Observed that the input was reflected inside a JavaScript string.

Tested the following input:

```text id="fyd9jn"
test'payload
```

Observed that the single quote was escaped.

Tested the following input:

```text id="zaqhck"
test\payload
```

Observed that the backslash was not escaped.

Used the final payload:

```javascript id="i7nhui"
\'-alert(1)//
```

The browser executed the injected JavaScript and displayed an alert.

Successfully solved the lab.

### Result

The payload executed successfully and triggered `alert(1)`.

This confirmed that the application was vulnerable to reflected XSS due to incomplete JavaScript string escaping.

### What I Learned

I learned how an unescaped backslash can bypass single quote escaping inside a JavaScript string.

### Security Impact

An attacker could craft a malicious URL that executes JavaScript when opened by a victim.

### Mitigation

Escape JavaScript string characters correctly, including backslashes, and avoid placing raw user input inside inline JavaScript.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Repeater

Web Browser

Browser Developer Tools
