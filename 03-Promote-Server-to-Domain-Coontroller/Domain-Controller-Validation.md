# Domain Controller Validation

## Executive Summary

Following the successful promotion of **DC01** to a Domain Controller, a comprehensive validation was performed to ensure that Active Directory Domain Services (AD DS), Domain Name System (DNS), and supporting services were functioning correctly.

The purpose of this validation is to confirm that the Domain Controller is healthy, fully operational, and ready to support domain users, computers, Organizational Units (OUs), and Group Policy Objects (GPOs).

---

# Business Justification

Validating a newly promoted Domain Controller is a critical step in any Active Directory deployment.

Failure to verify the deployment may result in:

- Authentication failures
- DNS resolution issues
- Failed domain joins
- Group Policy processing errors
- Active Directory replication issues (in multi-DC environments)

A formal validation ensures the environment is ready for production use before client systems are introduced.

---

# Prerequisites

Before beginning this validation, ensure:

- The server has been successfully promoted to a Domain Controller.
- The server has restarted after promotion.
- You are logged in with a domain administrative account.
- No pending Windows updates or restart requests exist.

---

# Validation Overview

The following components will be validated:

- Active Directory Domain Services
- DNS Server
- SYSVOL and NETLOGON shares
- Active Directory database
- Domain information
- Forest information
- Active Directory Users and Computers
- Group Policy Management
- Event Logs

---

# Step-by-Step Validation

## Step 1 — Verify Successful Domain Logon

Sign in using the domain Administrator account.

Example:

```
Username:
CORP\Administrator
```

or

```
Administrator@corp.local
```

Successful logon confirms that domain authentication is operational.

---

## Step 2 — Verify the Computer Name

Open Windows PowerShell.

Run:

```powershell
hostname
```

Expected Output:

```text
DC01
```

---

## Step 3 — Verify Domain Information

Run:

```powershell
Get-ADDomain
```

Expected output should include information such as:

- DNS Root
- NetBIOS Name
- Domain SID
- Domain Mode
- PDC Emulator
- RID Master
- Infrastructure Master

> **Screenshot Placeholder**
>
> `assets/images/domain-validation/get-addomain.png`

---

## Step 4 — Verify Forest Information

Run:

```powershell
Get-ADForest
```

Confirm:

- Forest Name
- Forest Functional Level
- Global Catalog
- Domains
- Sites

> **Screenshot Placeholder**
>
> `assets/images/domain-validation/get-adforest.png`

---

## Step 5 — Verify Domain Controller Information

Run:

```powershell
Get-ADDomainController
```

Confirm that:

- DC01 appears as the Domain Controller.
- The Global Catalog role is enabled.
- The server is online.

---

## Step 6 — Verify SYSVOL and NETLOGON Shares

Run:

```powershell
net share
```

Expected Output:

```text
NETLOGON
SYSVOL
```

The presence of these shares confirms that Active Directory has successfully initialized.

---

## Step 7 — Verify DNS Service

Run:

```powershell
Get-Service DNS
```

Expected Output:

```text
Status : Running
```

---

## Step 8 — Verify Active Directory Services

Run:

```powershell
Get-Service NTDS
```

Expected Output:

```text
Status : Running
```

---

## Step 9 — Verify DNS Resolution

Run:

```powershell
nslookup corp.local
```

Expected Result:

The domain resolves successfully using the Domain Controller's DNS service.

---

## Step 10 — Verify Active Directory Administrative Tools

Open the following consoles from the Start Menu or Server Manager:

- Active Directory Users and Computers
- Active Directory Administrative Center
- Active Directory Sites and Services
- Active Directory Domains and Trusts
- Group Policy Management

Each console should open without errors.

> **Screenshot Placeholder**
>
> `assets/images/domain-validation/ad-management-tools.png`

---

## Step 11 — Review Event Logs

Open **Event Viewer**.

Navigate to:

```
Applications and Services Logs
    └── Directory Service
```

Review for:

- Errors
- Critical events
- Warnings related to AD DS or DNS

No critical issues should be present.

---

# PowerShell Validation Commands

```powershell
Get-ADDomain

Get-ADForest

Get-ADDomainController

Get-Service NTDS

Get-Service DNS

dcdiag

repadmin /replsummary

net share
```

> **Note:** In a single Domain Controller environment, `repadmin` will return limited replication information because there are no replication partners.

---

# Expected Results

The validation is considered successful if:

- The domain can be queried using PowerShell.
- The forest information is returned successfully.
- SYSVOL and NETLOGON shares are present.
- DNS is operational.
- NTDS service is running.
- Administrative tools open successfully.
- Event Viewer reports no critical AD DS issues.

---

# Troubleshooting

## SYSVOL Not Shared

Possible causes:

- Promotion incomplete
- DFS Replication issue

Verify:

```powershell
net share
```

Review:

- DFS Replication logs
- Directory Service logs

---

## DNS Service Not Running

Verify:

```powershell
Get-Service DNS
```

Restart if necessary:

```powershell
Restart-Service DNS
```

---

## Active Directory Module Not Available

Import the module:

```powershell
Import-Module ActiveDirectory
```

If unavailable, verify that the AD DS management tools are installed.

---

## DCDIAG Reports Errors

Run:

```powershell
dcdiag /v
```

Review the report and address any reported issues before joining client computers to the domain.

---

# Best Practices

- Validate every Domain Controller immediately after promotion.
- Review Event Viewer before onboarding client systems.
- Run `dcdiag` after major Active Directory changes.
- Document validation results.
- Take a system backup or virtual machine snapshot after successful validation.

---

# Lessons Learned

A successful Domain Controller promotion does not guarantee a healthy Active Directory environment. Comprehensive validation confirms that authentication, DNS, SYSVOL, and administrative services are functioning correctly before the infrastructure begins serving users and client systems.

---

# Conclusion

The validation confirmed that **DC01** is operating as a healthy Domain Controller. Active Directory Domain Services, DNS, and supporting components are functioning as expected, and the environment is ready for client computers to join the domain.
