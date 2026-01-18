## Project 1: ARP Spoofing Detection Using Wireshark

### Objective

Demonstrate my ability to detect and validate an ARP Spoofing (MITM) attack using targeted packet filtering and traffic analysis in Wireshark.

---

### Detection Method

-   Applied ARP-focused filters to isolate Layer 2 traffic.
-   Identified duplicate IP-to-MAC mappings for the same internal IP address.
-   Confirmed spoofing through unsolicited ARP replies and Wireshark’s "Duplicate IP Address" warning.
![s](<Screenshot 2026-01-08 080535.png>)
### Key Indicators Observed

-   Same IP address advertised by multiple MAC addresses.
-   Continuous ARP replies without corresponding requests.
-   Automatic Wireshark alert indicating ARP cache poisoning.
-   Resulting TCP/TLS session instability confirming MITM impact.

### Tools & Filters Used

-   **Tool:** Wireshark
-   **Filters:**
    -   `arp`
    -   `arp.opcode == 2`

### SOC-Relevant Skills Demonstrated

-   Network attack detection (ARP Spoofing / MITM).
-   Packet filtering and traffic isolation.
-   Security alert validation.
-   Rapid identification of malicious behavior at Layer 2.
-   Incident confirmation using multiple indicators.
# ********************************************************
## Project 2 : ARP Scan Detection Using Wireshark

### What I Observed

While analyzing the capture file `ARP_Scan.pcap`, I observed a large number of ARP requests sent sequentially for IP addresses within the same subnet. The requests followed a clear pattern:

> Who has 192.168.10.1?
> Who has 192.168.10.2?
> Who has 192.168.10.3?
> ...
> Who has 192.168.10.39?
![s](<Screenshot 2026-01-09 113622.png>)
The requests were sent in ascending IP order, which is not normal user behavior. Additionally, most of these ARP requests did not receive replies, indicating that the sender was probing the network rather than attempting legitimate communication.

### Why This Is Suspicious

-   Sequential IP enumeration is a known network discovery technique.
-   Legitimate ARP traffic is usually sporadic and reactive, not sequential.
-   The lack of ARP replies suggests host discovery, not real communication.
-   There was a high concentration of ARP “Who has” requests in a short time window.

This traffic pattern is consistent with an **ARP scanning / reconnaissance activity**.

### Filters Used

To isolate this behavior, I applied the main filter:
`arp`

And focused specifically on:
-   "Who has" ARP requests.
-   Broadcast destination addresses.
-   Rapid, sequential IP progression.

### Attack Classification

Based on the traffic pattern, I classified this activity as:
**ARP Scan / Network Reconnaissance (Pre-Attack Phase)**

This type of scan is commonly performed before:
-   ARP Spoofing
-   MITM attacks
-   Lateral movement
# ********************************************************
## Project 3: Detecting a Wi-Fi Deauthentication Attack (Layer 2)

### Overview

In this analysis, I investigated a wireless packet capture to identify a Layer 2 Wi-Fi deauthentication attack using Wireshark.

---

### Detection Process

I began by filtering 802.11 management frames related to a specific BSSID and narrowed the traffic to **Deauthentication frames**. This allowed me to focus only on activity relevant to Layer 2 wireless control traffic.

![s](<Screenshot 2026-01-10 184241.png>)

### Indicators of Attack

-   A high volume of deauthentication frames within a very short time window, which is abnormal in normal Wi-Fi operations.
-   Frames sent in both directions (AP → client and spoofed client → AP).
-   Repeated **Reason Code 7** ("Class 3 frame received from non-associated STA"), suggesting frame injection by a device not legitimately connected.
-   Rapid sequence number increments, consistent with automated attack tools rather than normal access point behavior.

### Conclusion

Based on the excessive deauthentication frames, abnormal reason codes, and sequence patterns, I concluded that this activity represents a **Layer 2 Wi-Fi deauthentication (DoS) attack** aimed at forcibly disconnecting clients.

> This analysis demonstrates my ability to detect wireless attacks at Layer 2, analyze 802.11 protocol behavior, and communicate findings clearly from a SOC analyst perspective.

