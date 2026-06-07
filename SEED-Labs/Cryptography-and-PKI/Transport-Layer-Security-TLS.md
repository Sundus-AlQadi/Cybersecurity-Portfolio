# SEED Lab: Transport Layer Security (TLS)

## Category
Cryptography and PKI

## Environment
Controlled academic lab environment using SEED Ubuntu VM and Docker containers.

## Objective
The goal of this lab was to understand how TLS protects communication and how certificates are used in HTTPS.

## Concepts Practiced
- Transport Layer Security (TLS)
- HTTPS
- TLS client and server communication
- X.509 certificates
- Subject Alternative Name (SAN)
- HTTPS proxy concept
- Man-in-the-middle risks

## Tools Used
- SEED Ubuntu VM
- Docker / Docker Compose
- Linux Terminal
- OpenSSL
- Python
- Web Browser
- Wireshark

## Lab Summary
In this lab, I studied how TLS is used to protect data transmitted over a network. I practiced concepts related to certificates, HTTPS communication, and how trust affects secure connections.

## What I Learned
- TLS provides confidentiality and integrity for communication.
- HTTPS is built on top of TLS.
- Certificates help verify server identity.
- Trusted Certificate Authorities play an important role in secure communication.
- Compromised trust can create security risks.

## Security Relevance
TLS is essential for protecting sensitive data in transit. Understanding TLS helps explain how secure web communication works and why certificate validation is important.

## Mitigation / Defense
- Use valid certificates from trusted CAs.
- Enforce HTTPS.
- Disable outdated TLS versions.
- Configure certificates correctly.
- Monitor for certificate misconfiguration.

## Note
This lab was performed only in a controlled academic environment.
