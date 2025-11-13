# Here we will try to answer these 4 questions :
![Screenshot](<Images/Screenshot 2025-11-13 100507.png>)
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
