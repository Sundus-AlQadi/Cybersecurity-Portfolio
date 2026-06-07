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

## Lab 01: Username Enumeration via Different Responses

In this lab, I used Burp Intruder to test a list of candidate usernames.

Most invalid usernames returned the same response. One username returned a different response indicating that the password was incorrect, which showed that the username was valid.

After identifying the valid username, I used the provided password list to test the password parameter. The successful login attempt returned a `302` response.

## Key Takeaway

Authentication systems should avoid giving different responses for invalid usernames and incorrect passwords. Generic error messages, rate limiting, account lockout, monitoring, and multi-factor authentication can reduce the risk.
