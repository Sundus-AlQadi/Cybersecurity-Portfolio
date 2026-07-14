# Cross-Site Scripting Vulnerabilities

This folder contains selected documentation from the Cross-Site Scripting learning path in PortSwigger Web Security Academy.

The learning path covered reflected XSS, stored XSS, DOM-based XSS, JavaScript contexts, HTML attribute contexts, event handlers, AngularJS expression injection, SVG-based XSS, WAF bypass techniques, and real-world XSS impact such as bypassing CSRF defenses.

## Topics Covered

Cross-Site Scripting

Reflected XSS

Stored XSS

DOM-based XSS

HTML context injection

HTML attribute injection

Anchor `href` attribute injection

JavaScript URL payloads

Event handler injection

`onmouseover`, `onerror`, `onfocus`, `onclick`, `onresize`, and `onbegin` events

DOM sources such as `location.search` and `location.hash`

Dangerous DOM sinks such as `innerHTML`, `document.write()`, jQuery `.attr()`, and jQuery selector sinks

JavaScript string context

Script block breakout

Backslash and quote escaping bypasses

JavaScript template literal injection

AngularJS expression injection

Stored DOM XSS

Reflected DOM XSS

JSON response processing

Unsafe `eval()` usage

WAF bypass methodology

Custom tag injection

SVG markup-based XSS

Canonical link tag attribute injection

CSRF token theft through XSS

XSS impact concepts such as cookie theft and password capture

Burp Collaborator tool limitation

## Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Burp Intruder

Burp Target Site Map

XSS Cheat Sheet

Exploit Server

Web Browser

Chrome Browser

Browser Developer Tools

GitHub

## Skills Practiced

Identifying reflected, stored, and DOM-based XSS

Finding where user input is reflected

Understanding HTML, attribute, JavaScript, and DOM contexts

Building context-aware XSS payloads

Testing URL parameters, search fields, comments, and website fields

Using event handlers to execute JavaScript

Using JavaScript URLs inside `href` attributes

Breaking out of HTML attributes

Breaking out of JavaScript strings

Breaking out of script blocks

Using template literal expression injection

Testing dangerous DOM sources and sinks

Using Burp Repeater to modify requests

Using Burp Intruder to test allowed tags and attributes

Using the XSS Cheat Sheet to identify payload options

Testing WAF and filter behavior

Using SVG tags and events for XSS

Understanding how stored XSS can affect other users

Understanding how XSS can bypass CSRF defenses

Documenting payloads, context, impact, and mitigations

## Completed Labs

Lab 01: Reflected XSS into HTML Context with Nothing Encoded

Lab 02: Stored XSS into HTML Context with Nothing Encoded

Lab 03: DOM XSS in `document.write` Sink Using Source `location.search`

Lab 04: DOM XSS in `innerHTML` Sink Using Source `location.search`

Lab 05: DOM XSS in jQuery Anchor `href` Attribute Sink Using `location.search`

Lab 06: DOM XSS in jQuery Selector Sink Using a Hashchange Event

Lab 07: Reflected XSS into Attribute with Angle Brackets HTML-Encoded

Lab 08: Stored XSS into Anchor `href` Attribute with Double Quotes HTML-Encoded

Lab 09: Reflected XSS into a JavaScript String with Angle Brackets HTML-Encoded

Lab 10: DOM XSS in `document.write` Sink Using Source `location.search` Inside a `select` Element

Lab 11: DOM XSS in AngularJS Expression with Angle Brackets and Double Quotes HTML-Encoded

Lab 12: Reflected DOM XSS

Lab 13: Stored DOM XSS

Lab 14: Reflected XSS into HTML Context with Most Tags and Attributes Blocked

Lab 15: Reflected XSS into HTML Context with All Tags Blocked Except Custom Ones

Lab 16: Reflected XSS with Some SVG Markup Allowed

Lab 17: Reflected XSS in Canonical Link Tag

Lab 18: Reflected XSS into a JavaScript String with Single Quote and Backslash Escaped

Lab 19: Reflected XSS into a JavaScript String with Angle Brackets and Double Quotes HTML-Encoded and Single Quotes Escaped

Lab 20: Stored XSS into `onclick` Event with Angle Brackets and Double Quotes HTML-Encoded and Single Quotes and Backslash Escaped

Lab 21: Reflected XSS into a Template Literal with Multiple Characters Escaped

Lab 24: Exploiting XSS to Bypass CSRF Defenses

## Pending Labs

Lab 22: Exploiting XSS to Steal Cookies

Lab 23: Exploiting XSS to Capture Passwords

These labs were reviewed and understood conceptually, but they were marked as pending because the intended solutions require Burp Collaborator / Burp Suite Professional.

The current setup uses Burp Suite Community Edition, so these labs will be revisited later when Collaborator access is available.

## Deferred Labs

The remaining Expert-level XSS labs are deferred for future study.

These include advanced topics such as AngularJS sandbox escape, CSP bypass, strict CSP scenarios, and highly restricted JavaScript contexts.

They are not part of the current short-term goal because the focus is to build strong practical coverage across multiple vulnerability categories before the upcoming review meeting.

## What I Learned

XSS happens when user-controlled input is handled in a way that allows JavaScript to execute in the browser.

The correct payload depends on the context where the input appears.

HTML context, attribute context, JavaScript string context, template literal context, and DOM context all require different techniques.

Reflected XSS executes through malicious links.

Stored XSS is more dangerous because the payload is saved and can affect other users.

DOM XSS happens when client-side JavaScript reads unsafe input and writes it into the page using dangerous sinks.

Encoding only some characters is not enough if the application does not handle the full context safely.

Escaping quotes can fail if backslashes or other JavaScript syntax characters are not handled correctly.

WAFs and filters can block common payloads but still miss custom tags, SVG tags, or event handlers.

XSS can have real impact beyond `alert(1)`, including stealing tokens, changing account data, or bypassing CSRF protections.

## Security Impact

XSS vulnerabilities can allow attackers to:

Execute JavaScript in a victim's browser

Modify page content

Perform actions as another user

Steal readable cookies or tokens

Capture sensitive input

Bypass CSRF defenses

Abuse authenticated sessions

Deliver phishing content inside a trusted website

Stored XSS is especially serious because the payload can affect multiple users who view the infected content.

## Mitigation Summary

To prevent XSS, developers should:

Use context-aware output encoding.

Avoid inserting raw user input into HTML, attributes, JavaScript, or DOM sinks.

Use safe DOM APIs such as `textContent`.

Avoid dangerous functions such as `eval()` and unsafe `innerHTML`.

Sanitize user-generated HTML with a trusted sanitizer when HTML is required.

Validate input based on expected values.

Avoid relying only on WAF rules or tag blacklists.

Escape JavaScript string characters correctly when data must appear inside JavaScript.

Use secure frameworks that escape output by default.

Use Content Security Policy as an additional defense.

Set sensitive cookies with the `HttpOnly` flag where possible.

## Notes

All labs were completed in controlled PortSwigger Web Security Academy environments for educational purposes.

Labs 22 and 23 are currently pending due to Burp Collaborator access requirements.

Expert-level XSS labs are planned for future study after completing more vulnerability categories.

The current XSS documentation focuses on core practical skills, payload construction, context analysis, and realistic impact.