# ********************************************************
## Project 4: Detection of TCP Port Scan via Packet Analysis

### Objective

Identify and confirm malicious reconnaissance activity (TCP port scanning) within network traffic using packet-level analysis.

---

### Tools & Technologies

-   **Wireshark:** Packet capture and deep traffic inspection.
-   **PCAP Analysis:** Offline forensic investigation.
-   **TCP/IP Protocol Analysis**
-   **Display Filters:** Specifically, `tcp.flags.reset == 1`.
![s](<Screenshot 2026-01-12 073417.png>)
### Detection Steps

1.  Loaded the PCAP file (`nmap_frag_fw_bypass.pcapng`) into Wireshark for analysis.
2.  Applied a targeted display filter to isolate TCP Reset (RST) packets, which are commonly generated in response to unsolicited connection attempts:
    `tcp.flags.reset == 1`
3.  Observed a high volume of `RST/ACK` responses originating from the same source IP and targeting multiple destination ports in rapid succession.
4.  Correlated source and destination behavior:
    -   One internal host attempted connections to many different TCP ports.
    -   The target host responded with consistent `RST, ACK` flags.
    -   The traffic pattern matched known Nmap TCP scan behavior.
5.  Validated attack characteristics:
    -   Sequential and non-sequential port targeting.
    -   Short time intervals between packets.
    -   No successful TCP three-way handshake completion.

### Analysis & Conclusion

> Based on the traffic pattern, TCP flags, and port distribution, I confidently identified this activity as a **TCP port scan reconnaissance attack**. The behavior is consistent with Nmap-based scanning techniques, often used as a precursor to exploitation.
# ********************************************************

## Project 5: ICMP Smurf Attack Detection – PCAP Analysis (Wireshark)

### Attack Type Identified

ICMP Smurf Denial-of-Service Attack

### Objective

Analyze captured network traffic to identify abnormal ICMP behavior indicative of a denial-of-service attack and demonstrate practical SOC analyst detection and traffic-analysis skills using Wireshark.

---

### Step-by-Step Detection Process (My Methodology)

![s](<Screenshot 2026-01-12 185914.png>)

1.  **Initial Traffic Scoping**
    I began by applying an `icmp` display filter to isolate ping-related traffic. This immediately revealed an abnormally high volume of ICMP Echo (ping) requests within a very short time frame.

2.  **Identification of Abnormal ICMP Patterns**
    While reviewing the packet list, I observed:
    -   A single source host (`192.168.10.5`).
    -   Repeatedly sending ICMP Echo Requests.
    -   Targeting the same destination host (`192.168.10.1`).
    -   Rapid sequence number increments with minimal time delta between packets.
    > This behavior significantly deviates from normal ICMP usage, which is typically sparse and diagnostic in nature.

3.  **Payload and Packet Size Analysis**
    Upon inspecting individual packets, I identified:
    -   Extremely large ICMP payloads (~25,000 bytes).
    -   Continuous transmission without pauses.
    -   Reassembled IPv4 payloads indicating fragmentation handling.
    > This confirms an attempt to consume bandwidth and processing resources, a classic DoS indicator.

4.  **Request–Reply Imbalance Observation**
    I also noticed:
    -   Numerous ICMP Echo Requests.
    -   Missing or delayed Echo Replies.
    -   "No response found" indicators in Wireshark.
    > This imbalance suggests that the destination host was being overwhelmed and could not respond to all incoming requests.

### Attack Classification Reasoning

Based on the analysis, I classified this traffic as an **ICMP Smurf-style attack** due to:

-   High-volume ICMP Echo Requests.
-   Repetitive and automated packet structure.
-   Large payload sizes.
-   Resource exhaustion characteristics.
-   Intent to deny service rather than perform reconnaissance.

> While classic Smurf attacks rely on broadcast amplification, this capture demonstrates the core Smurf DoS behavior: abusing ICMP to overwhelm a target.

# ***************************************

# Project 6: HTTP Directory Enumeration Attack Analysis

