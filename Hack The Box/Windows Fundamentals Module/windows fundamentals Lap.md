## The Goal
The primary objective is to correctly configure a secure file share by creating a user (`Jim`) and a group (`HR`), and then applying specific Share and NTFS permissions to a folder structure (`Company Data\HR`).

A secondary objective is to answer specific evaluation questions by finding information within the local Windows environment.

---

## The Steps: Configuration

1.  **Connect:** Logged into the Windows machine (`10.129.201.57`) via RDP.
2.  **Create Folders:** Created `C:\Users\htb-student\Desktop\Company Data` and a subfolder `HR`.

![Screenshot](<Images/Screenshot 2025-11-14 041516.png>)

3.  **Create User & Group:**
    *   Created a new local user named `Jim`.
    *   Created a new local security group named `HR`.
    *   Added the `Jim` user to the `HR` group.

       ![Screenshot](<Images/Screenshot 2025-11-14 045253.png>)

5.  **Set Share Permissions:**
    *   Configured sharing on the `Company Data` folder.
    *   Removed the default `Everyone` group from the share permissions.
    *   Added the `HR` group with **Change** and **Read** permissions.
      ![Screenshot](<Images/Screenshot 2025-11-14 044410.png>)
6.  **Set NTFS Permissions:**
    *   On the `Company Data` folder's Security tab, disabled inheritance.
    *   Removed default inherited permissions.
    *   Gave the `HR` group **Modify**, **Read & Execute**, **List folder contents**, **Read**, and **Write** permissions.
7.  **Audit Service:**
    *   Used PowerShell to get detailed properties for the Windows Update service (`wuauserv`).
    *   **Command:**
        ```powershell
        Get-Service -Name wuauserv | Format-List *
        ```
      ![Screenshot](<Images/Screenshot 2025-11-14 045643.png>)
---

## The Steps: Answering Evaluation Questions

#### 1. Question: What is the name of the group that is present in the Company Data Share Permissions ACL by default?
*   **Action Taken:**
    1.  Right-clicked the `Company Data` folder -> **Properties**.
    2.  Navigated to the **Sharing** tab -> **Advanced Sharing...** -> **Permissions**.
    3.  Observed the list of groups present before making any changes.
*   **Answer Found:** `Everyone`

#### 2. Question: What is the name of the tab that allows you to configure NTFS permissions?
*   **Action Taken:**
    1.  Right-clicked the `Company Data` folder -> **Properties**.
    2.  Examined the available tabs at the top of the properties window.
*   **Answer Found:** `Security`

#### 3. Question: What is the name of the service associated with Windows Update?
*   **Action Taken:**
    1.  Opened PowerShell.
    2.  Ran the command `Get-Service` to list all services.
    3.  Scanned the `DisplayName` column for "Windows Update" and noted the corresponding `Name`.
*   **Answer Found:** `wuauserv`

#### 4. Question: List the SID associated with the user account Jim you created.
*   **Action Taken:**
    1.  Opened Command Prompt or PowerShell.
    2.  Ran the command `wmic useraccount where name='Jim' get sid` to query for the local user's SID.
*   **Answer Found:** `S-1-5-21-2614195641-1726409526-3792725429-1006`

#### 5. Question: List the SID associated with the HR security group you created.
*   **Action Taken:**
    1.  Opened Command Prompt or PowerShell.
    2.  Ran the command `wmic group where name='HR' get sid` to query for the local group's SID.
*   **Answer Found:** `S-1-5-21-2614195641-1726409526-3792725429-1007`
