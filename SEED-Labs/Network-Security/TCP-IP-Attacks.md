# SEED Lab: TCP/IP Attacks

## Category
Network Security

## Environment
Controlled academic lab environment using SEED Ubuntu VM and Docker containers.

## Objective
The goal of this lab was to understand TCP/IP protocol weaknesses and common attacks against TCP-based communication.

## Concepts Practiced
- TCP protocol behavior
- SYN flooding concept
- TCP reset attack concept
- TCP session hijacking concept
- Network service behavior
- Defensive mechanisms such as SYN cookies

## Tools Used
- SEED Ubuntu VM
- Docker / Docker Compose
- Linux Terminal
- Python
- Scapy
- Wireshark
- Netstat

## Lab Summary
In this lab, I explored selected TCP/IP attacks in a controlled environment. The lab helped me understand how protocol behavior can be abused and why defensive mechanisms are needed.

## What I Learned
- TCP connection setup can be targeted by SYN flooding.
- TCP sessions can be disrupted or manipulated if not properly protected.
- Network monitoring tools help observe connection behavior.
- Protocol security should be considered during system design.

## Security Relevance
TCP/IP attacks are important because many real-world services depend on TCP communication. Understanding these attacks helps explain the need for secure configurations and monitoring.

## Mitigation / Defense
- Enable SYN cookies.
- Monitor abnormal connection attempts.
- Use firewalls and rate limiting.
- Disable unnecessary services.
- Use encrypted and authenticated protocols.

## Note
This lab was performed only in a controlled academic environment.
