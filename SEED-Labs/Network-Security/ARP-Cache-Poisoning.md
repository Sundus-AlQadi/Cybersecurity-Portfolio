# SEED Lab: ARP Cache Poisoning

## Category
Network Security

## Environment
Controlled academic lab environment using SEED Ubuntu VM and Docker containers.

## Objective
The goal of this lab was to understand how ARP cache poisoning works and how it can lead to man-in-the-middle attacks inside a local network.

## Concepts Practiced
- ARP protocol
- ARP cache poisoning
- MAC address and IP address mapping
- Man-in-the-middle attack concept
- Packet spoofing using Scapy

## Tools Used
- SEED Ubuntu VM
- Docker / Docker Compose
- Linux Terminal
- Scapy
- Python
- Wireshark

## Lab Summary
In this lab, I studied how ARP is used to map IP addresses to MAC addresses. I practiced how forged ARP messages can affect a victim's ARP cache in a controlled environment.

## What I Learned
- ARP does not include strong authentication.
- Forged ARP messages can modify ARP cache entries.
- ARP cache poisoning can redirect traffic through another machine.
- This type of attack can support man-in-the-middle scenarios.

## Security Relevance
ARP cache poisoning is important because it shows how local network communication can be manipulated if devices trust unauthenticated ARP messages.

## Mitigation / Defense
- Use static ARP entries for critical systems.
- Enable dynamic ARP inspection where supported.
- Use network segmentation.
- Monitor unusual ARP traffic.
- Use encrypted protocols to protect sensitive communication.

## Note
This lab was performed only in a controlled academic environment.
