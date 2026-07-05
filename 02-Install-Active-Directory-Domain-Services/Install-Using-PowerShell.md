# Installing Active Directory Domain Services Using PowerShell

## Executive Summary

In addition to the graphical installation method provided by Server Manager, Windows Server supports the deployment of Active Directory Domain Services (AD DS) through Windows PowerShell.

PowerShell enables administrators to automate server deployments, standardize infrastructure provisioning, and support repeatable installations across multiple environments.

This document demonstrates the PowerShell implementation that can be used to install the Active Directory Domain Services role and its required management tools.

> **Note**
>
> The primary deployment documented in this project was completed using **Server Manager**. This document provides the equivalent PowerShell implementation for automation and scripting purposes.

---

# Business Justification

Automation is an essential component of modern infrastructure administration.

Using PowerShell provides several operational advantages:

- Consistent server deployments
- Faster provisioning
- Reduced manual configuration
- Simplified disaster recovery
- Repeatable infrastructure builds
- Integration with Infrastructure as Code (IaC) workflows

---

# Prerequisites

Before executing the installation command, verify that:

- Windows Server 2022 is installed.
- Administrative privileges are available.
- The server has completed all prerequisite configuration tasks.
- Pending Windows restarts have been completed.

---

# Installation Command

The AD DS role and associated management tools were installed using the following PowerShell command.

```powershell
Install-WindowsFeature `
    -Name AD-Domain-Services `
    -IncludeManagementTools
```

---

# Expected Output

A successful installation returns output similar to the following:

```text
Success Restart Needed Exit Code      Feature Result
------- -------------- ---------      --------------
True    No             Success        {Active Directory Domain Services}
```

This confirms that the Active Directory Domain Services role and management tools were installed successfully.

---

# Installed Components

Executing the installation command installs the following components:

| Component | Installed |
|----------|-----------|
| Active Directory Domain Services | ✅ |
| AD DS Management Tools | ✅ |
| Active Directory Users and Computers | ✅ |
| Active Directory Administrative Center | ✅ |
| Active Directory Sites and Services | ✅ |
| Active Directory Domains and Trusts | ✅ |
| Group Policy Management Console | ✅ |
| Active Directory PowerShell Module | ✅ |

---

# Verification

Verify that the AD DS role has been installed.

```powershell
Get-WindowsFeature AD-Domain-Services
```

Expected Output:

```text
[X] Active Directory Domain Services
```

---

Verify all Active Directory-related features.

```powershell
Get-WindowsFeature *AD*
```

---

Verify the Active Directory PowerShell module.

```powershell
Get-Module -ListAvailable ActiveDirectory
```

---

Import the Active Directory module.

```powershell
Import-Module ActiveDirectory
```

The command should complete without errors.

---

# Operational Impact

Installing the AD DS role using PowerShell has the same effect as installing it through Server Manager.

At this stage:

- The server remains a standalone Windows Server.
- No domain has been created.
- Authentication continues to use local accounts.
- The server is ready for Domain Controller promotion.

---

# Rollback

If the role must be removed before promotion, execute:

```powershell
Uninstall-WindowsFeature `
    -Name AD-Domain-Services `
    -IncludeManagementTools
```

Verify removal:

```powershell
Get-WindowsFeature AD-Domain-Services
```

Expected Output:

```text
[ ] Active Directory Domain Services
```

---

# Common Issues

## Installation Requires Restart

### Resolution

Restart the server and rerun the verification commands.

---

## Access Denied

### Resolution

Run PowerShell with elevated administrative privileges.

---

## Installation Fails

### Resolution

Review the installation logs in Server Manager and Event Viewer, verify Windows updates are current, and ensure the server meets all prerequisites.

---

# Best Practices

- Execute PowerShell as Administrator.
- Include management tools during installation.
- Validate the installation immediately after completion.
- Document installation output for future reference.
- Use PowerShell automation for repeatable deployments.

---

# Lessons Learned

PowerShell provides a reliable and repeatable method for deploying Windows Server roles. While graphical tools simplify one-time installations, scripting is better suited for enterprise environments where consistency and automation are essential.

---

# Conclusion

The Active Directory Domain Services role can be deployed successfully using Windows PowerShell with a single command, producing the same result as the graphical installation method while supporting automation and standardized infrastructure deployment.
