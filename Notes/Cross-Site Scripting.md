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
