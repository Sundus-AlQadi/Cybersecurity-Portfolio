# Lab 09: Offline Password Cracking

## Platform

PortSwigger Web Security Academy

## Difficulty

Practitioner

## Topic

Authentication Vulnerabilities / Remember-Me Cookies / XSS / Offline Password Cracking

## Lab Status

Solved

## Objective

The goal of this lab was to obtain the victim user's stay-logged-in cookie, extract the password hash stored inside it, crack the hash offline, and use the recovered password to access the victim account in a controlled lab environment.

## Purpose of the Lab

This lab demonstrates how multiple weaknesses can be chained together to create a serious account takeover vulnerability.

The application had two main issues:

1. The stay-logged-in cookie stored a password hash.
2. The comment functionality was vulnerable to stored XSS.

By combining these issues, it was possible to steal the victim user's cookie, extract the password hash, crack it offline, and log in as the victim user.

## Simple Explanation

The application allowed users to stay logged in by storing a cookie in the browser.

After analyzing the cookie, I found that it was Base64-encoded. When decoded, the cookie contained the username and an MD5 hash of the user's password.

The cookie followed this structure:

```text
username:md5(password)
```

This is insecure because the cookie exposes password-derived data to the client side.

The application also had a stored XSS vulnerability in the comment functionality. This allowed a malicious script to run in the victim user's browser and send the victim's cookies to the exploit server.

After obtaining the victim user's stay-logged-in cookie, I decoded it, extracted the MD5 hash, cracked the hash offline, and used the recovered password to log in as the victim user.

## Vulnerability Description

The application was vulnerable because it stored sensitive password-derived data inside a client-side cookie.

Although the password itself was not stored directly, the cookie contained an MD5 hash of the password. MD5 is fast and weak for password-related security, which makes offline cracking easier if the password is weak or common.

The stored XSS vulnerability made the issue more dangerous because it allowed the victim user's cookie to be stolen from their browser.

## Key Concept

Password hashes should never be stored in client-side cookies.

Remember-me functionality should use random, high-entropy, server-generated tokens instead of predictable values based on usernames or passwords.

Stored XSS can also be dangerous because it may allow attackers to steal cookies or perform actions as the victim user.

## Steps Taken

1. Opened the lab from PortSwigger Web Security Academy.
2. Logged in using the provided personal credentials.
3. Selected the `Stay logged in` option.
4. Captured the stay-logged-in cookie using Burp Suite.
5. Decoded the cookie using Base64 decoding.
6. Observed that the decoded cookie contained a username and an MD5 password hash.
7. Identified the cookie structure as:

```text
username:md5(password)
```

8. Tested the blog comment functionality and confirmed that it was vulnerable to stored XSS.
9. Opened the exploit server and copied the exploit server domain.
10. Submitted a stored XSS payload in a blog comment to send the victim user's cookies to the exploit server.
11. Opened the exploit server access log.
12. Found the victim user's stay-logged-in cookie in the access log.
13. Decoded the victim user's cookie using Base64 decoding.
14. Extracted the MD5 password hash from the decoded cookie.
15. Performed offline password cracking in the controlled lab environment.
16. Recovered the victim user's password.
17. Logged in as the victim user using the recovered credentials.
18. Opened the victim user's account page.
19. Deleted the victim user's account.
20. The lab was successfully solved.

## Result

The victim user's password was recovered by cracking the hash extracted from the stolen stay-logged-in cookie.

The victim account was accessed successfully, and the account was deleted to solve the lab.

## What I Learned

* Base64 is encoding, not encryption.
* MD5 is not suitable for password security.
* Password hashes should not be stored in client-side cookies.
* Stored XSS can be used to steal cookies from a victim user's browser.
* Offline password cracking does not require repeated login attempts against the website.
* Multiple vulnerabilities can be chained together to achieve account takeover.
* Remember-me cookies should be random server-side tokens, not predictable values.

## Security Impact

In a real-world application, this vulnerability could allow attackers to steal authentication cookies, extract password hashes, crack passwords offline, and take over user accounts.

This can also increase the risk of password reuse attacks if users reuse the same password across multiple services.

## Mitigation

To prevent this vulnerability, developers should:

* Never store passwords or password hashes in client-side cookies.
* Use random, high-entropy remember-me tokens generated by the server.
* Store remember-me tokens securely on the server side.
* Protect cookies using `HttpOnly`, `Secure`, and `SameSite` attributes.
* Avoid using MD5 for password-related security.
* Use strong password hashing algorithms such as bcrypt, scrypt, or Argon2.
* Sanitize and encode user-generated content to prevent XSS.
* Apply a strong Content Security Policy.
* Invalidate remember-me tokens after logout or password change.
* Monitor suspicious authentication and cookie usage.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Decoder
* Exploit Server
* Web browser
