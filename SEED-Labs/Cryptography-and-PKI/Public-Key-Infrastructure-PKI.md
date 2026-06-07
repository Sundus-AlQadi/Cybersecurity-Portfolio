# SEED Lab: Public-Key Infrastructure (PKI)

## Category
Cryptography and PKI

## Environment
Controlled academic lab environment using SEED Ubuntu VM.

## Objective
The goal of this lab was to understand how Public-Key Infrastructure works and how certificates are used to secure web communication.

## Concepts Practiced
- Public-key cryptography
- Certificate Authority (CA)
- X.509 certificates
- Root CA trust
- HTTPS certificate deployment
- Man-in-the-middle protection

## Tools Used
- SEED Ubuntu VM
- Linux Terminal
- OpenSSL
- Apache Web Server
- Web Browser

## Lab Summary
In this lab, I practiced creating and using digital certificates in a controlled environment. I learned how a Certificate Authority can issue certificates and how certificates help establish trust for HTTPS communication.

## What I Learned
- PKI helps verify ownership of public keys.
- Certificate Authorities are part of the trust model.
- X.509 certificates are used in HTTPS.
- If trust in a CA is broken, secure communication can be affected.
- Certificates help reduce man-in-the-middle risks.

## Security Relevance
PKI is important because it supports secure communication on the web. It helps users verify that they are communicating with the intended server.

## Mitigation / Defense
- Use trusted Certificate Authorities.
- Protect private keys.
- Renew and manage certificates properly.
- Validate certificate chains.
- Avoid trusting unknown or compromised root certificates.

## Note
This lab was performed only in a controlled academic environment.
