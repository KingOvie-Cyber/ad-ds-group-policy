# Environment Setup

## Overview

This section covers the initial preparation of the Windows Server environment before deploying Active Directory Domain Services (AD DS). Proper server preparation is critical to ensure a stable, secure, and reliable domain infrastructure.

The procedures documented in this section follow Microsoft's recommended practices for configuring a Windows Server prior to installing Active Directory.

---

# Objectives

Upon completing this phase, the server will be:

- Installed with Windows Server 2022
- Configured with the latest Windows updates
- Assigned a static IP address
- Renamed according to organizational standards
- Configured for remote administration
- Verified for network connectivity
- Ready for Active Directory Domain Services installation

---

# Why Environment Preparation Matters

A Domain Controller becomes one of the most critical systems in a Windows environment. Incorrect configurations made before promoting a server to a Domain Controller can lead to authentication issues, DNS failures, replication problems, and unnecessary downtime.

Preparing the server correctly ensures:

- Reliable authentication
- Proper DNS functionality
- Stable network communication
- Consistent system configuration
- Easier troubleshooting
- Compliance with Microsoft best practices

---

# Phase Checklist

Complete each of the following tasks before proceeding to the Active Directory installation.

| Step | Status |
|------|--------|
| Install Windows Server | ☐ |
| Configure Administrator Password | ☐ |
| Install Windows Updates | ☐ |
| Rename the Server | ☐ |
| Configure Static IP Address | ☐ |
| Verify Network Connectivity | ☐ |
| Configure DNS Settings | ☐ |
| Enable Remote Desktop (Optional) | ☐ |
| Verify System Information | ☐ |
| Create a Virtual Machine Snapshot (Optional) | ☐ |

---

# Documents in This Section

| File | Description |
|------|-------------|
| Windows-Server-Installation.md | Installing Windows Server 2022 |
| Initial-Server-Configuration.md | Post-installation configuration tasks |
| Network-Configuration.md | Configuring networking and static IP addressing |
| Rename-Server.md | Renaming the server before domain promotion |
| Windows-Updates.md | Installing operating system updates |
| Verification.md | Verifying server readiness before AD DS installation |

---

# Environment Specifications

| Component | Value |
|----------|-------|
| Operating System | Windows Server 2022 Standard |
| Server Name | DC01 |
| Role | Future Domain Controller |
| Domain | corp.local |
| Network | 192.168.1.0/24 |
| Preferred DNS | 192.168.1.10 |

---

# Recommended Configuration Order

Perform the following tasks in the order shown below.

1. Install Windows Server.
2. Configure the Administrator account.
3. Install Windows Updates.
4. Rename the server.
5. Configure a static IP address.
6. Configure DNS settings.
7. Verify network connectivity.
8. Enable Remote Desktop (if required).
9. Restart the server.
10. Verify all configurations.

---

# Validation Checklist

Before moving to the next section, verify the following:

- Windows Server is activated.
- The latest Windows updates are installed.
- The server has been renamed to **DC01**.
- A static IP address has been assigned.
- The correct DNS server has been configured.
- Network connectivity is functioning correctly.
- The system date and time are accurate.
- Administrative access is available.

---

# Best Practices

- Always configure a static IP address before installing AD DS.
- Rename the server before promoting it to a Domain Controller.
- Install all available Windows updates.
- Verify network connectivity before installing server roles.
- Use descriptive naming conventions.
- Keep deployment documentation up to date.
- Take a virtual machine snapshot before making major configuration changes.

---

# Expected Outcome

At the completion of this phase, the Windows Server environment will be fully prepared for the installation of Active Directory Domain Services.

The server will have:

- A stable network configuration
- A static IP address
- Proper DNS settings
- The latest operating system updates
- A standardized hostname
- Verified connectivity
- Administrative readiness for AD DS deployment

---

# Next Steps

After completing all tasks in this section, proceed to:

**02-Install-Active-Directory-Domain-Services**

The next phase covers installing the Active Directory Domain Services role using both the graphical interface and Windows PowerShell.
