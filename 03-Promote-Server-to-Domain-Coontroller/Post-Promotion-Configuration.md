# Post-Promotion Configuration

## Executive Summary

Following the successful promotion of **DC01** to a Domain Controller and completion of validation activities, several post-promotion configuration tasks were performed to establish a stable baseline before onboarding client computers into the domain.

These activities focused on validating core infrastructure services, confirming Active Directory health, reviewing DNS configuration, verifying time synchronization, and documenting the environment for future administration.

---

# Business Justification

Although a Domain Controller becomes operational immediately after promotion, validating and refining the environment before introducing users and client systems reduces operational risk.

This phase ensures that:

- DNS is functioning correctly.
- Time synchronization is properly configured.
- Active Directory administrative tools are operational.
- Default security settings are reviewed.
- The environment is ready to support domain-joined devices.

---

# Prerequisites

Before beginning this phase, ensure:

- DC01 has been successfully promoted to a Domain Controller.
- Domain Controller validation has been completed successfully.
- You are signed in using a Domain Administrator account.

---

# Configuration Overview

The following configuration areas were reviewed:

| Configuration Area | Status |
|--------------------|--------|
| DNS Configuration | ☐ |
| Time Synchronization | ☐ |
| Active Directory Administrative Tools | ☐ |
| Default Organizational Units | ☐ |
| Default Group Policies | ☐ |
| Event Logs | ☐ |
| System Health | ☐ |

---

# Step-by-Step Configuration

## Step 1 — Verify DNS Forwarders

Open **DNS Manager**.

Navigate to:

```
Server Manager
    └── Tools
          └── DNS
```

Right-click **DC01** and select **Properties**.

Open the **Forwarders** tab.

If your environment uses Internet name resolution through an upstream DNS server, configure the appropriate forwarder (for example, your organization's DNS resolver or a trusted public DNS service, if appropriate for your environment).

Click **OK** to save any changes.

> **Screenshot Placeholder**
>
> `assets/images/post-promotion/dns-forwarders.png`

---

## Step 2 — Verify Time Synchronization

Open PowerShell.

Run:

```powershell
w32tm /query /status
```

Review:

- Time Source
- Stratum
- Last Successful Sync

Accurate time synchronization is essential because Kerberos authentication depends on clocks remaining within the permitted time skew.

---

## Step 3 — Verify Active Directory Users and Computers

Open:

```
Server Manager
    └── Tools
          └── Active Directory Users and Computers
```

Confirm that the following default containers are present:

- Builtin
- Computers
- Domain Controllers
- Users

> **Screenshot Placeholder**
>
> `assets/images/post-promotion/aduc-default-containers.png`

---

## Step 4 — Verify Group Policy Management

Open:

```
Server Manager
    └── Tools
          └── Group Policy Management
```

Expand:

```
Forest
    └── Domains
          └── corp.local
```

Verify that the following default GPOs exist:

- Default Domain Policy
- Default Domain Controllers Policy

Do not modify these policies unnecessarily. Create new GPOs for custom configurations whenever possible.

---

## Step 5 — Review Active Directory Sites and Services

Open:

```
Server Manager
    └── Tools
          └── Active Directory Sites and Services
```

Verify:

- Default-First-Site-Name exists.
- DC01 appears under the **Servers** container.

---

## Step 6 — Review Event Viewer

Open:

```
Event Viewer
```

Review:

- Directory Service
- DNS Server
- DFS Replication
- System

Confirm there are no recurring critical errors before joining client computers to the domain.

---

## Step 7 — Create a Recovery Point (Recommended)

If the Domain Controller is hosted in a virtual environment, create a VM snapshot or perform a backup using your organization's approved backup solution before making additional infrastructure changes.

> **Note:** For production environments, rely on supported backup procedures rather than snapshots as a long-term recovery strategy.

---

# PowerShell Verification

Verify time service:

```powershell
w32tm /query /status
```

Verify domain:

```powershell
Get-ADDomain
```

Verify forest:

```powershell
Get-ADForest
```

Verify Domain Controller:

```powershell
Get-ADDomainController
```

List Organizational Units:

```powershell
Get-ADOrganizationalUnit -Filter *
```

---

# Expected Results

The post-promotion configuration is considered successful when:

- DNS configuration has been reviewed.
- Time synchronization is functioning correctly.
- Active Directory administrative consoles open without errors.
- Default containers and policies are present.
- Event logs show no blocking issues.
- The Domain Controller is ready to support client onboarding.

---

# Troubleshooting

## Time Synchronization Issues

Verify the Windows Time service:

```powershell
Get-Service W32Time
```

Restart if required:

```powershell
Restart-Service W32Time
```

---

## DNS Resolution Problems

Verify DNS configuration:

```powershell
Get-DnsClientServerAddress
```

Confirm that the Domain Controller is using the appropriate DNS configuration for your deployment.

---

## Missing Default Policies

Verify Group Policy health:

```powershell
dcdiag /test:sysvolcheck
```

Investigate any reported issues before creating custom Group Policy Objects.

---

# Best Practices

- Validate DNS before onboarding clients.
- Ensure reliable time synchronization.
- Avoid modifying the default GPOs unless necessary.
- Review Event Viewer after significant infrastructure changes.
- Record baseline configurations for future reference.
- Take a full backup before introducing additional services.

---

# Lessons Learned

Completing a post-promotion review establishes a stable operational baseline and helps identify configuration issues before they affect users or domain-joined systems. Investing time in validation at this stage reduces troubleshooting effort later in the deployment lifecycle.

---

# Conclusion

The Domain Controller has been successfully reviewed and configured following promotion. Core services are operational, administrative tools have been validated, and the environment is ready for client computers to join the Active Directory domain.
