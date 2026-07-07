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

### Lab 05: DOM XSS in jQuery Anchor `href` Attribute Sink Using `location.search`

This lab focused on DOM-based XSS through an anchor tag's `href` attribute.

The source was:

```javascript
location.search
```

This means the application read data from the URL query string.

The sink was jQuery's `.attr()` function:

```javascript
$('#backLink').attr("href", value)
```

The application took the `returnPath` parameter from the URL and placed it inside the Back link's `href` attribute.

A normal value such as:

```text
/feedback
```

would make the Back link navigate to that page.

However, by setting the value to a JavaScript URL, the link could execute JavaScript when clicked.

The payload used was:

```text
javascript%3Aalert(document.cookie)
```

After URL decoding, this becomes:

```javascript
javascript:alert(document.cookie)
```

When the Back link was clicked, the browser executed the JavaScript and displayed the cookie.

#### Key Takeaway

DOM XSS can occur when user-controlled URL parameters are assigned to sensitive HTML attributes such as `href`.

Applications should validate return paths and should not allow dangerous URL schemes like `javascript:`.

### Lab 06: DOM XSS in jQuery Selector Sink Using a Hashchange Event

This lab focused on DOM-based XSS using the URL hash and a jQuery selector sink.

The source was:

```javascript
location.hash
```

This means the application read data from the part of the URL after the `#` symbol.

The event used was:

```javascript
hashchange
```

This event runs when the hash part of the URL changes.

The sink was jQuery's selector function:

```javascript
$()
```

The application expected the hash to contain a normal post title or selector value that could be used to scroll to a blog post.

However, because the hash value was user-controlled, it was possible to inject HTML instead of a normal selector.

The exploit used an iframe:

```html
<iframe src="https://0a9900f104e223f380a5037500a000be.web-security-academy.net/#" onload="this.src=this.src+'<img src=x onerror=print()>'"></iframe>
```

The iframe first loaded the vulnerable page with an empty hash.

After the iframe loaded, the `onload` event appended the payload to the hash.

The payload was:

```html
<img src=x onerror=print()>
```

The invalid image source caused the image to fail loading.

When the image failed to load, the `onerror` event executed:

```javascript
print()
```

#### Key Takeaway

DOM XSS can happen when client-side JavaScript uses `location.hash` unsafely.

If user-controlled hash values are passed into dangerous sinks such as jQuery's `$()` selector, an attacker may inject HTML or JavaScript behavior.

The `hashchange` event can be abused to trigger vulnerable JavaScript after the page has loaded.

### Lab 07: Reflected XSS into Attribute with Angle Brackets HTML-Encoded

This lab focused on reflected XSS in an HTML attribute context.

The search input was reflected inside a quoted attribute.

Angle brackets were HTML-encoded, so payloads such as:

```html
<script>alert(1)</script>
```

would not work because the browser would treat the angle brackets as text instead of HTML.

The successful technique was to break out of the existing attribute value and inject a new event handler attribute.

The payload used was:

```html
"onmouseover="alert(1)
```

The URL-encoded version was:

```text
%22onmouseover=%22alert(1)
```

The first quotation mark closed the original attribute value.

Then the payload added:

```html
onmouseover="alert(1)"
```

When the mouse moved over the affected element, the browser executed the JavaScript.

#### Key Takeaway

Encoding `<` and `>` is not always enough to prevent XSS.

If user input is inserted inside an HTML attribute, quotation marks and attribute-specific characters must also be encoded properly.

XSS prevention must be context-aware because the correct defense depends on where the input appears in the page.
