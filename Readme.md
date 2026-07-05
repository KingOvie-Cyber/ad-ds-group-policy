# Active Directory Domain Services (AD DS) & Group Policy Administration Lab

![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-blue)
![Active Directory](https://img.shields.io/badge/Active%20Directory-Domain%20Services-success)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1+-5391FE)
![License](https://img.shields.io/badge/License-MIT-green)

---

# Project Overview

This repository documents the deployment and administration of a Microsoft Active Directory Domain Services (AD DS) environment from initial server installation to enterprise domain management.

The project demonstrates the complete process of designing, deploying, configuring, and managing a Windows Server domain environment while following Microsoft best practices. It also showcases the implementation of Group Policy Objects (GPOs), Active Directory administration, DNS configuration, and PowerShell automation.

This repository serves as both a technical knowledge base and a portfolio project demonstrating practical Windows Server administration skills.

---

# Objectives

This project covers the following objectives:

- Deploy a Windows Server environment.
- Configure a static IP address.
- Install Active Directory Domain Services (AD DS).
- Promote a Windows Server to a Domain Controller.
- Configure DNS.
- Join Windows client computers to the domain.
- Create Organizational Units (OUs).
- Create and manage Active Directory users.
- Create and manage security groups.
- Deploy and manage Group Policy Objects (GPOs).
- Verify Group Policy deployment.
- Perform administrative tasks using PowerShell.
- Validate the deployment using Microsoft administration tools.
- Document troubleshooting procedures.

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Windows Server 2022 | Domain Controller |
| Windows 10 Pro | Domain Client |
| Windows 11 Pro | Domain Client |
| Active Directory Domain Services | Identity Management |
| DNS Server | Name Resolution |
| Group Policy Management | Centralized Administration |
| Active Directory Users and Computers | Directory Administration |
| PowerShell | Automation |
| Server Manager | Windows Server Management |
| Hyper-V / VMware / VirtualBox | Virtualization |

---

# Repository Structure

```text
Active-Directory-Domain-Services-Lab/
│
├── README.md
├── docs/
│   ├── prerequisites.md
│   ├── architecture.md
│   └── network-topology.md
│
├── 01-Environment-Setup/
├── 02-Install-Active-Directory-Domain-Services/
├── 03-Promote-Server-to-Domain-Controller/
├── 04-DNS-Configuration/
├── 05-Join-Client-to-Domain/
├── 06-Active-Directory-Users-and-Computers/
├── 07-Group-Policy/
├── 08-PowerShell/
├── 09-Validation-and-Testing/
├── 10-Troubleshooting/
│
└── assets/
```

---

# Project Workflow

The project follows the deployment sequence below:

1. Prepare the Windows Server environment.
2. Configure networking.
3. Install Active Directory Domain Services.
4. Promote the server to a Domain Controller.
5. Configure DNS.
6. Join Windows clients to the domain.
7. Create Organizational Units.
8. Create users and groups.
9. Configure Group Policy Objects.
10. Validate the deployment.
11. Troubleshoot common issues.

---

# Skills Demonstrated

This project demonstrates practical experience with:

- Windows Server Administration
- Active Directory Administration
- Domain Controller Deployment
- DNS Administration
- Windows Domain Management
- Organizational Unit Design
- User Administration
- Group Administration
- Group Policy Management
- PowerShell Automation
- Enterprise Troubleshooting
- Microsoft Infrastructure Administration

---

# Sample Group Policies Implemented

- Password Policy
- Account Lockout Policy
- Disable Control Panel
- Disable Command Prompt
- Disable Registry Editor
- Disable Task Manager
- Desktop Wallpaper Policy
- USB Storage Restriction
- Windows Firewall Policy
- Windows Update Policy
- Drive Mapping
- Logon Scripts
- Folder Redirection

---

# Validation Tools

The deployment is validated using:

- Active Directory Users and Computers (ADUC)
- Group Policy Management Console (GPMC)
- DNS Manager
- Server Manager
- Event Viewer
- PowerShell
- Command Prompt

Common commands include:

```powershell
gpupdate /force
gpresult /r
dcdiag
repadmin /replsummary
nslookup
ipconfig /all
```

---

# Best Practices

- Assign a static IP address before installing AD DS.
- Rename the server before promoting it to a Domain Controller.
- Keep Windows fully updated.
- Use descriptive Organizational Units.
- Apply the Principle of Least Privilege.
- Test Group Policies before production deployment.
- Document all configuration changes.
- Back up Active Directory regularly.

---

# Future Improvements

Future enhancements for this lab include:

- Additional Domain Controller deployment
- Active Directory Certificate Services (AD CS)
- Distributed File System (DFS)
- DHCP Integration
- Windows Server Update Services (WSUS)
- Local Administrator Password Solution (LAPS)
- Microsoft Entra ID Integration
- Azure AD Connect
- Windows Admin Center
- Windows Event Forwarding (WEF)
- SIEM Integration (Splunk / Wazuh)

---

# Author

**King Ovie**

Cybersecurity Engineer | SOC Analyst | Windows Server Administrator | Cloud Engineer

---

# Disclaimer

This repository was created for educational, laboratory, and portfolio purposes. All deployment steps should be tested in a non-production environment before implementation in enterprise infrastructure.
