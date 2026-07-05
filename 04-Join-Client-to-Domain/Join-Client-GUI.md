# Join a Windows Client to the Active Directory Domain (GUI Method)

## Executive Summary

After validating the client computer's network configuration and confirming communication with the Domain Controller, **CLIENT01** was successfully joined to the **corp.local** Active Directory domain using the Windows graphical interface.

Joining the computer to the domain establishes a secure trust relationship with the Domain Controller, allowing centralized authentication, authorization, and policy management.

Once the process is complete and the client is restarted, domain users can authenticate using their Active Directory credentials, and Group Policy Objects (GPOs) can be applied.

---

# Business Justification

Joining computers to an Active Directory domain enables centralized administration and improves security across the enterprise.

Key benefits include:

- Centralized authentication
- Centralized authorization
- Group Policy enforcement
- Simplified user management
- Improved auditing
- Secure access to domain resources
- Reduced reliance on local user accounts

---

# Prerequisites

Before joining the client, verify that:

- The Domain Controller is operational.
- The client can communicate with the Domain Controller.
- DNS is configured to use the Domain Controller.
- The client supports domain membership (Windows Pro, Enterprise, or Education).
- A domain account with permission to join computers is available.

---

# Environment

| Component | Value |
|----------|-------|
| Domain Controller | DC01 |
| Client Computer | CLIENT01 |
| Operating System | Windows 11 Pro |
| Domain | corp.local |
| DNS Server | 192.168.1.10 |

---

# Step-by-Step Implementation

## Step 1 — Open System Properties

Open:

```
Settings
    └── System
          └── About
```

Click:

```
Rename this PC (Advanced)
```

The **System Properties** window opens.

---

## Step 2 — Open Computer Name Settings

Within the **System Properties** window:

Select the **Computer Name** tab.

Click:

```
Change...
```

---

## Step 3 — Join the Domain

Under **Member of**, select:

```
Domain
```

Enter the domain name:

```
corp.local
```

Click:

```
OK
```

---

## Step 4 — Enter Domain Credentials

When prompted, enter credentials with permission to join computers to the domain.

Example:

```
Username

CORP\Administrator
```

or

```
Administrator@corp.local
```

Enter the password.

Click:

```
OK
```

---

## Step 5 — Confirm Successful Join

If the operation succeeds, Windows displays a message similar to:

```
Welcome to the corp.local domain.
```

Click:

```
OK
```


---

## Step 6 — Restart the Client Computer

Windows prompts for a restart.

Select:

```
Restart Now
```

The restart completes the domain join process.

---

## Step 7 — Sign In Using a Domain Account

After the restart, select **Other User** on the sign-in screen.

Authenticate using a domain account.

Example:

```
CORP\Administrator
```

or

```
Administrator@corp.local
```

Successful authentication confirms that the trust relationship has been established.

---

# PowerShell Equivalent

The client can also be joined to the domain using PowerShell.

```powershell
Add-Computer `
    -DomainName corp.local `
    -Credential CORP\Administrator `
    -Restart
```

> **Note:** The PowerShell method is documented in greater detail in **Join-Client-PowerShell.md**.

---

# Verification

## Verify Domain Membership

Run:

```powershell
systeminfo | findstr /B /C:"Domain"
```

Expected Output:

```text
Domain: corp.local
```

---

## Verify Computer Information

Run:

```powershell
Get-ComputerInfo | Select-Object CsDomain, CsName
```

Expected Output:

```text
CsName    : CLIENT01
CsDomain  : corp.local
```

---

## Verify Secure Channel

Run:

```powershell
Test-ComputerSecureChannel
```

Expected Output:

```text
True
```

---

## Verify Current User

Run:

```powershell
whoami
```

Example Output:

```text
corp\administrator
```

---

# Why This Matters

When a client joins an Active Directory domain:

- A computer object is created in Active Directory.
- A secure trust relationship is established.
- Kerberos becomes the default authentication protocol.
- Group Policy processing becomes available.
- Domain resources can be accessed using centralized credentials.

Without a successful domain join, the client remains isolated and cannot participate in centralized identity management.

---

# Common Mistakes

### Using Public DNS

❌ Configuring the client to use a public DNS server (such as 8.8.8.8) instead of the Domain Controller.

**Result:** The client cannot locate the domain.

---

### Incorrect Time

❌ Client time differs significantly from the Domain Controller.

**Result:** Kerberos authentication fails.

---

### Incorrect Domain Name

❌ Entering:

```
corp
```

instead of:

```
corp.local
```

**Result:** Domain join fails.

---

### Using Insufficient Privileges

❌ Attempting to join the domain with a user account that lacks permission.

**Result:** Access denied.

---

# Troubleshooting

## "The specified domain either does not exist or could not be contacted"

Verify:

- DNS configuration
- Domain Controller availability
- Network connectivity
- Firewall settings

---

## "Access is denied"

Verify:

- Credentials
- User permissions
- Domain account status

---

## "RPC Server is unavailable"

Confirm:

- The Domain Controller is online.
- Required services are running.
- Network communication is functioning correctly.

---

# Best Practices

- Configure DNS before joining the domain.
- Restart the client immediately after the join.
- Verify the secure channel.
- Test domain authentication.
- Move the computer account into the appropriate Organizational Unit (OU) after the join if your organizational structure requires it.
- Record the hostname and deployment date for inventory purposes.

---

# Lessons Learned

A successful domain join depends primarily on correct DNS configuration and reliable communication with the Domain Controller. Completing prerequisite validation before attempting the join significantly reduces deployment failures.

---

# Conclusion

**CLIENT01** was successfully joined to the **corp.local** Active Directory domain. The client established a secure trust relationship with **DC01**, enabling centralized authentication and preparing the system for Group Policy processing and enterprise management.
