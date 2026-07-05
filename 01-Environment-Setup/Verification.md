# Environment Verification

## Executive Summary

Before installing Active Directory Domain Services (AD DS), a comprehensive verification of the Windows Server environment was performed to confirm that all prerequisite configurations had been successfully completed.

This validation serves as a quality assurance checkpoint, ensuring that the server is fully prepared for its transition from a standalone Windows Server to a Domain Controller.

---

# Business Justification

Domain Controllers provide core authentication and directory services for an organization. Any configuration issues introduced prior to promotion can impact authentication, DNS resolution, Group Policy processing, and overall infrastructure stability.

Conducting a pre-deployment verification minimizes implementation risk, reduces troubleshooting time, and aligns with enterprise change management and operational best practices.

---

# Validation Scope

The following components were validated during this phase:

- Operating System Health
- Computer Name
- Windows Updates
- Static IP Configuration
- DNS Configuration
- Network Connectivity
- Administrative Access
- Available Storage
- Event Logs
- Windows Features

---

# Environment Information

| Property | Value |
|----------|-------|
| Operating System | Windows Server 2022 Standard |
| Computer Name | DC01 |
| Current Role | Standalone Server |
| Planned Role | Domain Controller |
| Domain | Not Yet Configured |

---

# Verification Procedures

## Verify Computer Name

The hostname was confirmed to ensure it matches the organization's naming convention.

```powershell
hostname
```

Expected Output:

```text
DC01
```

---

## Verify Operating System

Operating system information was reviewed.

```powershell
Get-ComputerInfo
```

The following values were confirmed:

- Windows Server Edition
- Build Number
- Installed Memory
- System Manufacturer
- BIOS Version

---

## Verify Network Configuration

The server's IP configuration was reviewed.

```powershell
ipconfig /all
```

Validation included:

- Static IPv4 Address
- Correct Subnet Mask
- Default Gateway
- Preferred DNS Server

---

## Verify Network Adapter

```powershell
Get-NetAdapter
```

Validation confirmed:

- Adapter Status: Up
- Link Speed
- Interface Availability

---

## Verify DNS Configuration

```powershell
Get-DnsClientServerAddress
```

The configured DNS server was reviewed to ensure it matched the planned deployment design.

---

## Verify Network Connectivity

Connectivity to the gateway was confirmed.

```powershell
Test-NetConnection 192.168.1.1
```

Expected Output:

```text
TcpTestSucceeded : True
```

---

## Verify Installed Updates

Installed updates were reviewed.

```powershell
Get-HotFix
```

Validation confirmed that the latest available security and quality updates had been successfully applied.

---

## Verify Storage

Available storage capacity was confirmed.

```powershell
Get-Volume
```

Adequate free space was available for:

- Active Directory Database (NTDS)
- SYSVOL
- DNS
- Future Windows Updates
- Event Logs

---

## Review Event Logs

Windows Event Viewer was reviewed to identify any critical or recurring system errors.

The following logs were inspected:

- System
- Application
- Security

No critical events requiring remediation were identified.

---

## Verify Administrative Access

Administrative access was confirmed using PowerShell.

```powershell
whoami
```

Expected Output:

```text
dc01\administrator
```

---

## Verify Installed Roles

The server was reviewed to confirm that Active Directory Domain Services had **not** yet been installed.

```powershell
Get-WindowsFeature
```

This established a clean baseline before role deployment.

---

# Validation Results

| Validation Item | Status |
|-----------------|--------|
| Windows Installation | ✅ Passed |
| Computer Name | ✅ Passed |
| Static IP Configuration | ✅ Passed |
| DNS Configuration | ✅ Passed |
| Network Connectivity | ✅ Passed |
| Windows Updates | ✅ Passed |
| Administrative Access | ✅ Passed |
| Storage Availability | ✅ Passed |
| Event Log Review | ✅ Passed |
| Environment Ready for AD DS | ✅ Passed |

---

# Risk Assessment

| Risk | Mitigation |
|------|------------|
| Incorrect Hostname | Verified prior to deployment |
| Dynamic IP Address | Static configuration implemented |
| DNS Misconfiguration | DNS configuration reviewed and validated |
| Pending Updates | Operating system fully updated |
| Hardware Issues | Initial health assessment completed |

Overall implementation risk at this stage was assessed as **Low**.

---

# Operational Readiness

Based on the validation results, the server met all documented prerequisites for the installation of Active Directory Domain Services.

No blocking issues or configuration inconsistencies were identified.

The environment was approved to proceed with Domain Controller deployment.

---

# Best Practices Applied

- Verified every prerequisite before role installation.
- Confirmed operating system health.
- Validated network configuration.
- Reviewed event logs.
- Documented implementation results.
- Established a clean deployment baseline.

---

# Lessons Learned

Completing a formal validation before installing infrastructure roles significantly reduces deployment risk and provides confidence that subsequent configuration issues are not caused by underlying operating system or network misconfigurations.

---

# Conclusion

The Environment Setup phase was successfully completed and validated.

The Windows Server is fully prepared for the installation of Active Directory Domain Services and promotion to a Domain Controller.

All prerequisite checks passed successfully.

---

# Phase Completion

**Environment Setup:** ✅ Completed
