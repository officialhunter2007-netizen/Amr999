![s](<Images/Screenshot 2025-11-11 184124.png>)

# What Hack The Box platform said about their module?
Windows is heavily used across corporate environments of all sizes. We often find ourselves gaining access to a Windows host during a penetration testing engagement. It is important to understand how to navigate the file system and command line to perform effective enumeration, privilege escalation, lateral movement, and post-exploitation. Windows can also be used as our attack box during assessments. Many servers run on Windows, and most companies deploy Windows workstations to their employees due to the ease of use for individuals and centralized administration that can be leveraged using Active Directory. This module covers the essentials for starting with the Windows operating system and command line.

In this module, we will cover:

- Windows Operating system structure
- The Windows file system
- Permissions management
- Windows services
- Processes in Windows
- Windows Task Manager
- Interacting with the operating system
- Windows security
- The Microsoft Management Console (MMC)
- Windows Subsystem for Linux (WSL)

This module is broken down into sections with accompanying hands-on exercises to practice each of the tactics and techniques we cover.

As you work through the module, you will see example commands and command output for the various topics introduced. It is worth reproducing as many of these examples as possible to reinforce further the concepts introduced in each section. You can do this in the target host provided in the interactive sections or your own virtual machine.

You can start and stop the module at any time and pick up where you left off. There is no time limit or "grading," but you must complete all of the exercises and the skills assessment to receive the maximum number of cubes and have this module marked as complete in any paths you have chosen.

The module is classified as "Fundamental" and assumes that the student has a basic knowledge of the Windows operating system from a casual user perspective.

This module has no prerequisites but serves as the basis for many of the modules contained within the Academy. Completion and an in-depth understanding of this module are crucial for success as you progress through the Academy and Hack the Box platforms.

# tools used in the module:


### Section 1 & 2: Introduction to Windows & OS Structure

#### Command-Line Utilities
*   **dir**: A built-in Command Prompt command to list the contents of a directory.
*   **tree**: A command-line utility that graphically displays the folder structure of a path.
*   **more**: A command used to display output one screen at a time, often piped from other commands (e.g., `tree c:\ /f | more`).

#### PowerShell Cmdlets
*   **Get-WmiObject**: A PowerShell cmdlet used to get information from the Windows Management Instrumentation (WMI) repository. Used here to find the OS version (`-Class win32_OperatingSystem`).

#### Remote Access Tools
*   **xfreerdp**: A command-line based Remote Desktop Protocol (RDP) client for Linux, used to connect to a Windows GUI.
*   **mstsc.exe (Remote Desktop Connection)**: The native Windows GUI client for RDP.

---

### Section 3 & 4: File System & NTFS vs. Share Permissions

#### Command-Line Utilities
*   **icacls**: A command-line utility for displaying and modifying file and folder permissions (NTFS Access Control Lists).
*   **smbclient**: A Linux-based command-line client for interacting with SMB/CIFS file shares (listing shares with `-L` or connecting to them).
*   **net share**: A Windows command to view all shared folders on the local system.
*   **mount (with -t cifs)**: A Linux command used to mount a remote Windows (CIFS/SMB) share to a local directory.

#### GUI-Based Management Tools
*   **Computer Management**: A Windows administrative tool used here to monitor shared folders, active sessions, and open files.
*   **Event Viewer (eventvwr.msc)**: The standard Windows tool for viewing system, security, and application logs.

---

### Section 5 & 6: Windows Services, Processes & Permissions

#### Command-Line Utilities
*   **sc.exe (Service Controller)**: The primary command-line tool for managing Windows Services (querying, starting, stopping, configuring).
*   **procdump.exe**: A Sysinternals tool for creating memory dumps of running processes, mentioned as being accessible from `\\live.sysinternals.com\tools`.

#### PowerShell Cmdlets
*   **Get-Service**: A PowerShell cmdlet for listing, filtering, and getting the status of Windows services.
*   **Get-Acl**: A PowerShell cmdlet to get the security descriptor (permissions) for an item, used here to inspect service permissions in the registry.

