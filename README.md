# 🌐 CIP-B103 Lab 8: DNS Spoofing Forensics

[![LinkedIn](https://img.shields.io/badge/LINKEDIN-CONNECT-0A66C2?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/muzammil-sethar/) [![Email](https://img.shields.io/badge/EMAIL-CONTACT_ME-D14836?style=for-the-badge&logo=gmail)](mailto:Muzammilsethar@gmail.com)
[![Status](https://img.shields.io/badge/STATUS-OPEN_FOR_OPPORTUNITIES_|_CTFS_|_BUG_BOUNTIES_|_COLLABORATIONS-44CC11?style=for-the-badge)]()

**Author:** Mohammad Muzamil
**Registration No:** C11/26/DFIT/17289
**Role:** Digital Forensics Internship Trainee (DFIT) | ICDFA
**Platform:** Kali Linux (Rolling Release) | Wireshark | TShark | Scapy | Apache2
**Availability:** Open for Opportunities, CTFs, Bug Bounties, Collaborations & Security Projects

---

## Overview
This repository contains the official lab documentation, network captures (`pcapng`), and forensic packet analysis for **CIP-B103 Lab 8: DNS Spoofing Forensics**, executed as part of the Digital Forensics Internship Trainee (DFIT) curriculum at the **International Cybersecurity and Digital Forensics Academy (ICDFA)**.

---

## Lab Objectives
1. Capture clean baseline DNS resolution traffic for target domain `portal.icdfa.test`.
2. Configure a local web server landing page on `192.168.238.137`.
3. Execute controlled DNS spoofing simulation using Python (`scapy`).
4. Perform forensic packet dissection using TShark and Wireshark to identify attack indicators.
5. Restore workstation and network configurations to clean baseline state.

---

## Evidence Summary & Cryptographic Hashes

| Evidence File | File Description | SHA-256 Hash |
| :--- | :--- | :--- |
| `dns_baseline.pcapng` | Unmanipulated Baseline Traffic | `995efead9f514d1cd89ca53e26bd6c8a58af72cea9c375a3ef6b5d4b297baf19` |
| `index.html` | Authorized Training Landing Page | `94067b44c80557b5a7383dcec000a42e2ec56ce92713606cbc3cca7cf82221f0` |
| `dns_spoof_controlled.pcapng` | Controlled Attack Network Capture | `3a5ed81e820c01876c242a12c834087c02a2d5fbf558a7d67c10079982bcbced` |

---

## Key Forensic Findings

| Indicator | Baseline State | Controlled Spoof State | Forensic Assessment |
| :--- | :--- | :--- | :--- |
| **Responder IP** | `192.168.238.2` (Gateway) | `192.168.238.137` (Analyst VM) | Forged response from unauthorized host. |
| **Responder MAC** | Legitimate Gateway MAC | Analyst Workstation MAC | Layer-2 MAC Address Mismatch. |
| **A-Record** | Authorized Portal Server IP | `192.168.238.137` | Redirection to local warning page. |
| **TTL Value** | Dynamic Standard TTL | Static (300 Seconds) | Injected response shows static TTL. |
| **Response Behavior** | Single Valid Reply | Duplicated / Race Condition | Forged packet arrived before real server. |

---

## Defensive Recommendations
- **DNSSEC Implementation:** Enforce cryptographic signature validation for DNS records.
- **Layer-2 Security:** Deploy Dynamic ARP Inspection (DAI) and DHCP Snooping on switches.
- **TLS/HTTPS Enforcement:** Strict certificate checks to prevent unauthorized web page redirection.
- **NIDS / SIEM Detection:** Alert on duplicate DNS responses with identical Query IDs.
