## Lab 12: Reflected DOM XSS

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Reflected DOM XSS / JSON Response / `eval()` Sink

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a reflected DOM XSS vulnerability and execute the `alert()` function.

### Simple Explanation

This lab demonstrated a reflected DOM XSS vulnerability.

The search input was sent to the server, and the server reflected it inside a JSON response.

After that, JavaScript on the page processed the reflected JSON response unsafely using the dangerous `eval()` function.

Because `eval()` can execute text as JavaScript code, it was possible to inject a payload that broke out of the JSON string and executed `alert(1)`.

### Vulnerability Description

The application reflected the user-controlled search input inside a JSON response from the `/search-results` endpoint.

The page then used client-side JavaScript to process this response.

The JavaScript file `searchResults.js` used the dangerous `eval()` function to handle the JSON response.

Although quotation marks were escaped, the backslash character was not escaped correctly.

This allowed the payload to bypass the escaping and break out of the string context.

### Key Concept

Reflected DOM XSS happens when:

```text id="5jx269"
User input → Server response → Client-side JavaScript processes it unsafely → Dangerous sink executes it
```

In this lab:

```text id="fh1qg5"
Reflected input: search parameter
Response type: JSON
Dangerous sink: eval()
Payload: \"-alert(1)}//
```

### Payload Used

The payload used was:

```text id="yow3d4"
\"-alert(1)}//
```

URL-encoded version:

```text id="pm2vuu"
%5C%22-alert%281%29%7D%2F%2F
```

### Payload Explanation

The payload was:

```text id="4smpms"
\"-alert(1)}//
```

It can be understood as follows:

```text id="moydrq"
\
```

Uses a backslash to interfere with the application's escaping behavior.

```text id="bjhv9q"
"
```

Breaks out of the JSON string after the escaping is bypassed.

```javascript id="xnz0r5"
-alert(1)
```

Uses the subtraction operator to keep the JavaScript syntax valid and execute `alert(1)`.

```text id="6hn9ar"
}
```

Closes the JSON object early.

```text id="vkq8x6"
//
```

Comments out the rest of the response to avoid syntax errors.

### Example Response

The server generated a response similar to:

```json id="1wh8ab"
{"searchTerm":"\\"-alert(1)}//", "results":[]}
```

Because the page used `eval()` to process this response, the injected JavaScript was executed by the browser.

### Steps Taken

Opened Burp Suite and enabled Intercept.

Used the search bar in the lab to search for a random test string:

```text id="4y10jh"
xss
```

Forwarded the intercepted request in Burp.

Observed that the search input was reflected inside a JSON response from:

```text id="r8swzq"
/search-results?search=xss
```

Opened the site map in Burp Suite.

Checked the JavaScript resources and found the file:

```text id="lv951o"
searchResults.js
```

Reviewed the JavaScript file and confirmed that the JSON response was processed using `eval()`.

Tested the final payload in the search input:

```text id="1jkuhs"
\"-alert(1)}//
```

Submitted the search request.

The browser executed the injected JavaScript and displayed an alert.

Successfully solved the lab.

### Result

The payload executed successfully and triggered `alert(1)`.

This confirmed that the application was vulnerable to reflected DOM XSS because reflected server-side data was processed unsafely by client-side JavaScript using `eval()`.

### What I Learned

Reflected DOM XSS combines reflected input and unsafe client-side JavaScript processing.

The vulnerability is not only in the server response and not only in the DOM.

The server reflects the input, but the browser-side JavaScript makes it dangerous by passing the response to a dangerous sink.

JSON responses can still lead to XSS if they are processed unsafely.

The `eval()` function is dangerous because it executes strings as JavaScript code.

Escaping quotation marks is not enough if backslashes are not escaped correctly.

Payloads can use operators and comments to keep JavaScript syntax valid after breaking out of a string.

### Security Impact

In a real-world application, an attacker could craft a malicious search URL and send it to a victim.

When the victim opens the URL, the server reflects the payload in the response, and the client-side script executes it.

This could allow attackers to perform actions as the victim, manipulate page content, steal sensitive information, or conduct phishing attacks depending on the application's protections.

### Mitigation

To prevent this vulnerability, developers should:

Avoid using `eval()` to process JSON responses.

Use safe JSON parsing methods such as `JSON.parse()`.

Properly escape both quotation marks and backslashes.

Treat all reflected data as untrusted.

Validate and sanitize user-controlled input.

Apply context-aware output encoding.

Avoid building JavaScript code using user-controlled data.

Use secure client-side coding practices.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Target Site Map

Web Browser

Browser Developer Tools