#### GUI-Based Management Tools
*   **services.msc**: The MMC snap-in for managing Windows services via a GUI.
*   **Task Manager (taskmgr.exe)**: A system monitor for viewing running processes, performance, and services.
*   **Resource Monitor**: An advanced performance monitoring tool, accessible from the Task Manager.
*   **Process Explorer**: A Sysinternals tool that acts as an advanced Task Manager, showing detailed process information, including loaded DLLs and handles.
*   **TCPView**: A Sysinternals tool mentioned for monitoring internet activity.
*   **PSExec**: A Sysinternals tool mentioned for remote management via the SMB protocol.

---

### Section 7 & 8: Windows Sessions & Interacting with the OS

#### Command-Line Utilities
*   **cmd.exe (Command Prompt)**: The legacy command-line interpreter for Windows.
*   **help**: The built-in `cmd.exe` command to get information about other commands.
*   **ipconfig**: A command to display IP configuration information.
*   **schtasks.exe**: The command-line tool for managing Windows Scheduled Tasks.

#### PowerShell & Related
*   **PowerShell (powershell.exe)**: The modern, object-oriented command-line shell and scripting language.
*   **PowerShell ISE (Integrated Scripting Environment)**: A built-in graphical editor for writing, testing, and debugging PowerShell scripts.
*   **Get-ChildItem (Aliases: ls, gci)**: The cmdlet for listing items in a directory.
*   **Set-Location (Aliases: cd, sl)**: The cmdlet for changing the current directory.
*   **Get-Alias / New-Alias**: Cmdlets for viewing and creating command shortcuts (aliases).
*   **Get-Help / Update-Help**: Cmdlets for accessing and updating PowerShell's built-in documentation.
*   **Get-Module / Import-Module**: Cmdlets for listing and loading PowerShell modules.
*   **Get-ExecutionPolicy / Set-ExecutionPolicy**: Cmdlets for managing PowerShell's script execution security feature.

---

### Section 9: Windows Management Instrumentation (WMI)

#### Command-Line Utilities
*   **wmic.exe**: The command-line interface for WMI. It is noted as being deprecated but still functional.

#### PowerShell Cmdlets
*   **Get-WmiObject**: The primary PowerShell cmdlet for querying WMI for system information.
*   **Invoke-WmiMethod**: A PowerShell cmdlet used to execute methods on WMI objects (e.g., renaming a file).

---

### Section 10: Microsoft Management Console (MMC)

#### GUI-Based Management Tools
*   **mmc.exe (Microsoft Management Console)**: A framework that hosts administrative tools ("snap-ins") to create custom management consoles.

---

### Section 11: Windows Subsystem for Linux (WSL)

#### Command-Line Utilities
*   **bash.exe**: The executable that launches a Bash shell within the WSL environment.
*   **uname**: A Linux command shown being run inside WSL to display system information.

#### PowerShell Cmdlets
*   **Enable-WindowsOptionalFeature**: The PowerShell cmdlet used to install WSL.

---

### Section 12: Desktop Experience vs. Server Core

#### Command-Line Utilities
*   **Sconfig.vbs**: A text-based interface script for initial configuration of Windows Server Core.

---

### Section 13: Windows Security

#### Command-Line Utilities
*   **whoami**: A command to display the current user's information, used here with `/user` to show the SID.
*   **reg query**: A command-line tool to query values from the Windows Registry.

#### GUI-Based Management Tools
*   **regedit.exe**: The Windows Registry Editor GUI.
*   **gpedit.msc**: The Local Group Policy Editor.
*   **Windows Security Center**: The central GUI for managing Windows Defender Antivirus and other security features.

#### PowerShell Cmdlets
*   **Get-MpComputerStatus**: A PowerShell cmdlet to check the status of Windows Defender Antivirus protection settings.

# Cybersecurity Skills Gained from This Module:

By finishing this module, a user has acquired the following operational cybersecurity skills:

