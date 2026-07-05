# Prerequisites

## Overview

Before deploying Active Directory Domain Services (AD DS), it is essential to prepare the environment to ensure a smooth and successful installation. Proper planning reduces deployment issues and establishes a stable foundation for domain services.

This document outlines the hardware, software, networking, and administrative requirements for this project.

---

# Objectives

After completing this phase, you should have:

- A Windows Server ready for deployment.
- A properly configured network.
- Administrative access to the server.
- A deployment plan for Active Directory.
- An environment that meets Microsoft's recommended requirements.

---

# Lab Environment

| Component | Value |
|------------|-------|
| Windows Server Version | Windows Server 2022 Standard |
| Client Operating Systems | Windows 10 Pro / Windows 11 Pro |
| Domain Name | corp.local |
| NetBIOS Name | CORP |
| Domain Controller Name | DC01 |
| Virtualization Platform | VMware Workstation / Hyper-V / VirtualBox |

---

# Hardware Requirements

## Domain Controller

| Component | Minimum | Recommended |
|------------|----------|-------------|
| CPU | 2 Cores | 4 Cores |
| Memory | 4 GB RAM | 8 GB RAM or higher |
| Storage | 60 GB | 100 GB or higher |
| Network Adapter | 1 | 1 |

---

## Client Computer

| Component | Minimum |
|------------|----------|
| CPU | 2 Cores |
| Memory | 4 GB RAM |
| Storage | 50 GB |
| Operating System | Windows 10 Pro or Windows 11 Pro |

---

# Software Requirements

The following software is required:

- Windows Server 2022 ISO
- Windows 10 Pro ISO
- Windows 11 Pro ISO
- Hyper-V, VMware Workstation, or VirtualBox
- PowerShell 5.1 or later
- Remote Desktop (Optional)

---

# Administrative Requirements

The account used during deployment should have:

- Local Administrator privileges
- Permission to install Windows Roles and Features
- Permission to restart the server
- Permission to modify network settings

---

# Network Requirements

The Domain Controller must use a **static IP address**.

Example configuration:

| Setting | Example |
|----------|---------|
| IP Address | 192.168.1.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |
| Preferred DNS | 192.168.1.10 |

> **Important:** After installing AD DS, the Domain Controller should point to itself as its preferred DNS server.

---

# Server Naming Convention

Use meaningful server names.

| Server Name | Purpose |
|--------------|---------|
| DC01 | Primary Domain Controller |
| FS01 | File Server |
| WSUS01 | Windows Server Update Services |
| APP01 | Application Server |

---

# Client Naming Convention

| Computer Name | Department |
|---------------|------------|
| CLIENT01 | IT |
| CLIENT02 | HR |
| CLIENT03 | Finance |
| CLIENT04 | Operations |

---

# Planned Organizational Unit Structure

The following Organizational Units (OUs) will be created later in the project.

```text
CORP.LOCAL
│
├── Users
├── Groups
├── Computers
├── Servers
├── Workstations
├── IT
├── HR
├── Finance
└── Service Accounts
```

---

# Windows Configuration Checklist

Before installing Active Directory, verify the following:

- Windows Server installation completed.
- Administrator password configured.
- Latest Windows Updates installed.
- Static IP address assigned.
- Server renamed to **DC01**.
- Correct date and time configured.
- Network connectivity verified.
- Disk space confirmed.
- Windows Firewall reviewed.
- Virtual machine snapshot created (if applicable).

---

# Verify System Configuration

Open **PowerShell** and verify the computer name.

```powershell
hostname
```

Display IP configuration.

```powershell
ipconfig /all
```

Display network configuration.

```powershell
Get-NetIPAddress
```

Display general system information.

```powershell
Get-ComputerInfo
```

---

# Verify Network Connectivity

Test connectivity to the default gateway.

```cmd
ping 192.168.1.1
```

Verify DNS resolution.

```cmd
nslookup
```

---

# Security Recommendations

Before deploying Active Directory:

- Use a strong Administrator password.
- Apply all available Windows Updates.
- Disable unnecessary services.
- Restrict remote access to authorized administrators.
- Document all configuration changes.
- Keep installation media from trusted sources.

---

# Expected Outcome

At the end of this phase:

- Windows Server is fully installed.
- Networking is configured correctly.
- The server has a static IP address.
- Administrative access is available.
- The environment is ready for Active Directory installation.

---

# Common Issues

| Issue | Possible Cause | Resolution |
|--------|----------------|------------|
| Dynamic IP Address | DHCP Enabled | Configure a static IP address. |
| Unable to Reach Gateway | Incorrect Network Configuration | Verify IP address, subnet mask, and gateway. |
| Incorrect Computer Name | Server Not Renamed | Rename the server before promoting it to a Domain Controller. |
| Incorrect Date and Time | Time Not Synchronized | Configure the correct date, time, and time zone. |

---

# Best Practices

- Plan the domain structure before deployment.
- Use meaningful server names.
- Always assign static IP addresses to infrastructure servers.
- Maintain consistent naming conventions.
- Create virtual machine snapshots before major configuration changes.
- Keep detailed deployment documentation.
- Test all configurations before moving to production.

---

# Summary

Completing these prerequisites ensures that the Windows Server environment is fully prepared for the installation of Active Directory Domain Services. Proper preparation minimizes deployment issues and provides a stable foundation for the remaining stages of the project.
