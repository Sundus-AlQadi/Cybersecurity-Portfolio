# SEED Lab: ICMP Redirect Attack

## Category
Network Security

## Environment
Controlled academic lab environment using SEED Ubuntu VM and Docker containers.

## Objective
The goal of this lab was to understand how ICMP redirect messages can affect routing decisions and how this can create man-in-the-middle risks.

## Concepts Practiced
- IP protocol
- ICMP protocol
- ICMP redirect messages
- Routing behavior
- Man-in-the-middle attack concept

## Tools Used
- SEED Ubuntu VM
- Docker / Docker Compose
- Linux Terminal
- Wireshark
- Scapy
- Python

## Lab Summary
In this lab, I explored how ICMP redirect messages can influence the route used by a victim machine. The lab demonstrated how routing changes can redirect traffic through another machine in a controlled environment.

## What I Learned
- ICMP redirect messages are used to inform hosts about better routes.
- If trusted incorrectly, ICMP redirects can change a victim's routing behavior.
- Routing manipulation can support man-in-the-middle scenarios.
- Network-layer security controls are important.

## Security Relevance
ICMP redirect attacks are relevant because they show how routing behavior can be abused when systems accept untrusted network messages.

## Mitigation / Defense
- Disable ICMP redirects when not needed.
- Use secure routing configurations.
- Monitor unexpected route changes.
- Apply network segmentation.
- Use encrypted protocols to protect data in transit.

## Note
This lab was performed only in a controlled academic environment.
