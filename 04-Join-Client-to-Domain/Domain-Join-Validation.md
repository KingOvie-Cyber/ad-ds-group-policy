# Domain Join Validation

## Executive Summary

After successfully joining **CLIENT01** to the **corp.local** Active Directory domain, it is important to verify that the domain join completed successfully. Simply receiving a "Welcome to the domain" message is not sufficient; several checks should be performed to ensure the computer can authenticate with the Domain Controller, communicate with Active Directory, and receive Group Policy Objects (GPOs).

This document provides a comprehensive validation procedure to confirm that the domain join was successful and that the client computer is functioning correctly within the Active Directory environment.

---

# Business Justification

Performing a validation after joining a computer to the domain helps ensure that:

- The computer object was successfully created in Active Directory.
- The secure trust relationship between the client and the Domain Controller is healthy.
- DNS is functioning correctly.
- Domain authentication is working.
- Group Policy can be processed successfully.
- The computer is ready for production use.

---

# Prerequisites

Before beginning the validation, ensure that:

- The client computer has been restarted after joining the domain.
- You are signed in with a domain account.
- The Domain Controller is online and operational.
- Network connectivity between the client and the Domain Controller is functioning.

---

# Environment

| Component | Value |
|----------|-------|
| Domain Controller | DC01 |
| Client Computer | CLIENT01 |
| Domain | corp.local |
| Operating System | Windows 11 Pro |

---

# Step-by-Step Validation

## Step 1 — Verify the Computer is Joined to the Domain

1. Open **Windows PowerShell** as Administrator.
2. Run the following command:

```powershell
systeminfo | findstr /B /C:"Domain"
```

3. Verify that the output displays your Active Directory domain.

Example:

```text
Domain: corp.local
```

If the output displays **WORKGROUP**, the computer has not successfully joined the domain.

---

## Step 2 — Verify the Computer Object in Active Directory

1. Sign in to the Domain Controller.
2. Open **Server Manager**.
3. Click **Tools**.
4. Select **Active Directory Users and Computers**.
5. Expand your domain (for example, `corp.local`).
6. Open the **Computers** container, or the Organizational Unit (OU) where the computer account should reside.
7. Confirm that **CLIENT01** appears in the list.

The presence of the computer object confirms that Active Directory recognizes the client.

---

## Step 3 — Verify the Secure Channel

Return to **CLIENT01**.

Run:

```powershell
Test-ComputerSecureChannel
```

Expected output:

```powershell
True
```

A value of `True` confirms that the secure trust relationship between the client and the Domain Controller is functioning correctly.

---

## Step 4 — Verify the Logged-On User

Run:

```powershell
whoami
```

Example output:

```text
corp\administrator
```

This confirms that authentication is occurring through Active Directory rather than a local account.

---

## Step 5 — Verify DNS Resolution

Run:

```powershell
nslookup corp.local
```

The command should successfully resolve the domain using the Domain Controller's DNS service.

You can also verify the Domain Controller hostname:

```powershell
nslookup DC01
```

Both commands should return valid DNS records.

---

## Step 6 — Verify Network Connectivity

Run:

```powershell
ping DC01
```

Then test important Active Directory services:

```powershell
Test-NetConnection DC01 -Port 53
```

```powershell
Test-NetConnection DC01 -Port 88
```

```powershell
Test-NetConnection DC01 -Port 389
```

Expected result:

```text
TcpTestSucceeded : True
```

---

## Step 7 — Verify Group Policy Processing

Run:

```powershell
gpupdate /force
```

The command should complete successfully without errors.

Next, generate a Group Policy report:

```powershell
gpresult /r
```

Review the output to confirm that:

- The computer is receiving computer policies.
- The logged-on user is receiving user policies.

---

## Step 8 — Verify Time Synchronization

Run:

```powershell
w32tm /query /status
```

Confirm that the client is synchronized with the appropriate time source.

Time synchronization is essential because Kerberos authentication depends on accurate system time.

---

## Step 9 — Verify Active Directory Authentication

Lock the computer by pressing:

```
Windows + L
```

Sign back in using a domain account.

Successful authentication confirms that the client is communicating with the Domain Controller.

---

# PowerShell Validation Commands

```powershell
systeminfo | findstr /B /C:"Domain"

Test-ComputerSecureChannel

whoami

nslookup corp.local

nslookup DC01

ping DC01

Test-NetConnection DC01 -Port 53

Test-NetConnection DC01 -Port 88

Test-NetConnection DC01 -Port 389

gpupdate /force

gpresult /r

w32tm /query /status
```

---

# Expected Results

The validation is considered successful when:

- The computer reports membership in the correct domain.
- The computer object exists in Active Directory.
- The secure channel test returns **True**.
- Domain authentication succeeds.
- DNS resolution functions correctly.
- Group Policy updates successfully.
- Network connectivity to the Domain Controller is confirmed.

---

# Common Mistakes

## Logging in with a Local Account

If you sign in using a local account, Group Policy and domain-based authentication will not be tested.

---

## Incorrect DNS Configuration

The client must use the Domain Controller as its preferred DNS server.

---

## Skipping the Restart

The computer must be restarted after joining the domain before validation can be completed.

---

## Ignoring Group Policy Errors

Always investigate any errors returned by `gpupdate` or `gpresult` before considering the deployment complete.

---

# Troubleshooting

## Computer Not Found in Active Directory

Verify that the domain join completed successfully. If necessary, remove the computer from the domain, restart it, and repeat the join process.

---

## Secure Channel Test Returns False

Repair the trust relationship:

```powershell
Test-ComputerSecureChannel -Repair -Credential CORP\Administrator
```

---

## Group Policy Fails to Apply

Run:

```powershell
gpupdate /force
```

If the issue persists, review the **Event Viewer** logs under **Applications and Services Logs > Microsoft > Windows > GroupPolicy**.

---

## DNS Resolution Fails

Run:

```powershell
ipconfig /all
```

Verify that the preferred DNS server is the Domain Controller.

---

# Best Practices

- Always validate a domain join before handing the computer over to a user.
- Confirm the secure channel immediately after the restart.
- Verify that Group Policy is processing successfully.
- Ensure DNS and time synchronization remain healthy.
- Move the computer object into the appropriate OU before applying production Group Policies.

---

# Lessons Learned

Successfully joining a computer to the domain is only the first step. A structured validation process ensures that the client can authenticate, communicate with Active Directory, resolve DNS records, and receive Group Policy Objects, providing confidence that the deployment is ready for production use.

---

# Conclusion

The validation confirmed that **CLIENT01** was successfully integrated into the **corp.local** Active Directory domain. The client established a healthy trust relationship with **DC01**, authenticated using Active Directory, and demonstrated readiness for centralized management.

