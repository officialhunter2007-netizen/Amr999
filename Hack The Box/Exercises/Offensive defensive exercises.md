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
![s](<Screenshot 2025-11-30 112756.png>)
2.  **Exploitation:** From my position inside the network, I executed `Rubeus` to perform a Kerberoasting attack. This automatically discovered all service accounts in the domain and extracted their Kerberos service ticket hashes, which I saved to a file named `spn.txt`.
![s](<Screenshot 2025-11-30 123402.png>)
3.  **Offline Cracking:** I then exfiltrated the `spn.txt` file. On my own attack machine, I used `Hashcat` with the `rockyou.txt` wordlist to crack the hashes. I successfully recovered the password for the `svc-iam` account, which was `mariposa`.
![s](<Screenshot 2025-11-30 123053.png>)
![s](<Screenshot 2025-11-30 124028.png>)
### Defensive Phase: Detection and Analysis

Next, I switched to a defensive mindset to detect my own attack.

1.  **Event Log Analysis:** I connected to the Domain Controller (`DC1`) and opened the Windows Event Viewer, focusing on the Security log.
2.  **Identifying the Anomaly:** I filtered for **Event ID 4769** (Kerberos Service Ticket Requested). I immediately noticed a large burst of these events all originating from a single source (`172.16.18.25`) in a very short time frame.
![s](<Screenshot 2025-11-30 140137.png>)
3.  **Confirming the Attack:** By examining the details of these events, I confirmed that the user `bob` was requesting tickets for multiple, unrelated services back-to-back. This anomalous pattern is a *clear and reliable indicator* of a Kerberoasting scan in progress, allowing me to successfully identify the malicious activity.
# *****************************************************************
## Exercise Two: GPP Password Attack Simulation

### Offensive Phase: GPP Password Extraction

First, as an attacker, I simulated a GPP Password attack.

1.  **Initial Access:** I started by using `xfreerdp` to RDP into the network as the user `bob`.
![s](<Screenshot 2025-12-02 175219.png>)
2.  **Exploitation:** Once on the machine, I navigated to the `Downloads` folder and opened PowerShell. I bypassed the execution policy with the command `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` and then imported the `Get-GPPPassword.ps1` script.
3.  **Credential Extraction:** I ran the `Get-GPPPassword` function. The script automatically searched the SYSVOL share, found a `Groups.xml` file containing a GPP password, and decrypted the `cpassword` field, revealing the plaintext password `abcd@123`.
![s](<Screenshot 2025-12-02 175942.png>)
### Defensive Phase: Detection and Analysis

Next, I switched to a defensive role to detect my own actions.

1.  **Auditing Configuration:** I first ensured that "Object Access" auditing was enabled on the `Groups.xml` file in the SYSVOL share, specifically configured to log access attempts by "Everyone".
![s](<Screenshot 2025-12-02 180148.png>)
2.  **Log Analysis:** I then went to the Event Viewer and looked for **Event ID 4663** ("An attempt was made to access an object").
3.  **Detection:** I successfully identified the attack by finding the specific event where the user `bob` accessed the `Groups.xml` file. This confirmed that the compromised user account was used to read the file containing the GPP credentials.
![s](<Screenshot 2025-12-02 180230.png>)

# *****************************************************************
## Exercise Three: Exposed Credentials in Network Share

### Offensive Phase: Credential Discovery

First, as an attacker, I simulated finding credentials exposed in a network share.

1.  **Initial Access & Recon:** I started by RDP'ing into `WS001` as the user `bob`. From there, I ran `Invoke-ShareFinder` (from PowerView) to discover all accessible network shares in the domain. This revealed a non-standard share: `\\Server01.eagle.local\dev$`.
![s](<Screenshot 2025-12-04 134353.png>)
2.  **Exploitation:** I explored the `dev$` share and used `findstr` to search for PowerShell scripts containing the word "eagle". This led me to a script named `connect.ps1`.
![s](<Screenshot 2025-12-04 134620.png>)
3.  **Credential Extraction:** I examined the `connect.ps1` script and found it contained hardcoded credentials for the Administrator account (`administrator:Slavi123`) used in a `net use` command.

### Defensive Phase: Detection and Analysis

Next, I switched to a defensive role to detect this activity.

1.  **Log Analysis:** I connected to the Domain Controller and opened the Event Viewer, focusing on the Security log for logon events.
2.  **Detection:** I looked for successful logon events (**Event ID 4624**). I identified the attack by finding a logon event for the `Administrator` account that originated from an unusual **Source Network Address** (`172.16.18.20`, the Kali machine). An administrator logging in from a non-standard workstation is a *strong indicator* of credential compromise and lateral movement.
![s](<Screenshot 2025-12-04 134729.png>)
