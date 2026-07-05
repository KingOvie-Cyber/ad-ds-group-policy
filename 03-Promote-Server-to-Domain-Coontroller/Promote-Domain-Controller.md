# Promote Server to Domain Controller

## Executive Summary

After successfully installing and validating the Active Directory Domain Services (AD DS) role, **DC01** was promoted to the organization's first Domain Controller.

The promotion process established a new Active Directory forest and domain, installed an Active Directory-integrated DNS server, configured the Global Catalog, initialized the Active Directory database (NTDS), created the SYSVOL share, and enabled centralized authentication services.

Upon completion, the server transitioned from a standalone Windows Server into the first Domain Controller responsible for identity and access management within the environment.

---

# Change Summary

| Property | Value |
|----------|-------|
| Change Type | Infrastructure Deployment |
| Server | DC01 |
| Previous Role | Standalone Windows Server |
| New Role | Domain Controller |
| Forest Name | corp.local |
| Domain Name | corp.local |
| NetBIOS Name | CORP |
| DNS Installation | Active Directory Integrated |
| Global Catalog | Enabled |
| Read-Only Domain Controller | No |

---

# Business Justification

Promoting the server to a Domain Controller establishes the organization's centralized identity platform.

This enables:

- Centralized authentication
- Domain-based authorization
- Organizational Unit (OU) administration
- Security Group management
- Group Policy deployment
- Kerberos authentication
- Integrated DNS services
- Centralized management of users and computers

This implementation removes the need to manage separate local accounts on individual systems and provides a scalable identity infrastructure.

---

# Implementation Overview

The Domain Controller promotion was completed using the **Active Directory Domain Services Configuration Wizard** in Server Manager.

The following configuration was implemented:

| Configuration | Value |
|--------------|-------|
| Deployment Operation | Add a New Forest |
| Root Domain Name | corp.local |
| Forest Functional Level | Windows Server 2022 (or highest available) |
| Domain Functional Level | Windows Server 2022 (or highest available) |
| DNS Server | Installed |
| Global Catalog | Enabled |
| Read-Only Domain Controller | Disabled |

---

# Directory Services Restore Mode (DSRM)

A **Directory Services Restore Mode (DSRM)** password was configured during the promotion process.

DSRM provides an offline recovery mode that can be used for:

- Active Directory database recovery
- Authoritative restores
- Non-authoritative restores
- Directory maintenance
- Disaster recovery

> **Security Note**
>
> The DSRM password should be stored securely in accordance with the organization's credential management policy.

---

# Active Directory Database

Following promotion, the following core components were created:

| Component | Default Location |
|----------|------------------|
| NTDS Database | `C:\Windows\NTDS` |
| Log Files | `C:\Windows\NTDS` |
| SYSVOL | `C:\Windows\SYSVOL` |

These components form the foundation of the Active Directory infrastructure.

---

# DNS Configuration

As part of the promotion process:

- DNS Server was installed.
- An Active Directory-integrated DNS zone was created.
- Secure dynamic updates were enabled.
- The Domain Controller registered its required DNS records.

This configuration allows domain-joined systems to locate domain services using DNS.

---

# Server Restart

Upon completion of the promotion process, the server restarted automatically to finalize the Domain Controller configuration.

After the restart:

- Active Directory services were initialized.
- DNS services started.
- SYSVOL was shared.
- Domain authentication became available.

---

# Verification

The promotion was validated using the following PowerShell commands.

## Verify Domain Information

```powershell
Get-ADDomain
```

Expected Result:

The command returns information about the newly created domain.

---

## Verify Forest Information

```powershell
Get-ADForest
```

---

## Verify Domain Controller

```powershell
Get-ADDomainController
```

---

## Verify Installed Forest

```powershell
(Get-ADForest).RootDomain
```

Expected Output:

```text
corp.local
```

---

## Verify SYSVOL

```powershell
net share
```

Expected Output includes:

```text
NETLOGON
SYSVOL
```

---

## Verify DNS Service

```powershell
Get-Service DNS
```

Expected Output:

```text
Status   Name
------   ----
Running  DNS
```

---

## Verify AD DS Service

```powershell
Get-Service NTDS
```

Expected Output:

```text
Status   Name
------   ----
Running  NTDS
```

---

# Validation Results

| Validation Item | Status |
|-----------------|--------|
| Domain Created | ✅ Passed |
| Forest Created | ✅ Passed |
| DNS Installed | ✅ Passed |
| Global Catalog Enabled | ✅ Passed |
| SYSVOL Shared | ✅ Passed |
| NETLOGON Shared | ✅ Passed |
| NTDS Service Running | ✅ Passed |
| DNS Service Running | ✅ Passed |

---

# Operational Impact

Following promotion:

- DC01 became the organization's first Domain Controller.
- Local authentication was supplemented by domain-based authentication.
- Active Directory administrative tools became fully operational.
- DNS was integrated with the directory service.
- The environment was ready to onboard domain-joined client systems.

---

# Rollback Considerations

Reverting this change requires **demoting** the Domain Controller before removing the AD DS role.

Example PowerShell command:

```powershell
Uninstall-ADDSDomainController
```

> **Note:** Demotion should only be performed after assessing the impact on dependent services and ensuring appropriate backups exist.

---

# Best Practices

- Verify DNS functionality immediately after promotion.
- Securely store the DSRM password.
- Validate SYSVOL and NETLOGON shares.
- Review Directory Services and DNS event logs.
- Take a virtual machine snapshot (or backup) after successful promotion.
- Avoid unnecessary configuration changes immediately after promotion.

---

# Lessons Learned

Domain Controller promotion is a foundational infrastructure change that introduces centralized identity services. Careful validation after promotion ensures that authentication, DNS, and directory services are functioning correctly before additional servers or client devices are integrated into the environment.

---

# Conclusion

The promotion of **DC01** to the organization's first Domain Controller was completed successfully. Active Directory, DNS, and authentication services are now operational, providing the foundation for centralized identity and policy management.
