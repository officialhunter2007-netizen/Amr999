# 🛡️ Advanced Cybersecurity Portfolio: Threat Detection & Systems Security 💻

[![Hack The Box](https://img.shields.io/badge/Hack%20The%20Box-Success-green?style=for-the-badge&logo=hackthebox)](https://www.hackthebox.com/)
[![Wireshark](https://img.shields.io/badge/Wireshark-Traffic%20Analysis-blue?style=for-the-badge&logo=wireshark)](https://www.wireshark.org/)
[![Windows Security](https://img.shields.io/badge/Windows-Security%20Hardening-0078D6?style=for-the-badge&logo=windows)](https://www.microsoft.com/en-us/security)

Welcome to my professional cybersecurity portfolio. This repository is an exhaustive collection of my technical journey, documenting every lab, exercise, and project I have completed. It showcases my expertise in **Network Forensics**, **Active Directory Security**, **Password Attacks**, and **Enterprise Systems Hardening**.

---

## 🌟 What Differentiates Me?

In a landscape of automated tools, I focus on the **"Why"** and **"How"** of an attack. My approach is characterized by:

*   **Packet-Level Precision**: I dissect raw TCP/IP, 802.11, and DNS protocols to uncover stealthy indicators of compromise (IoCs).
*   **Full Attack-and-Detect Cycles**: I simulate both offensive phases (exploitation) and defensive phases (forensic analysis and detection), providing a 360-degree view of security.
*   **Enterprise-Grade Hardening**: I implement robust security controls, from Active Directory ACLs to least-privilege NTFS permissions.
*   **Forensic Transparency**: Every project is backed by raw evidence, including PCAP analysis, event log correlation, and technical screenshots.

---

## 🛠️ Core Technical Skill Set

| Category | Skills & Tools |
| :--- | :--- |
| **Network Forensics** | Wireshark, PCAP Analysis, Protocol Dissection (TCP, ICMP, DNS, TLS, 802.11) |
| **Active Directory Security** | Kerberoasting, DCSync, Golden Ticket, GPP Password Attacks, ACL Exploitation, BloodHound, Rubeus, Mimikatz |
| **Threat Detection** | Event Log Analysis (IDs 4769, 4663, 4624, 4738), Honeypot Strategies, NTLM Relay Detection |
| **Password Attacks** | Brute-forcing (Hydra, NetExec), Offline Cracking (Hashcat), Credential Discovery in Shares/AD Attributes |
| **Systems Security** | Windows Hardening, NTFS & Share Permissions, PowerShell Auditing, User/Group Management |

---

## 🔬 Exhaustive Project & Lab List

Below is a complete list of every project and lab documented in this repository, categorized by module.

### 1. Network Traffic Analysis & Intrusion Detection
*Detailed forensic investigations into network-layer attacks using Wireshark.*

*   **[Project 1: ARP Spoofing Detection](Hack%20The%20Box/Exercises/Network%20traffic%20Analysis.md#project-1-arp-spoofing-detection-using-wireshark)**: Detecting MITM attacks via duplicate IP-to-MAC mapping.
*   **[Project 2: ARP Scan Detection](Hack%20The%20Box/Exercises/Network%20traffic%20Analysis.md#project-2-arp-scan-detection-using-wireshark)**: Identifying network reconnaissance through sequential IP enumeration.
*   **[Project 3: Wi-Fi Deauthentication Attack](Hack%20The%20Box/Exercises/Network%20traffic%20Analysis.md#project-3-detecting-a-wi-fi-deauthentication-attack-layer-2)**: Analyzing 802.11 management frames to identify Layer 2 DoS.
*   **[Project 4: TCP Port Scan Detection](Hack%20The%20Box/Exercises/Network%20traffic%20Analysis.md#project-4-detection-of-tcp-port-scan-via-packet-analysis)**: Confirming reconnaissance activity through TCP Reset (RST) flag analysis.
*   **[Project 5: ICMP Smurf Attack Detection](Hack%20The%20Box/Exercises/Network%20traffic%20Analysis.md#project-5-icmp-smurf-attack-detection--pcap-analysis-wireshark)**: Identifying bandwidth-exhaustion DoS via ICMP payload analysis.
*   **[Project 6: HTTP Directory Enumeration](Hack%20The%20Box/Exercises/Network%20traffic%20Analysis.md#project-6-http-directory-enumeration-attack-analysis)**: Detecting automated web reconnaissance (Gobuster/DirBuster) in HTTP traffic.
*   **[Project 7: TLS Renegotiation DoS](Hack%20The%20Box/Exercises/Network%20traffic%20Analysis.md#project-7-tls-renegotiation-dos-attack-analysis)**: Identifying CPU-exhaustion attacks in encrypted traffic.
*   **[Project 8: DNS Tunneling Exfiltration](Hack%20The%20Box/Exercises/Network%20traffic%20Analysis.md#project-dns-tunneling-data-exfiltration-analysis)**: Uncovering covert C2 channels and data exfiltration within DNS `TXT` records.

### 2. Active Directory Offensive & Defensive Simulations
*Full-cycle simulations of advanced AD attacks and their corresponding detection methods.*

*   **[Exercise 1: Kerberoasting Simulation](Hack%20The%20Box/Exercises/Offensive%20defensive%20exercises.md#exercise-one-kerberoasting-simulation)**: Attacking service accounts with Rubeus and detecting via Event ID 4769.
*   **[Exercise 2: GPP Password Attack](Hack%20The%20Box/Exercises/Offensive%20defensive%20exercises.md#exercise-two-gpp-password-attack-simulation)**: Extracting credentials from SYSVOL and detecting via Object Access auditing (Event ID 4663).
*   **[Exercise 3: Exposed Credentials in Shares](Hack%20The%20Box/Exercises/Offensive%20defensive%20exercises.md#exercise-three-exposed-credentials-in-network-share)**: Discovering hardcoded passwords in scripts and detecting lateral movement (Event ID 4624).
*   **[Exercise 4: AD Enumeration & Attributes](Hack%20The%20Box/Exercises/Offensive%20defensive%20exercises.md#exercise-four-ad-enumeration-and-credential-discovery)**: Finding passwords in AD user attributes and monitoring for suspicious TGT requests.
*   **[Exercise 5: DCSync Attack Simulation](Hack%20The%20Box/Exercises/Offensive%20defensive%20exercises.md#exercise-five-dcsync-attack-simulation)**: Performing unauthorized domain replication with Mimikatz and detecting via Event ID 4662.
*   **[Exercise 6: Golden Ticket Attack](Hack%20The%20Box/Exercises/Offensive%20defensive%20exercises.md#exercise-six-golden-ticket-attack-simulation)**: Forging persistence and detecting elevated token logons and abnormal Kerberos traffic.
*   **[Exercise 7: Print Spooler (PrinterBug) Relay](Hack%20The%20Box/Exercises/Offensive%20defensive%20exercises.md#exercise-seven-print-spooler-attack--detection-summary)**: Relaying NTLM credentials for domain compromise and detecting via source IP mismatches.
*   **[Exercise 8: ACL Attack & Honeypot Defense](Hack%20The%20Box/Exercises/Offensive%20defensive%20exercises.md#exercise-eight-acl-attack--defense)**: Exploiting `GenericAll` permissions and implementing a honeypot account for instant detection.

### 3. Password Attacks & Network Services
*Practical labs focused on credential harvesting and service exploitation.*

*   **[Project 1: WinRM Brute-Force & Flag Recovery](Hack%20The%20Box/Password%20Attacks%20Module/Network%20Services%20Laps.md#project-one)**: Using `netexec` and `evil-winrm` to compromise Windows Remote Management.
*   **[Project 2: SSH Brute-Force & Flag Recovery](Hack%20The%20Box/Password%20Attacks%20Module/Network%20Services%20Laps.md#project-two)**: Using `hydra` to crack SSH credentials and gain shell access.

### 4. Windows Systems Hardening
*Foundational security configuration and auditing.*

*   **[Secure File Share Configuration](Hack%20The%20Box/Windows%20Fundamentals%20Module/windows%20fundamentals%20Lap.md)**: Implementing least-privilege NTFS and Share permissions for enterprise data.
*   **[Service Auditing via PowerShell](Hack%20The%20Box/Windows%20Fundamentals%20Module/windows%20fundamentals%20Lap.md#7-audit-service)**: Using CLI tools to validate system service security states.

---

## 📂 Repository Architecture

```bash
Amr999/
├── Hack The Box/
│   ├── Exercises/                      # Network Forensics & AD Simulations
│   │   ├── [Network traffic Analysis.md](Hack%20The%20Box/Exercises/Network%20traffic%20Analysis.md)
│   │   └── [Offensive defensive exercises.md](Hack%20The%20Box/Exercises/Offensive%20defensive%20exercises.md)
│   ├── Password Attacks Module/        # Credential Harvesting Labs
│   │   └── [Network Services Laps.md](Hack%20The%20Box/Password%20Attacks%20Module/Network%20Services%20Laps.md)
│   └── Windows Fundamentals Module/    # Systems Hardening & Auditing
│       ├── [Module Summary.md](Hack%20The%20Box/Windows%20Fundamentals%20Module/Module%20Summary.md)
│       └── [windows fundamentals Lap.md](Hack%20The%20Box/Windows%20Fundamentals%20Module/windows%20fundamentals%20Lap.md)
└── README.md                           # Exhaustive Portfolio Overview
```

---

## 🤝 Let's Secure the Future

I am actively seeking opportunities to apply my analytical skills in a **SOC Analyst** or **Security Researcher** capacity. If you value technical depth, forensic precision, and a proactive security mindset, let's connect.

[![GitHub](https://img.shields.io/badge/GitHub-Profile-181717?style=for-the-badge&logo=github)](https://github.com/officialhunter2007-netizen)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/)
