## The Lab Environment

The lab is a small Active Directory network with the following machines:

-   **Two Domain Controllers:** `DC1` and `DC2`
-   **A File Server:** `Server01`
-   **A Certificate Authority Server:** `PKI`
-   **A Windows 10 Workstation:** `WS001` (This is the machine I start on as the compromised user `bob`).
-   **A Kali Linux Machine:** my attacker machine.

The network is segmented. I can only directly access the Windows 10 machine (`WS001`). To reach the other servers (like the DCs), I must first compromise `WS001` and then pivot my traffic through it.

---

## Exercise One: Kerberoasting Simulation

In this exercise, I simulated a full attack and detection cycle within the provided Active Directory environment.

### Offensive Phase: Kerberoasting Attack

First, I took on the role of the attacker.

1.  **Initial Access:** I established a foothold by using `xfreerdp` to RDP into the Windows 10 machine (`WS001`) as the user `bob` with the provided credentials.
2.  **Exploitation:** From my position inside the network, I executed `Rubeus` to perform a Kerberoasting attack. This automatically discovered all service accounts in the domain and extracted their Kerberos service ticket hashes, which I saved to a file named `spn.txt`.
3.  **Offline Cracking:** I then exfiltrated the `spn.txt` file. On my own attack machine, I used `Hashcat` with the `rockyou.txt` wordlist to crack the hashes. I successfully recovered the password for the `svc-iam` account, which was `maiiposs`.

### Defensive Phase: Detection and Analysis

Next, I switched to a defensive mindset to detect my own attack.

1.  **Event Log Analysis:** I connected to the Domain Controller (`DC1`) and opened the Windows Event Viewer, focusing on the Security log.
2.  **Identifying the Anomaly:** I filtered for **Event ID 4769** (Kerberos Service Ticket Requested). I immediately noticed a large burst of these events all originating from a single source (`172.16.18.25`) in a very short time frame.
3.  **Confirming the Attack:** By examining the details of these events, I confirmed that the user `bob` was requesting tickets for multiple, unrelated services back-to-back. This anomalous pattern is a *clear and reliable indicator* of a Kerberoasting scan in progress, allowing me to successfully identify the malicious activity.
