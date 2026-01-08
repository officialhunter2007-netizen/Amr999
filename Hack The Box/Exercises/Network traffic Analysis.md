## Project: ARP Spoofing Detection Using Wireshark

### Objective

Demonstrate my ability to detect and validate an ARP Spoofing (MITM) attack using targeted packet filtering and traffic analysis in Wireshark.

---

### Detection Method

-   Applied ARP-focused filters to isolate Layer 2 traffic.
-   Identified duplicate IP-to-MAC mappings for the same internal IP address.
-   Confirmed spoofing through unsolicited ARP replies and Wireshark’s "Duplicate IP Address" warning.
![s](<>)
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

