# Burp Suite Fundamentals

## What is Burp Suite?

Burp Suite is a web application security testing platform used to inspect, modify, analyze, and replay HTTP requests and responses between a client and a web application.

It is one of the most widely used tools in web application security testing.

---

## Proxy

### Purpose

Captures HTTP requests and responses between the browser and the target application.

### How it Helps

* View application traffic
* Identify parameters
* Observe cookies and sessions
* Discover hidden functionality
* Intercept requests before they reach the server

### Common Usage

* Finding SQL injection parameters
* Finding IDOR vulnerabilities
* Observing authentication flows

---

## HTTP History

### Purpose

Stores all captured requests and responses.

### How it Helps

* Review application behavior
* Locate interesting endpoints
* Find login requests
* Identify API requests

### Common Usage

* Finding password reset requests
* Finding role modification requests
* Reviewing application traffic

---

## Repeater

### Purpose

Allows requests to be modified and resent manually.

### How it Helps

* Test vulnerabilities
* Modify parameters
* Change cookies
* Change headers
* Observe server responses

### Common Usage

* SQL Injection
* Access Control Testing
* IDOR Testing
* Authentication Testing

---

## Intruder

### Purpose

Automates repeated requests using payload lists.

### How it Helps

* Brute force testing
* Enumeration attacks
* Automated parameter testing

### Common Usage

* Username enumeration
* Password brute force
* Authentication testing

---

## Decoder

### Purpose

Encodes and decodes data into various formats.

### How it Helps

* Base64 decoding
* URL decoding
* Hex conversion
* Hash inspection

### Common Usage

* Cookie analysis
* Token analysis
* Encoded payload inspection

---

## Comparer

### Purpose

Compares requests or responses.

### How it Helps

* Identify differences
* Compare authorization responses
* Compare application behavior

---

## Extensions

### Purpose

Add additional functionality to Burp Suite.

### Examples

* Turbo Intruder
* Logger++
* Additional testing utilities

---

## Key Takeaway

Burp Suite allows security testers to observe, manipulate, replay, and analyze web application traffic. Understanding how each module works is essential for web application security testing.
