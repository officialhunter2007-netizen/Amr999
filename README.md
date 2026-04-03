# 🛡️ Advanced Cybersecurity Portfolio: Threat Detection & Systems Security 💻

[![Hack The Box](https://img.shields.io/badge/Hack%20The%20Box-Success-green?style=for-the-badge&logo=hackthebox)](https://www.hackthebox.com/)
[![Wireshark](https://img.shields.io/badge/Wireshark-Traffic%20Analysis-blue?style=for-the-badge&logo=wireshark)](https://www.wireshark.org/)
[![Windows Security](https://img.shields.io/badge/Windows-Security%20Hardening-0078D6?style=for-the-badge&logo=windows)](https://www.microsoft.com/en-us/security)

Welcome to my professional cybersecurity portfolio. This repository documents a series of high-fidelity technical projects focused on **Network Traffic Analysis**, **Threat Hunting**, and **Enterprise Systems Hardening**. Unlike standard documentation, this portfolio provides deep-dive forensic evidence and actionable security intelligence derived from real-world attack simulations.

---

## 🌟 What Differentiates Me?

In a landscape of automated tools, I focus on the **"Why"** and **"How"** of an attack. My approach is characterized by:

*   **Packet-Level Precision**: I don't just see alerts; I dissect the raw TCP/IP, 802.11, and DNS protocols to uncover stealthy indicators of compromise (IoCs).
*   **SOC-Ready Methodology**: Every analysis follows a structured forensic process—from initial scoping and pattern recognition to final threat classification and mitigation strategy.
*   **Hybrid Expertise**: I bridge the gap between **Offensive Reconnaissance** (understanding attacker TTPs) and **Defensive Hardening** (implementing least-privilege NTFS/Share permissions and service auditing).
*   **Visual Evidence**: Every claim is backed by forensic screenshots and PCAP analysis, ensuring transparency and technical validation.

---

## 🛠️ Core Technical Skill Set

| Category | Skills & Tools |
| :--- | :--- |
| **Network Forensics** | Wireshark, PCAP Analysis, Protocol Dissection (TCP, ICMP, DNS, TLS, 802.11) |
| **Threat Detection** | ARP Spoofing, MITM, DNS Tunneling, TLS Renegotiation DoS, HTTP Directory Enumeration |
| **Systems Security** | Windows Security Hardening, NTFS & Share Permissions, PowerShell Auditing, User/Group Management |
| **Reconnaissance** | Nmap Traffic Analysis, ARP Scanning Detection, Wireless Deauthentication Analysis |

---

## 🔬 Featured Projects & Visual Evidence

### 1. Network Intrusion & Protocol Abuse Detection
I specialize in identifying advanced threats that bypass traditional firewalls by abusing trusted protocols.

*   **DNS Tunneling Exfiltration**: Detected covert C2 channels by identifying anomalous `TXT` record queries and extracting embedded malicious payloads.
    > *“Smoking Gun” Evidence: Extracted `HTB{This is kind of malicious ;)}` from raw DNS traffic.*
    ![DNS Tunneling](Hack%20The%20Box/Exercises/Screenshot%202026-01-18%20091309.png)

*   **TLS Renegotiation DoS**: Identified application-layer resource exhaustion attacks by analyzing `Client Hello` flood patterns and server `Encrypted Alert` responses.
    ![TLS DoS](Hack%20The%20Box/Exercises/Screenshot%202026-01-17%20110148.png)

*   **ARP Spoofing (MITM)**: Validated Layer 2 attacks using duplicate IP-to-MAC mapping analysis and unsolicited ARP reply detection.
    ![ARP Spoofing](Hack%20The%20Box/Exercises/Screenshot%202026-01-08%20080535.png)

### 2. Enterprise Systems Hardening (Windows)
Beyond detection, I implement robust security controls to prevent unauthorized access and lateral movement.

*   **Secure File Share Configuration**: Implemented a strict least-privilege model for sensitive HR data, configuring complex NTFS and Share permission inheritance.
*   **Service Auditing via PowerShell**: Used advanced CLI queries to audit critical system services (e.g., `wuauserv`) and validate security configurations.
    ![Windows Hardening](Hack%20The%20Box/Windows%20Fundamentals%20Module/Images/Screenshot%202025-11-14%20045253.png)

---

## 📂 Repository Architecture

```bash
Amr999/
├── Hack The Box/
│   ├── Exercises/                      # Deep-dive PCAP Analysis & Threat Hunting
│   │   ├── Network traffic Analysis.md  # ARP, TCP, ICMP, Wi-Fi Attack Detection
│   │   └── ... (Forensic Screenshots)
│   ├── Password Attacks Module/        # Credential Security & Lab Writeups
│   └── Windows Fundamentals Module/    # Systems Hardening & Security Auditing
│       ├── Module Summary.md
│       └── windows fundamentals Lap.md
└── README.md                           # Professional Portfolio Overview
```

---

## 🤝 Let's Secure the Future

I am actively seeking opportunities to apply my analytical skills in a **SOC Analyst** or **Security Researcher** capacity. If you value technical depth, forensic precision, and a proactive security mindset, let's connect.

[![GitHub](https://img.shields.io/badge/GitHub-Profile-181717?style=for-the-badge&logo=github)](https://github.com/officialhunter2007-netizen)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/)
