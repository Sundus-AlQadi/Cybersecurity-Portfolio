## Cross-Site Scripting Notes

### What is XSS?

Cross-Site Scripting, also known as XSS, is a web security vulnerability that allows an attacker to inject JavaScript into a web page viewed by other users.

XSS happens when an application takes user-controlled input and displays it in a page without proper output encoding or sanitization.

If the browser interprets the input as code, the injected JavaScript can execute in the context of the vulnerable website.

### Why XSS Matters

JavaScript running inside a trusted website can interact with the page and the user's session.

Depending on the application and browser protections, XSS may allow attackers to:

View sensitive page content

Perform actions as the victim

Modify page content

Steal tokens if they are accessible to JavaScript

Create phishing forms inside the trusted website

Redirect users to malicious pages

### Common XSS Types

#### Reflected XSS

Reflected XSS occurs when user input is immediately included in the application's response.

Example flow:

```text
User input → Server response → Browser executes payload
```

A common example is a search page that reflects the search term back to the user without encoding.

#### Stored XSS

Stored XSS occurs when the payload is saved by the application and displayed later.

Example flow:

```text
User submits comment → Application stores comment → Another user views page → Browser executes payload
```

Stored XSS is usually more dangerous because it can affect many users without requiring each user to click a special link.

#### DOM-Based XSS

DOM-based XSS occurs when client-side JavaScript takes user-controlled data and writes it into the page in an unsafe way.

In this case, the vulnerability is mainly in the browser-side JavaScript rather than the server response.

### HTML Context

HTML context means the user input is placed directly inside the HTML body of the page.

Example:

```html
<p>You searched for: USER_INPUT</p>
```

If the application inserts this input without encoding, a payload such as:

```html
<script>alert(1)</script>
```

may be interpreted by the browser as JavaScript code.

### Output Encoding

Output encoding means converting dangerous characters into safe HTML entities.

Examples:

```text
< becomes &lt;
> becomes &gt;
" becomes &quot;
' becomes &#x27;
& becomes &amp;
```

If output is properly encoded, the browser displays the payload as text instead of executing it as code.

### Lab 01: Reflected XSS into HTML Context with Nothing Encoded

In this lab, the vulnerability existed in the search functionality.

The search input was reflected back into the HTML page without encoding.

The payload used was:

```html
<script>alert(1)</script>
```

The browser executed the payload because the application inserted it into the page as HTML instead of treating it as text.

#### Key Takeaway

Reflected XSS can happen when user input is immediately returned in the response without output encoding.

### Lab 02: Stored XSS into HTML Context with Nothing Encoded

In this lab, the vulnerability existed in the comment functionality.

The comment was stored by the application and later displayed on the blog post page without encoding.

The payload used was:

```html
<script>alert(1)</script>
```

The browser executed the payload when the blog post was viewed.

#### Key Takeaway

Stored XSS can happen when user-generated content is saved and later displayed without output encoding.

Stored XSS is often more dangerous than reflected XSS because it can affect multiple users.

### Main Difference Between Reflected and Stored XSS

| Type          | Where the payload goes   | When it executes                 |
| ------------- | ------------------------ | -------------------------------- |
| Reflected XSS | In the request           | Immediately in the response      |
| Stored XSS    | Saved in the application | When the affected page is viewed |

### Main Defense

The main defense against XSS is context-aware output encoding.

Applications should encode user-controlled input before displaying it in HTML, attributes, JavaScript, or URLs.

Input validation can help, but it should not replace output encoding.

Content Security Policy can also reduce XSS impact, but it should be used as an additional layer, not the only protection.

### Lab 03: DOM XSS in `document.write` Sink Using Source `location.search`

DOM-based XSS happens when the vulnerability exists in client-side JavaScript.

In this lab, the source was:

```javascript
location.search
```

This means the application read data from the URL query string.

The sink was:

```javascript
document.write()
```

This function writes content directly into the page.

The search input was placed inside an `img src` attribute. Because the input was not safely handled, it was possible to break out of the attribute and inject new HTML.

The payload used was:

```html
"><svg onload=alert(1)>
```

The first part closed the existing attribute and tag:

```html
">
```

Then a new SVG element was injected:

```html
<svg onload=alert(1)>
```

When the SVG loaded, the browser executed the JavaScript and displayed an alert.

#### Key Takeaway

DOM XSS can happen even when the server does not directly return the payload in the response.

If client-side JavaScript reads user-controlled data and writes it into the page using unsafe sinks like `document.write()`, JavaScript execution may be possible.

### Lab 04: DOM XSS in `innerHTML` Sink Using Source `location.search`

This lab focused on DOM-based XSS using the `innerHTML` sink.

The source was:

```javascript
location.search
```

This means the application read data from the URL query string.

The sink was:

```javascript
innerHTML
```

This is dangerous because `innerHTML` treats inserted content as HTML, not just plain text.

The payload used was:

```html
<img src=1 onerror=alert(1)>
```

The payload created an image element with an invalid source.

Because the image failed to load, the browser triggered the `onerror` event handler.

This executed:

```javascript
alert(1)
```

#### Key Takeaway

DOM XSS can occur when client-side JavaScript reads user-controlled data and inserts it into the page using unsafe sinks like `innerHTML`.

When displaying user-controlled text, safer APIs such as `textContent` should be used instead of `innerHTML`.
