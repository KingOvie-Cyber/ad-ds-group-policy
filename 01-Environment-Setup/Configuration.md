# Initial Server Configuration

## Overview

Following the successful installation of Windows Server 2022, several post-installation configuration tasks were completed to prepare the server for its future role as the Domain Controller.

These configurations establish a secure and stable operating environment before deploying Active Directory Domain Services (AD DS). Performing these tasks before promoting the server helps prevent unnecessary configuration changes after the server becomes a Domain Controller.

---

# Purpose

The purpose of this phase was to:

- Prepare the operating system for enterprise deployment.
- Verify the health of the server.
- Apply the latest operating system updates.
- Configure administrative settings.
- Prepare the server for Active Directory installation.

---

# System Information

| Property | Value |
|----------|-------|
| Operating System | Windows Server 2022 Standard (Desktop Experience) |
| Server Role | Standalone Server |
| Planned Role | Domain Controller |
| Computer Name | DC01 *(Configured later in this phase)* |

---

# Initial Assessment

Immediately after installation, the following areas were reviewed:

- Operating system version
- Server Manager status
- Device Manager
- Network adapter detection
- Storage availability
- Event Viewer
- Windows Activation status

No hardware or operating system issues were identified during the initial assessment.

---

# Windows Updates

The server was updated with the latest available Windows updates before any infrastructure roles were installed.

Installing updates before deploying Active Directory reduces compatibility issues, improves security, and ensures the operating system includes the latest fixes released by Microsoft.

The update process included:

- Security Updates
- Quality Updates
- Servicing Stack Updates
- .NET Framework Updates (where applicable)

After installation, the server was restarted to complete the update process.

---

# Server Manager Verification

Server Manager was used to verify the server's health and ensure that no configuration warnings or installation errors were present.

The following components were confirmed to be functioning normally:

- Local Server
- Event Viewer
- File and Storage Services
- Device Manager
- Computer Management

---

# Windows Activation

The operating system activation status was reviewed to confirm that Windows was properly licensed.

PowerShell command used:

```powershell
slmgr /xpr
```

Expected outcome:

```text
The machine is permanently activated.
```

> **Note:** Evaluation editions may display a remaining evaluation period instead of permanent activation.

---

# Operating System Verification

PowerShell was used to validate the operating system details.

```powershell
Get-ComputerInfo
```

The following information was reviewed:

- Windows Version
- Build Number
- BIOS Version
- Installed Memory
- Computer Name
- System Manufacturer
- Operating System Architecture

---

# Storage Verification

Available storage volumes were reviewed to confirm that sufficient disk space existed before installing server roles.

PowerShell command:

```powershell
Get-Volume
```

The system volume contained adequate free space for:

- Active Directory Database
- SYSVOL
- DNS
- Windows Logs
- Future updates

---

# Network Adapter Verification

The installed network adapter was verified.

PowerShell command:

```powershell
Get-NetAdapter
```

Verification confirmed:

- Adapter Status: Up
- Link Speed
- MAC Address
- Interface Name

---

# Event Log Review

Windows Event Viewer was reviewed for any critical system events immediately following installation.

The following logs were inspected:

- System
- Application
- Security

No critical or recurring errors were identified during the initial review.

---

# Administrative Configuration

The local Administrator account was retained for initial server administration.

Administrative tasks during this phase were performed using:

- Server Manager
- Windows PowerShell
- Command Prompt
- Computer Management

---

# Configuration Validation

The following commands were executed to validate the server configuration.

Display hostname:

```powershell
hostname
```

Display operating system details:

```powershell
systeminfo
```

Display network configuration:

```powershell
ipconfig /all
```

Display installed Windows features:

```powershell
Get-WindowsFeature
```

---

# Outcome

Upon completion of the initial configuration phase:

- Windows Server was fully operational.
- All available updates had been installed.
- No hardware issues were identified.
- Network connectivity was confirmed.
- Administrative tools were functioning correctly.
- The server was ready for infrastructure configuration.

---

# Best Practices Applied

The following best practices were followed during this phase:

- Updated the operating system before installing infrastructure roles.
- Reviewed Event Viewer for potential issues.
- Verified system resources before deployment.
- Confirmed successful network adapter initialization.
- Documented the server's baseline configuration.
- Restarted the server after updates to ensure pending changes were applied.

---

# Lessons Learned

Preparing the operating system before installing Active Directory significantly reduces deployment issues. Establishing a clean baseline also simplifies future troubleshooting by making it easier to identify changes introduced during later configuration phases.

---

# Summary

The Windows Server installation was successfully validated and prepared for infrastructure deployment. With the operating system updated and verified, the next phase focuses on configuring the server's networking, including assigning a static IP address and configuring DNS settings required for Active Directory Domain Services.
