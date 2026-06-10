# Lab 04: Broken Brute-Force Protection - IP Block

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Topic
Authentication Vulnerabilities / Brute-Force Protection / Logic Flaws

## Lab Status
Solved

## Objective
The goal of this lab was to exploit a logic flaw in the application's brute-force protection mechanism, identify the victim user's password using a provided wordlist, and access the victim's account page in a controlled lab environment.

## Simple Explanation
The application blocked the client IP after three consecutive failed login attempts.

However, the protection mechanism had a logic flaw. Logging in successfully to a valid account reset the failed login counter.

To avoid triggering the IP block, I alternated between a successful login to my own account and a password attempt against the victim account.

The request order followed this pattern:

```text
wiener:peter
carlos:password1
wiener:peter
carlos:password2
wiener:peter
carlos:password3
