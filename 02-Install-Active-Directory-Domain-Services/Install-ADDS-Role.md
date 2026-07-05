# Install Active Directory Domain Services (AD DS)

## Executive Summary

Following the successful preparation and validation of the Windows Server environment, the Active Directory Domain Services (AD DS) server role was installed on **DC01**.

The installation included the AD DS role service, required feature dependencies, and Microsoft management tools necessary for administering an Active Directory environment.

At the completion of this phase, the server remained a standalone Windows Server. No Active Directory domain had been created at this stage, and the server had not yet been promoted to a Domain Controller.

---

# Change Summary

| Property | Value |
|----------|-------|
| Change Type | Infrastructure Deployment |
| Target Server | DC01 |
| Operating System | Windows Server 2022 Standard |
| Change Description | Installation of Active Directory Domain Services |
| Implementation Status | Successful |

---

# Business Justification

Deploying Active Directory Domain Services provides the foundation for centralized identity and access management across Windows environments.

Installing the AD DS role enables future implementation of:

- Centralized user authentication
- Organizational Unit (OU) management
- Group Policy deployment
- Security group administration
- Kerberos authentication
- Enterprise directory services

This installation establishes the platform upon which the organization's identity infrastructure will operate.

---

# Scope of Implementation

The following components were installed:

- Active Directory Domain Services
- Active Directory Administrative Center
- Active Directory Users and Computers
- Active Directory Domains and Trusts
- Active Directory Sites and Services
- Group Policy Management Console (GPMC)
- Active Directory PowerShell Module
- Required Windows Feature Dependencies

---

# Installation Method

The Active Directory Domain Services role was installed using **Server Manager** through the **Add Roles and Features Wizard**.

The following installation options were selected:

| Configuration | Selection |
|--------------|-----------|
| Installation Type | Role-based or Feature-based Installation |
| Target Server | DC01 |
| Server Role | Active Directory Domain Services |
| Management Tools | Installed |
| Feature Dependencies | Installed Automatically |

The installation completed successfully without requiring any manual dependency installation.

---

# Feature Dependencies

During installation, Windows automatically installed the required supporting components.

| Feature | Installed |
|----------|-----------|
| Group Policy Management | ✅ |
| Remote Server Administration Tools (RSAT) | ✅ |
| AD DS Snap-ins | ✅ |
| PowerShell Module | ✅ |
| Management Console Extensions | ✅ |

---

# Post-Installation Status

Following installation:

- The AD DS binaries were installed.
- Administrative tools became available.
- The server remained a standalone system.
- A post-deployment notification became available in Server Manager indicating that the server was ready for Domain Controller promotion.

No changes were made to authentication services during this phase.

---

# Verification

The installation was validated using PowerShell.

Display installed Windows features:

```powershell
Get-WindowsFeature AD-Domain-Services
```

Expected Output

```text
Display Name                                            Name                     Install State
------------                                            ----                     -------------
[X] Active Directory Domain Services                    AD-Domain-Services       Installed
```

---

Display all installed Active Directory features.

```powershell
Get-WindowsFeature *AD*
```

---

Verify the Active Directory module.

```powershell
Get-Module -ListAvailable ActiveDirectory
```

Expected Result:

The ActiveDirectory PowerShell module is available for administrative use.

---

Verify installed management tools.

```powershell
Get-WindowsFeature RSAT*
```

---

# Validation Results

| Validation Item | Status |
|-----------------|--------|
| AD DS Installed | ✅ |
| Required Features Installed | ✅ |
| Management Tools Available | ✅ |
| PowerShell Module Available | ✅ |
| Installation Completed Successfully | ✅ |

---

# Event Log Review

Following installation, the Windows Event Viewer was reviewed to identify any installation-related warnings or errors.

The following logs were inspected:

- System
- Application
- Server Manager

No critical installation errors were identified.

---

# Operational Impact

Installing the AD DS role has minimal operational impact because the server has not yet been promoted to a Domain Controller.

At this stage:

- No domain exists.
- Authentication continues to use the local Administrator account.
- Clients cannot join a domain.
- Group Policy is not yet available.
- Active Directory database files have not yet been created.

---

# Rollback Considerations

If the deployment must be reversed before Domain Controller promotion, the AD DS role can be removed without affecting domain services, since no domain has been created.

PowerShell command:

```powershell
Uninstall-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

> **Note:** Once the server has been promoted to a Domain Controller, the removal process requires demotion before uninstalling the AD DS role.

---

# Common Issues

## Installation Fails

### Possible Causes

- Pending Windows restart.
- Corrupted component store.
- Insufficient privileges.

### Resolution

- Restart the server.
- Confirm administrative privileges.
- Review Server Manager installation logs.

---

## Missing Management Tools

### Possible Cause

Management tools were not selected during installation.

### Resolution

Reinstall the AD DS role and ensure **Include Management Tools** is selected.

---

## PowerShell Module Not Found

### Possible Cause

RSAT components were not installed.

### Resolution

Verify installed Windows features:

```powershell
Get-WindowsFeature RSAT*
```

---

# Best Practices

- Install the AD DS role only after completing server validation.
- Include all management tools during installation.
- Review installation logs after deployment.
- Validate feature installation before Domain Controller promotion.
- Document all infrastructure changes.

---

# Lessons Learned

Installing Active Directory Domain Services is a preparatory step rather than the deployment of a functioning domain. Separating the installation phase from the Domain Controller promotion phase allows administrators to validate the environment before making irreversible infrastructure changes.

---

# Conclusion

The Active Directory Domain Services role was successfully installed on **DC01**, along with all required dependencies and administrative tools.

The server is now fully prepared for promotion to the organization's first Domain Controller.
