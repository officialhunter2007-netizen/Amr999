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
# *****************************************************************
## Exercise four: AD Enumeration and Credential Discovery

In this lab environment, I simulated an attack from the compromised Windows 10 machine `WS001`, where the user `Bob` was assumed to be already compromised. From there, I performed several offensive actions to understand how an attacker could enumerate and extract sensitive data inside an Active Directory domain containing multiple servers (`DC1`, `DC2`, `Server01`, `PKI`, etc.).

After simulating the attack, I switched to the defensive perspective and analyzed the environment to detect the malicious activity.

---

### Offensive Phase

-   Gained remote code execution on `WS001` (Windows 10) as user `Bob`.
-   Used PowerShell to enumerate Active Directory users and searched for sensitive information.
![s](<Screenshot 2025-12-05 053815.png>)
-   Discovered cleartext credentials in user attributes (e.g., `Description` field containing `pass: Slavi1234`).
-   Identified the domain controller and queried user properties using `Get-ADUser`.
![s](<Screenshot 2025-12-05 054819.png>)

### Defensive Phase

-   Monitored Windows Security logs for suspicious activity.
-   Detected **Event ID 4768** indicating a Kerberos TGT request from user `bonni`, originating from `WS001`.
![s](<Screenshot 2025-12-05 062836.png>)
# *****************************************************************
## Exercise Five: DCSync Attack Simulation

### Offensive Phase

-   Gained remote code execution on `WS001` as `Bob`, a standard AD user.
-   Used `runas` to escalate privileges and impersonate `rocky`, a domain user.
-   Executed Mimikatz to perform a DCSync attack targeting the `Administrator` account in the `eagle.local` domain.
![s](<Screenshot 2025-12-07 095819.png>)
-   Successfully extracted the NTLM hash of the domain administrator from `DC1`.

### Defensive Phase

-   Investigated Windows Security Event Logs, focusing on **Event ID 4662**.
-   Identified suspicious "Control Access" operations on `domainDNS` objects.
-   Noted that the account `rocky` performed sensitive directory access, despite not being a Domain Controller.
-   Correlated the event with the DCSync activity, confirming unauthorized replication behavior.
![s](<Screenshot 2025-12-07 100346.png>)
# *****************************************************************
## Exercise Six: Golden Ticket Attack Simulation

### Offensive Simulation

-   Gained remote code execution on `WS001` as `Bob`, a regular AD user, using `xfreerdp` for remote access.
![s](<Screenshot 2025-12-07 180426.png>)
-   Escalated privileges using `runas` to impersonate the user `rocky`.
-   Loaded `PowerView.ps1` and retrieved the domain SID.
![s](<Screenshot 2025-12-08 100227.png>)
-   Executed Mimikatz to perform DCSync attacks on the `krbtgt`account.
-   Extracted NTLM hashes and domain replication data.
![s](<Screenshot 2025-12-08 100156.png>)
-   Forged a Golden Ticket using the `krbtgt` hash and the domain SID.
![s](<Screenshot 2025-12-08 100330.png>)
-   Verified ticket injection with `klist` and accessed domain resources.
![s](<Screenshot 2025-12-08 100441.png>)
### Detection and Analysis

-   Detected **Event ID 4624** indicating a suspicious logon with an elevated token, a sign of potential privilege escalation.
![s](<Screenshot 2025-12-08 100615.png>)
-   Correlated with **Event ID 4769** showing Kerberos service ticket requests from `Administrator@eagle.local`, indicating lateral movement attempts.
  
![s](<Screenshot 2025-12-08 100636.png>)
-   Identified forged ticket behavior by analyzing abnormal client addresses and service names in Kerberos logs.
-   Confirmed Golden Ticket activity by matching timestamps and encryption types across related security events.

# *****************************************************************

## Exercise Seven: Print Spooler Attack & Detection Summary

In this lab, I simulated a full attack-and-detect cycle, playing the role of both an attacker and a defender.

---

### Offensive Phase: Abusing the Print Spooler

My goal was to compromise the domain by abusing the Print Spooler service.

-   **Setup:** From my Kali machine (`172.16.18.20`), I started `ntlmrelayx` to listen for connections and relay them to `DC2` (`172.16.18.4`) with the goal of performing a `DCSync` attack.
![s](<Screenshot 2025-12-12 100111.png>)
-   **Execution:** I then used the `dementor.py` script to trigger the "PrinterBug" vulnerability. This forced `DC1` (`172.16.18.3`) to authenticate to my Kali machine.
-   **Result:** My `ntlmrelayx` tool successfully intercepted `DC1`'s powerful machine account credentials, relayed them to `DC2`, and dumped domain password hashes, achieving a full domain compromise.
![s](<Screenshot 2025-12-12 100624.png>)
### Defensive Phase: Detecting the Relay

I then switched roles to see if I could detect my own attack.

-   **Detection:** I quickly found a successful logon event (**Event ID 4624**) for the `DC1$` machine account.
-   **Confirmation:** The key piece of evidence was the **Source Network Address**. The logon came from my Kali machine's IP (`172.16.18.20`), not `DC1`'s actual IP. This mismatch was the definitive proof of the NTLM relay attack.
![s](<Screenshot 2025-12-12 112627.png>)
# *****************************************************************
## Exercise eight: ACL Attack & Defense

In this lab, I simulated a full attack-and-detect cycle focused on Active Directory Access Control Lists (ACLs).

---

### Offensive Phase: ACL Exploitation

My goal was to escalate from a regular user, `Bob`, to a privileged one.

-   **Reconnaissance:** I ran `SharpHound.exe` to collect all permission data from the domain.
![s](<Screenshot 2025-12-14 140225.png>)
-   **Analysis:** I loaded this data into BloodHound, which instantly showed me that my user, `Bob`, had been given `GenericAll` (full control) over another user, `Anni`, and a computer, `Server01`.
![s](<Screenshot 2025-12-14 140301.png>)
-   **Exploitation:** This gave me a clear path to compromise the domain. I could either reset `Anni`'s password to take over her account or abuse my control over `Server01` to gain administrative access.

### Defensive Phase: Detection via Honeypot

I then switched roles to detect my own attack.

-   **Detection:** I analyzed the domain controller's logs and found that when I modified the `Anni` user object, it generated **Event ID 4738** ("A user account was changed"). While this event doesn't show *what* changed, it does show *who* changed it (`Bob`).
![s](<Screenshot 2025-12-14 140409.png>)
-   **Honeypot Strategy:** This led to my defensive strategy: create a "honeypot" user account and intentionally give broad modification permissions to it. Then, set up a high-priority alert to trigger anytime an **Event ID 4738** is logged for this specific honeypot account.
-   **Outcome:** Since no legitimate user should ever modify this trap account, any alert is a reliable sign of an attacker. This allows me to instantly detect the malicious activity and disable the source account (`Bob`) to stop the attack.
