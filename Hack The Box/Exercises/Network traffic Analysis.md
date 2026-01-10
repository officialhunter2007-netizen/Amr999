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
