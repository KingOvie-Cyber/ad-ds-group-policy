# Join a Windows Client to the Active Directory Domain Using PowerShell

## Executive Summary

Although joining a Windows computer to an Active Directory domain using the graphical interface is straightforward, Windows PowerShell provides a faster and more scalable method for domain integration. This approach is particularly useful when deploying multiple client computers, automating installations, or managing systems remotely.

This document demonstrates how to join a Windows client computer to an Active Directory domain using Windows PowerShell, verify the operation, and confirm that the trust relationship has been successfully established.

---

# Business Justification

Using PowerShell to join computers to a domain offers several advantages:

- Supports automation and scripting.
- Reduces manual configuration.
- Ensures consistency across multiple deployments.
- Enables remote administration.
- Integrates well with enterprise deployment tools such as Microsoft Deployment Toolkit (MDT), Windows Autopilot, and Configuration Manager.

---

# Prerequisites

Before proceeding, ensure that:

- The Domain Controller is online and operational.
- Active Directory Domain Services (AD DS) is functioning correctly.
- The client computer can communicate with the Domain Controller.
- DNS is configured to point to the Domain Controller.
- You have a domain account with permission to join computers to the domain.
- Windows PowerShell is opened with administrative privileges.

---

# Environment

| Component | Value |
|----------|-------|
| Domain Controller | DC01 |
| Client Computer | CLIENT01 |
| Domain | corp.local |
| Operating System | Windows 11 Pro |
| PowerShell Version | 5.1 or later |

---

# Step-by-Step Implementation

## Step 1 — Open Windows PowerShell as Administrator

1. Click the **Start** button.
2. Search for **PowerShell**.
3. Right-click **Windows PowerShell**.
4. Select **Run as administrator**.
5. If prompted by User Account Control (UAC), click **Yes**.

You should now have an elevated PowerShell session.

---

## Step 2 — Verify the Current Computer Name

Run the following command:

```powershell
hostname
```

Example output:

```text
CLIENT01
```

Confirm that the computer name matches your intended hostname before joining the domain.

---

## Step 3 — Verify Current Domain Membership

Run:

```powershell
(Get-CimInstance Win32_ComputerSystem).Domain
```

If the computer has not yet joined a domain, the output typically displays the local workgroup name (for example, `WORKGROUP`).

---

## Step 4 — Join the Computer to the Domain

Run the following command:

```powershell
Add-Computer `
    -DomainName "corp.local" `
    -Credential "CORP\Administrator"
```

After executing the command:

1. A credential window appears.
2. Enter the password for the specified domain account.
3. Click **OK**.

PowerShell attempts to establish communication with the Domain Controller and create the computer object in Active Directory.

---

## Step 5 — Restart the Computer

Once the command completes successfully, restart the computer by running:

```powershell
Restart-Computer
```

Alternatively, combine both operations into a single command:

```powershell
Add-Computer `
    -DomainName "corp.local" `
    -Credential "CORP\Administrator" `
    -Restart
```

The computer will automatically restart after successfully joining the domain.

---

## Step 6 — Sign In Using a Domain Account

After the restart:

1. Wait for the Windows sign-in screen.
2. Select **Other User** if necessary.
3. Enter a domain account, for example:

```text
CORP\Administrator
```

or

```text
Administrator@corp.local
```

4. Enter the corresponding password.
5. Sign in.

A successful logon confirms that domain authentication is working.

---

# Verification

## Verify Domain Membership

Run:

```powershell
systeminfo | findstr /B /C:"Domain"
```

Expected output:

```text
Domain: corp.local
```

---

## Verify Computer Information

Run:

```powershell
Get-ComputerInfo | Select-Object CsName, CsDomain
```

Example output:

```text
CsName   : CLIENT01
CsDomain : corp.local
```

---

## Verify Secure Channel

Run:

```powershell
Test-ComputerSecureChannel
```

Expected output:

```powershell
True
```

A value of `True` confirms that the trust relationship between the client and the Domain Controller is healthy.

---

## Verify Current User

Run:

```powershell
whoami
```

Example output:

```text
corp\administrator
```

---

# Expected Results

After completing this procedure:

- The client computer is a member of the Active Directory domain.
- A corresponding computer object exists in Active Directory.
- Domain users can authenticate successfully.
- The secure channel between the client and Domain Controller is operational.
- The client is ready to receive Group Policy Objects (GPOs).

---

# Common Mistakes

## Running PowerShell Without Administrative Privileges

The `Add-Computer` command requires an elevated PowerShell session.

---

## Incorrect DNS Configuration

The client must use the Domain Controller as its DNS server. Using an external DNS server will typically prevent the domain from being located.

---

## Using Invalid Credentials

Ensure that the specified account has permission to join computers to the domain.

---

## Forgetting to Restart

The domain join process is not fully completed until the client has been restarted.

---

# Troubleshooting

## "The specified domain either does not exist or could not be contacted."

Check:

```powershell
nslookup corp.local
```

Verify that the client is using the Domain Controller for DNS resolution.

---

## "Access is denied."

Confirm that:

- The credentials are correct.
- The account has permission to join computers to the domain.
- The account is not locked or disabled.

---

## Secure Channel Test Returns False

Run:

```powershell
Test-ComputerSecureChannel -Repair -Credential CORP\Administrator
```

This attempts to repair the trust relationship without removing and rejoining the computer to the domain.

---

# Best Practices

- Verify DNS before attempting a domain join.
- Use descriptive computer names that follow your organization's naming convention.
- Restart the computer immediately after joining the domain.
- Confirm successful authentication using a domain account.
- Verify the secure channel before deploying additional software or policies.

---

# Lessons Learned

PowerShell provides a reliable and efficient method for joining Windows clients to an Active Directory domain. It is particularly valuable in enterprise environments where automation, consistency, and scalability are essential.

---

# Conclusion

The Windows client was successfully joined to the **corp.local** Active Directory domain using Windows PowerShell. The computer established a secure trust relationship with the Domain Controller and is now ready for centralized authentication, policy enforcement, and domain-based administration.

