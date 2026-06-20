# Lab 11: Insecure Direct Object References

## Platform

PortSwigger Web Security Academy

## Difficulty

Apprentice

## Topic

Access Control / IDOR / Information Disclosure

## Lab Status

Solved

## Objective

The goal of this lab was to obtain Carlos's password from chat transcript files and use it to access his account.

## Simple Explanation

The application stored customer support chat transcripts as text files on the server.

Each transcript was assigned a predictable file name using incrementing numbers.

By accessing a transcript and modifying the file number, it was possible to retrieve transcripts belonging to other users and discover sensitive information.

## Vulnerability Description

The application exposed internal files through predictable URLs without verifying whether the current user was authorized to access them.

Because transcript files were stored using sequential identifiers, attackers could access other users' files simply by changing the file name.

This is a classic Insecure Direct Object Reference (IDOR) vulnerability.

## Key Concept

Users should only be able to access files that belong to them.

Applications must verify authorization before providing access to stored resources, even when file names appear internal or hidden.

## Steps Taken

1. Opened the Live Chat functionality.
2. Sent a test message.
3. Accessed the generated transcript.
4. Identified that transcripts were stored as text files.
5. Observed that file names followed a predictable numbering pattern.
6. Modified the transcript file identifier.
7. Accessed another user's transcript.
8. Discovered Carlos's password within the transcript contents.
9. Logged in using Carlos's credentials.
10. Successfully solved the lab.

## Result

Carlos's password was obtained from an unauthorized transcript file and used to access his account.

## What I Learned

* Predictable resource identifiers can expose sensitive information.
* Files stored on the server require authorization checks.
* IDOR vulnerabilities are not limited to user profiles or APIs.
* Sensitive information should never be exposed through publicly accessible files.
* Sequential file names increase the risk of unauthorized access.

## Security Impact

In a real-world application, attackers could access private conversations, personal information, credentials, support tickets, financial records, or other sensitive files belonging to other users.

## Mitigation

To prevent this vulnerability:

* Enforce authorization checks for all file access requests.
* Avoid exposing internal file paths and identifiers.
* Use unpredictable identifiers for resources.
* Store sensitive information securely.
* Follow the principle of least privilege.

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Web Browser