#### 1. System Enumeration & Reconnaissance
*   **Identify OS Details**: Use tools like `Get-WmiObject` to determine the specific Windows version and build number, which is crucial for identifying potential vulnerabilities.
*   **Map File Systems**: Use `dir`, `tree`, and `Get-ChildItem` to explore the file system, identify non-standard directories, and locate sensitive files or misconfigurations.
*   **Enumerate Network Shares**: Use `net share` and `smbclient` to discover and inspect network shares, identifying potential data exposure or pivot points.
*   **Analyze Running Processes & Services**: Use Task Manager, `sc.exe`, and `Get-Service` to identify running processes and services, looking for unusual or misconfigured applications that could be exploited.
*   **Leverage WMI for Deep Enumeration**: Use `wmic` and `Get-WmiObject` to query the WMI repository for in-depth system and hardware details, both locally and remotely.

#### 2. Access Control & Permissions Analysis
*   **Understand and Analyze NTFS Permissions**: Use `icacls` and `Get-Acl` to read and interpret complex file and folder permissions, identifying weaknesses like excessive "Full Control" or "Write" access.
*   **Differentiate and Exploit Share vs. NTFS Permissions**: Understand the critical difference between Share permissions (network access) and NTFS permissions (local access) and how the most restrictive of the two applies, allowing for the identification of misconfigured shares.
*   **Analyze Service Permissions**: Use `sc.exe sdshow` and `Get-Acl` to inspect service permissions (DACLs), identifying services with weak configurations that could be hijacked for privilege escalation (e.g., modifying the `BINARY_PATH_NAME`).
*   **Decode Security Descriptors (SDDL)**: Gain a foundational understanding of how to read SDDL strings to determine which users/groups have specific rights over an object.

#### 3. Persistence & Lateral Movement Techniques
*   **Identify Persistence Mechanisms**: Use `reg query` to inspect the `Run` and `RunOnce` registry keys, a common location for malware to establish persistence.
*   **Leverage Remote Access Protocols**: Use tools like `xfreerdp` and knowledge of RDP to gain access to systems. Identify and search for saved `.rdp` files that may contain credentials or connection information.
*   **Abuse Service Misconfigurations**: Understand the theory behind replacing a service's executable path or abusing recovery options to execute malicious code, a key privilege escalation and persistence technique.
*   **Utilize WSL for "Living off the Land"**: Understand that the Windows Subsystem for Linux (WSL) can be used to run native Linux attack tools directly on a Windows host, potentially bypassing some security controls.

#### 4. Windows Security Feature Evasion & Hardening
*   **Understand and Bypass Execution Policy**: Use `Set-ExecutionPolicy Bypass -Scope Process` to bypass PowerShell's script execution restrictions for a single session, a fundamental step for running custom scripts on a target.
*   **Analyze and Configure Firewall Rules**: Understand the role of Windows Defender Firewall profiles (Public, Private, Domain) and how misconfigurations (like being turned off) can allow unauthorized network access.
*   **Identify Application Whitelisting**: Understand the concept of AppLocker and how it can be used to restrict executable and script execution, informing an attacker's strategy for bypassing it.
*   **Assess Antivirus Status**: Use `Get-MpComputerStatus` to check the status of Windows Defender, determining if real-time protection is active and if there are any exclusions that could be abused.
*   **Understand UAC**: Gain a conceptual understanding of User Account Control (UAC) and how it separates standard and administrative privileges, which is a key consideration for privilege escalation attacks.

#### 5. Identity & Credential Security
*   **Understand SIDs**: Use `whoami /user` to find a user's Security Identifier (SID) and understand its structure, which is fundamental to how Windows tracks security principals.
*   **Identify High-Value Processes**: Recognize that `lsass.exe` is the process responsible for storing credentials in memory, making it a primary target for credential dumping attacks with tools like Mimikatz.
*   **Differentiate Account Types**: Understand the security implications of different account types (Local vs. Domain, User vs. Service accounts like `NT AUTHORITY\SYSTEM`) and their default privilege levels.
