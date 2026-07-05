# Server Renaming

## Executive Summary

As part of the infrastructure preparation process, the server was renamed to align with the organization's naming convention before deploying Active Directory Domain Services (AD DS).

Renaming the server before promoting it to a Domain Controller ensures consistency across the environment and prevents unnecessary complications associated with changing the hostname after Active Directory installation.

The server was renamed from its automatically generated Windows installation name to **DC01**, reflecting its intended role as the organization's first Domain Controller.

---

# Business Justification

A standardized server naming convention provides several operational benefits:

- Simplifies infrastructure management.
- Improves troubleshooting.
- Enhances asset identification.
- Supports documentation consistency.
- Reduces administrative errors.
- Improves monitoring and reporting.

In enterprise environments, descriptive hostnames allow administrators to immediately identify a server's purpose without additional investigation.

---

# Implementation Summary

| Configuration | Value |
|--------------|-------|
| Previous Computer Name | Automatically Generated Windows Hostname |
| New Computer Name | DC01 |
| Server Role | Planned Domain Controller |
| Environment | Windows Server 2022 Standard |

---

# Why Rename Before Installing Active Directory?

Microsoft recommends renaming the server before promoting it to a Domain Controller.

Changing the hostname after Active Directory installation introduces additional complexity because the server becomes tightly integrated with:

- Active Directory
- DNS
- SYSVOL
- Kerberos
- Group Policy
- Replication

Although renaming a Domain Controller is supported in some scenarios, it should be avoided unless absolutely necessary.

---

# Naming Convention

The organization uses the following naming standard.

| Prefix | Purpose |
|---------|----------|
| DC | Domain Controller |
| FS | File Server |
| APP | Application Server |
| DB | Database Server |
| WSUS | Windows Server Update Services |
| WEB | Web Server |

Examples:

| Server | Description |
|---------|-------------|
| DC01 | Primary Domain Controller |
| DC02 | Secondary Domain Controller |
| FS01 | File Server |
| APP01 | Application Server |

---

# Implementation

The server hostname was updated using Windows PowerShell.

```powershell
Rename-Computer -NewName "DC01" -Restart
```

Alternatively, the hostname can be changed using **Server Manager** or **System Properties**.

Following the rename operation, the server was restarted to apply the change.

---

# Verification

After the restart, the following commands were executed.

Verify hostname:

```powershell
hostname
```

Expected Output

```text
DC01
```

Display system information:

```powershell
systeminfo
```

Verify the computer name:

```powershell
Get-ComputerInfo | Select-Object CsName
```

Example Output

```text
CsName
------
DC01
```

---

# Validation Results

The following validation checks were completed successfully.

| Validation | Status |
|------------|--------|
| Server Restarted | ✅ |
| New Hostname Applied | ✅ |
| PowerShell Verification Successful | ✅ |
| Server Manager Updated | ✅ |
| Event Logs Reviewed | ✅ |

---

# Operational Impact

Renaming the server before Active Directory deployment has no negative impact on the environment.

Benefits include:

- Consistent server identification.
- Simplified administration.
- Easier documentation.
- Reduced risk during future infrastructure expansion.

---

# Rollback Considerations

If the server name was configured incorrectly, it can be changed again before Active Directory installation.

Example:

```powershell
Rename-Computer -NewName "SERVER01" -Restart
```

Once the server has been promoted to a Domain Controller, hostname changes should be carefully planned and performed only in accordance with Microsoft's guidance.

---

# Common Issues

## Restart Not Performed

### Symptoms

The previous hostname continues to appear.

### Resolution

Restart the server.

---

## Hostname Not Updated

### Possible Cause

The system restart was interrupted.

### Resolution

Restart the server and verify the hostname again.

---

## PowerShell Access Denied

### Possible Cause

The command was executed without administrative privileges.

### Resolution

Run Windows PowerShell as **Administrator**.

---

# Best Practices

- Establish a naming convention before deployment.
- Rename servers before installing infrastructure roles.
- Avoid generic hostnames.
- Maintain consistent documentation.
- Record all hostname changes.
- Verify the hostname after every restart.

---

# Lessons Learned

A consistent naming convention improves operational efficiency and simplifies long-term infrastructure management.

Implementing descriptive hostnames before deploying Active Directory reduces administrative complexity and aligns with Microsoft's recommended deployment practices.

---

# Conclusion

The server was successfully renamed to **DC01** and verified following the restart.

The infrastructure is now correctly identified and prepared for Active Directory deployment.

