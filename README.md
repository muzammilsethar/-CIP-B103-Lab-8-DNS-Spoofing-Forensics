# 🌐 CIP-B103 Lab 8: DNS Spoofing Forensics

<p align="center">
  <a href="https://www.linkedin.com/in/muzammil-sethar/">
    <img src="https://img.shields.io/badge/LINKEDIN-CONNECT-0A66C2?style=for-the-badge&logo=linkedin" alt="LinkedIn">
  </a>
  <a href="mailto:Muzammilsethar@gmail.com">
    <img src="https://img.shields.io/badge/EMAIL-CONTACT_ME-D14836?style=for-the-badge&logo=gmail" alt="Email">
  </a>
  <img src="https://img.shields.io/badge/STATUS-OPEN_FOR_OPPORTUNITIES_|_CTFS_|_BUG_BOUNTIES_|_COLLABORATIONS-44CC11?style=for-the-badge" alt="Status">
</p>

---

**Author:** Mohammad Muzamil  
**Registration No:** C11/26/DFIT/17289  
**Role:** Digital Forensics Internship Trainee (DFIT) | ICDFA  
**Platform:** Kali Linux | Wireshark | TShark | Scapy | Apache2  
**Availability:** Open for Opportunities, CTFs, Bug Bounties & Security Projects  

---

## Overview
This repository contains the official lab documentation, network captures (`pcapng`), and forensic packet analysis for **CIP-B103 Lab 8: DNS Spoofing Forensics**, executed as part of the Digital Forensics Internship Trainee (DFIT) curriculum at the **International Cybersecurity and Digital Forensics Academy (ICDFA)**.

---

## Lab Execution & Forensic Evidence

### 1. Environment & Tools Verification
Configured system environment, set up the required dependencies, and prepared local web assets.

![Environment & Setup](b103%20lab8.1.png)

![Dependencies Installation](b103%20lab8.2.png)

---

### 2. Baseline Network Capture Analysis
Logged legitimate DNS requests to `portal.icdfa.test` and verified the original file integrity via SHA-256 digest.

![Baseline Capture Verification](b103%20lab8.3.png)

---

### 3. DNS Spoofing Script & Pre-Attack Checks
Checked current system IP forwarding, verified firewall configuration, and deployed the Scapy-based DNS injection script (`dns_spoof.py`).

![Pre-Attack Checks & Python Injection Code](b103%20lab8.4.png)

---

### 4. Controlled Interception & Packet Extraction
Captured modified network packets, dumped cryptographic hashes, and extracted network field structures using TShark.

![Controlled Network Capture](b103%20lab8.5.png)

![TShark Field Extraction](b103%20lab8.6.png)

---

### 5. System Restoration & Cleanup
Terminated background attack scripts, reset IP forwarding rules, flushed ARP dynamic tables, and verified clean system state.

![System Remediation & Cleanup](b103%20lab8.7.png)

---

## Cryptographic Hashes & Evidence Summary

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
