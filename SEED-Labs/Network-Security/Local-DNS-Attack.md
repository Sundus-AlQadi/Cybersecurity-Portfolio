# SEED Lab: Local DNS Attack

## Category
Network Security

## Environment
Controlled academic lab environment using SEED Ubuntu VM and Docker containers.

## Objective
The goal of this lab was to understand how DNS resolution works and how local DNS attacks can manipulate hostname-to-IP address resolution.

## Concepts Practiced
- DNS resolution
- Local DNS server setup
- DNS spoofing
- DNS cache poisoning
- Packet sniffing and spoofing
- Scapy basics

## Tools Used
- SEED Ubuntu VM
- Docker / Docker Compose
- Linux Terminal
- Wireshark
- Scapy
- Python
- DNS server tools

## Lab Summary
In this lab, I practiced DNS-related attack concepts in a controlled environment. I learned how DNS responses can be manipulated and how DNS cache poisoning can misdirect users to unintended destinations.

## What I Learned
- DNS translates domain names into IP addresses.
- DNS resolution can be manipulated if responses are spoofed.
- DNS cache poisoning can affect where users are redirected.
- Securing DNS is important for protecting users from misdirection.

## Security Relevance
DNS attacks are important because users rely on DNS to reach legitimate websites and services. If DNS responses are manipulated, users may be redirected to malicious or unintended destinations.

## Mitigation / Defense
- Use DNSSEC where applicable.
- Secure DNS server configurations.
- Restrict unauthorized DNS responses.
- Monitor DNS anomalies.
- Flush or validate suspicious DNS cache entries.

## Note
This lab was performed only in a controlled academic environment.
