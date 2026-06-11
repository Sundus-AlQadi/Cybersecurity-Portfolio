# Authentication Notes

## Username Enumeration

Username enumeration happens when an application reveals whether a username exists.

This can happen through differences in:

- Error messages
- Response length
- Status codes
- Response time

For example, a login page may return one message for an invalid username and a different message for a valid username with an incorrect password.

This allows attackers to identify valid usernames before attempting password attacks.

## Password Brute Force

Password brute force is the process of trying many possible passwords until the correct one is found.

In controlled lab environments, this can be practiced using a provided password wordlist.

In real applications, brute-force attempts can be dangerous if there is no rate limiting, account lockout, or monitoring.

## Useful Response Indicators

During authentication testing, different response indicators can reveal important information:

- Different response messages may reveal valid usernames.
- Different response lengths may show that one response is unusual.
- A `302` status code may indicate successful login if the application redirects after authentication.
- A `200` status code usually means the login page was returned again.

### Lab 01: Username Enumeration via Different Responses

In this lab, I used Burp Intruder to test a list of candidate usernames.

Most invalid usernames returned the same response. One username returned a different response indicating that the password was incorrect, which showed that the username was valid.

After identifying the valid username, I used the provided password list to test the password parameter. The successful login attempt returned a `302` response.

## Key Takeaway

Authentication systems should avoid giving different responses for invalid usernames and incorrect passwords. Generic error messages, rate limiting, account lockout, monitoring, and multi-factor authentication can reduce the risk.

## Username Enumeration via Subtle Response Differences

Username enumeration can occur even when the application uses a generic error message.

Sometimes the difference between invalid and valid usernames is very small, such as:

- A missing full stop
- An extra space
- A typo
- A slightly different response length
- A slightly different HTML structure

Because these differences may be hard to notice manually, Burp Intruder's Grep - Extract feature can be used to extract the error message and compare it across responses.

### Lab 02: Subtly Different Responses

In this lab, the application returned almost identical login error messages.

I used Burp Intruder to test candidate usernames and used Grep - Extract to extract the error message from each response. One response contained a subtle difference, which revealed the valid username.

After identifying the valid username, I tested the password parameter using the provided password list. The successful login attempt returned a `302` status code.

### Key Takeaway
Authentication responses must be consistent. Even small differences in wording, punctuation, spacing, response length, or status code can leak information about valid usernames.

## Username Enumeration via Response Timing

Username enumeration can happen through timing differences even when the application uses a generic error message.

If the username is invalid, the application may reject the login attempt quickly. If the username is valid, the application may spend additional time checking the password.

Using a long password can make this timing difference more noticeable.

### X-Forwarded-For and Rate Limit Bypass

Some applications use the client IP address to limit repeated login attempts.

In this lab, the application trusted the `X-Forwarded-For` header. By changing this header for each request, the IP-based brute-force protection could be bypassed in the controlled lab environment.

### Lab 03: Username Enumeration via Response Timing

In this lab, I used Burp Intruder with a Pitchfork attack.

One payload position was used for the `X-Forwarded-For` header, and another payload position was used for candidate usernames. A long static password was used to increase the timing difference.

The valid username was identified by comparing response completion times.

After identifying the username, I used a second Intruder attack to test candidate passwords. The successful login attempt returned a `302` status code.

### Key Takeaway

Authentication systems should avoid leaking information through response time. Rate limiting should also rely on trusted client information and should not trust user-controlled headers.

### Lab 04: Broken Brute-Force Protection: IP Block

Some applications block an IP address after a number of failed login attempts.

However, this protection can be weak if it only counts consecutive failed attempts and resets the counter after a successful login.

If a successful login resets the failed login counter, an attacker may be able to alternate between a valid login and attempts against a victim account.

Example pattern:

```text
valid-user:valid-password
victim-user:password1
valid-user:valid-password
victim-user:password2
```
## Username Enumeration via Account Lock

Account lock mechanisms are designed to protect accounts from brute-force attacks. However, they can create username enumeration vulnerabilities if they behave differently for valid and invalid usernames.

If repeated failed login attempts against a valid username trigger a message such as an account lock warning, but invalid usernames do not trigger the same behavior, attackers can identify valid usernames.

### Lab 05: Username Enumeration via Account Lock

In this lab, I tested candidate usernames by repeating failed login attempts.

The valid username triggered a different response related to too many incorrect login attempts. I used Burp Intruder and Grep-Match to identify the response containing the account lock message.

After identifying the valid username, I tested the password list and identified the correct password by finding the response that differed from the normal invalid or locked responses.

### Key Takeaway
Security controls such as account lockout must be implemented carefully. If account lock behavior reveals whether a username exists, it can create a username enumeration vulnerability.

## 2FA Simple Bypass

Two-factor authentication adds an additional verification step after username and password authentication.

However, 2FA must be enforced on the server side. If the application only redirects users to a 2FA page but does not restrict access to protected pages, the 2FA step may be bypassed.

### Lab 06: 2FA Simple Bypass

In this lab, I first logged in to my own account and completed the 2FA process to observe the normal account page URL.

After logging in with the victim user's valid username and password, the application prompted for a 2FA code. Instead of entering the code, I manually navigated to the account page URL.

The page loaded successfully, showing that the application did not properly verify whether the 2FA step had been completed.

### Key Takeaway
2FA must be enforced before granting access to protected resources. Applications should not rely only on the browser flow or redirects to protect sensitive pages.

