# Client Prerequisites

## Executive Summary

Before joining a Windows client computer to an Active Directory domain, several prerequisite checks were completed to ensure successful communication between the client and the Domain Controller.

The most common cause of domain join failures is incorrect network or DNS configuration. This document outlines the required preparations, verification procedures, and recommended practices before initiating the domain join process.

---

# Business Justification

Preparing client computers before joining them to the domain reduces deployment failures and ensures a consistent onboarding process.

Completing these prerequisite checks helps to:

- Reduce troubleshooting time.
- Ensure reliable communication with the Domain Controller.
- Prevent authentication issues.
- Validate DNS functionality.
- Confirm client readiness.

---

# Environment

| Component | Value |
|----------|-------|
| Domain Controller | DC01 |
| Client Computer | CLIENT01 |
| Operating System | Windows 11 Pro *(or Windows 10 Pro)* |
| Domain | corp.local |
| DNS Server | 192.168.1.10 |

> **Note:** Windows Home editions cannot join an Active Directory domain.

---

# Prerequisites Checklist

Before joining the client to the domain, verify:

- [ ] Windows Pro, Enterprise, or Education edition installed
- [ ] Client connected to the network
- [ ] Client can reach the Domain Controller
- [ ] Correct DNS server configured
- [ ] Date and time synchronized
- [ ] Administrative privileges available
- [ ] Domain Administrator credentials available

---

# Step-by-Step Preparation

## Step 1 — Verify Windows Edition

Open:

```
Settings
    └── System
          └── About
```

Review the Windows edition.

Supported editions include:

- Windows 10 Pro
- Windows 10 Enterprise
- Windows 11 Pro
- Windows 11 Enterprise
- Windows Education


---

## Step 2 — Verify Network Connectivity

Open PowerShell.

Run:

```powershell
ipconfig
```

Confirm:

- IPv4 Address
- Subnet Mask
- Default Gateway

The client should be on the same network (or have routed connectivity) as the Domain Controller.

---

## Step 3 — Configure DNS

Open:

```
Control Panel
    └── Network and Internet
          └── Network Connections
```

Right-click the active network adapter.

Select:

```
Properties
```

Open:

```
Internet Protocol Version 4 (TCP/IPv4)
```

Configure the Preferred DNS Server to point to the Domain Controller.

Example:

```
Preferred DNS Server

192.168.1.10
```

Click **OK**.


## Step 4 — Test Connectivity

Run:

```powershell
ping DC01
```

Then:

```powershell
ping 192.168.1.10
```

Expected Result:

Successful replies from the Domain Controller.

---

## Step 5 — Verify DNS Resolution

Run:

```powershell
nslookup corp.local
```

Expected Result:

The domain resolves successfully using the configured DNS server.

---

## Step 6 — Verify Communication with the Domain Controller

Run:

```powershell
Test-NetConnection DC01 -Port 53
```

Then:

```powershell
Test-NetConnection DC01 -Port 88
```

Then:

```powershell
Test-NetConnection DC01 -Port 389
```

Expected Result:

```text
TcpTestSucceeded : True
```

These ports verify access to:

| Port | Service |
|------|----------|
| 53 | DNS |
| 88 | Kerberos |
| 389 | LDAP |

---

## Step 7 — Verify Date and Time

Run:

```powershell
Get-Date
```

Compare the client time with the Domain Controller.

Kerberos authentication requires clocks to remain closely synchronized.

---

## Step 8 — Verify Domain Credentials

Confirm that you have credentials with permission to join computers to the domain.

Example:

```
CORP\Administrator
```

or

```
Administrator@corp.local
```

---

# PowerShell Summary

Useful commands for verification:

```powershell
ipconfig /all

ping DC01

nslookup corp.local

Test-NetConnection DC01 -Port 53

Test-NetConnection DC01 -Port 88

Test-NetConnection DC01 -Port 389

Get-Date
```

---

# Expected Results

The client is ready for domain integration when:

- Network connectivity is confirmed.
- DNS points to the Domain Controller.
- The domain resolves successfully.
- Required service ports are reachable.
- Date and time are synchronized.
- Domain credentials are available.

---

# Troubleshooting

## Unable to Resolve the Domain

Verify DNS configuration:

```powershell
ipconfig /all
```

Ensure the Preferred DNS Server is set to the Domain Controller.

---

## Cannot Ping DC01

Verify:

- Network connectivity
- Firewall rules
- IP addressing
- Routing (if on different subnets)

---

## Port Tests Fail

Confirm that:

- DNS Server service is running.
- Active Directory Domain Services is operational.
- Windows Firewall allows the required traffic.

---

## Time Difference Too Large

Synchronize the client with the Domain Controller before attempting the domain join.

---

# Best Practices

- Always use the Domain Controller as the preferred DNS server.
- Verify connectivity before joining the domain.
- Ensure the Windows edition supports domain membership.
- Synchronize system time before authentication.
- Document network settings for future troubleshooting.

---

# Lessons Learned

Most domain join failures are caused by prerequisite issues rather than problems with Active Directory itself. Performing a structured validation before joining the client significantly improves deployment success and reduces troubleshooting effort.

---

# Conclusion

The client computer has been successfully prepared for domain integration. Network connectivity, DNS configuration, and authentication prerequisites have been validated, and the system is ready to join the Active Directory domain.