In this project, I analyzed a PCAP file in Wireshark to investigate suspicious web traffic. I successfully identified and documented an HTTP directory and file enumeration attack, a common reconnaissance technique used by attackers to map a web application's attack surface.

---
![s](<Screenshot 2026-01-15 075251.png>)

---
### Key Findings & Actions

-   **Detection:** Identified a high volume of sequential HTTP `GET` requests for sensitive files (e.g., `/.bash_history`, `/git/HEAD`) from a single source IP (`192.168.10.5`).
-   **Analysis:** Correlated the requests with the server's **HTTP 401 Unauthorized** responses, confirming the attacker was actively probing for existing but protected resources.
-   **Classification:** The activity was classified as a **Directory & File Enumeration Attack (MITRE ATT&CK T1595)**, consistent with tools like Gobuster or DirBuster.

### SOC Skills Demonstrated

-   **Traffic Analysis:** Filtered and analyzed HTTP traffic to isolate malicious patterns.
-   **Threat Identification:** Recognized the attack signature of automated reconnaissance tools.
-   **Incident Documentation:** Correlated packet-level evidence to classify the threat and assess its potential impact.

> This analysis showcases my ability to dissect network traffic, identify attacker TTPs, and translate findings into actionable security intelligence.

# ***************************************

# Project 7: TLS Renegotiation DoS Attack Analysis

In this project, I analyzed a PCAP file in Wireshark and identified an application-layer Denial of Service (DoS) attack. The attacker utilized a TLS Renegotiation technique to exhaust the server's CPU resources.

---

![s](<Screenshot 2026-01-17 110148.png>)


### Key Findings

-   **Attack Signature:** I detected a high-volume flood of `Client Hello` messages sent from the attacker (`192.168.10.56`) to the victim (`192.168.10.23`) after an initial TLS session was already established.
-   **Malicious Intent:** This pattern is characteristic of a **TLS Renegotiation Attack**, where each `Client Hello` forces the server to perform expensive cryptographic calculations, leading to CPU exhaustion.
-   **Evidence:** The server responded with `Encrypted Alert` messages, indicating it was overwhelmed and terminating sessions under the strain of the attack.

### Threat Classification

-   **Attack:** Application Layer Denial of Service (DoS)
-   **Technique:** TLS Renegotiation Flood (MITRE ATT&CK T1499.002)

### SOC Skills Demonstrated

-   **Protocol Analysis:** Dissected TLS handshake traffic to identify anomalous behavior.
-   **Pattern Recognition:** Identified the specific signature of a resource exhaustion attack.
-   **Incident Triage:** Quickly classified the threat and determined its intent and impact.

> This analysis showcases my ability to detect stealthy, low-bandwidth DoS attacks by understanding the intricacies of network protocols.
# ***************************************

# Project: DNS Tunneling Data Exfiltration Analysis

In this project, I analyzed a PCAP file in Wireshark and uncovered a DNS Tunneling attack. This technique allows attackers to bypass firewalls and exfiltrate data by hiding it within DNS traffic.

---

![s](<Screenshot 2026-01-18 091309.png>)


### Key Findings

-   **Anomaly Detected:** I identified a high volume of suspicious DNS queries for `TXT` records originating from a single host (`192.168.10.5`). This is not typical user behavior.
-   **"Smoking Gun" Evidence:** By inspecting the DNS responses, I discovered a `TXT` record containing an embedded string: `HTB{This is kind of malicious ;)}`.
-   **Conclusion:** This proves that the DNS protocol was being abused as a covert channel for data exfiltration or command-and-control (C2), rather than for legitimate name resolution.

### Threat Classification

-   **Attack:** Covert Data Exfiltration / C2
-   **Technique:** DNS Tunneling (MITRE ATT&CK T1071.004)

### SOC Skills Demonstrated

-   **Anomaly Detection:** Identified non-standard use of the DNS protocol.
-   **Packet Analysis:** Extracted and interpreted malicious payloads from DNS records.
-   **Threat Hunting:** Proactively investigated suspicious patterns to uncover a hidden threat.

> This analysis demonstrates my ability to detect advanced threats that abuse trusted protocols to evade traditional security controls.
