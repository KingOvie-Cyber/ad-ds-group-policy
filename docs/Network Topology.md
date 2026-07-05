# Network Topology

## Overview

This document describes the network topology used throughout this Active Directory Domain Services (AD DS) deployment. The network is designed to provide reliable communication between the Domain Controller and domain-joined client computers while ensuring proper name resolution through DNS.

The topology represents a typical small enterprise deployment consisting of a single Domain Controller, an internal LAN, and Windows client computers.

---

# Objectives

The objectives of this network design are to:

- Provide reliable communication between servers and clients.
- Support Active Directory Domain Services.
- Support DNS name resolution.
- Enable centralized authentication.
- Allow future infrastructure expansion.
- Maintain a simple and manageable network architecture.

---

# Network Overview

| Component | Description |
|----------|-------------|
| Network Type | Internal LAN |
| Domain | corp.local |
| Domain Controller | DC01 |
| DNS Server | DC01 |
| DHCP | Optional (Static IP recommended for DC) |
| Client Systems | Windows 10 Pro / Windows 11 Pro |

---

# Logical Network Diagram

```text
                           Internet
                               │
                               │
                         Router / Gateway
                         192.168.1.1
                               │
                 ───────────────────────────
                               │
                          Internal Network
                      192.168.1.0/24
                               │
         ┌─────────────────────┼─────────────────────┐
         │                     │                     │
         │                     │                     │
      DC01                 CLIENT01             CLIENT02
192.168.1.10           192.168.1.20         192.168.1.21
 Domain Controller       Windows 10          Windows 11
 DNS Server
```

---

# IP Addressing Scheme

| Device | Hostname | IP Address | Role |
|---------|----------|------------|------|
| Router | Gateway | 192.168.1.1 | Default Gateway |
| Server | DC01 | 192.168.1.10 | Domain Controller / DNS |
| Client | CLIENT01 | 192.168.1.20 | Domain Workstation |
| Client | CLIENT02 | 192.168.1.21 | Domain Workstation |

---

# Network Configuration

## Domain Controller

| Setting | Value |
|----------|-------|
| IP Address | 192.168.1.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |
| Preferred DNS | 192.168.1.10 |

---

## Client Configuration

| Setting | Value |
|----------|-------|
| IP Address | DHCP or Static |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |
| Preferred DNS | 192.168.1.10 |

> **Important:** Domain clients must use the Domain Controller as their preferred DNS server. Using a public DNS server (such as 8.8.8.8) will prevent the client from locating the domain.

---

# Communication Flow

The following illustrates how authentication occurs within the environment.

```text
User
 │
 ▼
Client Computer
 │
 ▼
DNS Query
 │
 ▼
Domain Controller (DC01)
 │
 ▼
Kerberos Authentication
 │
 ▼
Group Policy Processing
 │
 ▼
Access Granted
```

---

# Network Services

| Service | Port | Protocol |
|----------|------|----------|
| DNS | 53 | TCP/UDP |
| Kerberos | 88 | TCP/UDP |
| LDAP | 389 | TCP/UDP |
| LDAPS | 636 | TCP |
| SMB | 445 | TCP |
| RPC Endpoint Mapper | 135 | TCP |
| Global Catalog | 3268 | TCP |
| Global Catalog SSL | 3269 | TCP |

---

# DNS Design

The DNS service is hosted on the Domain Controller and is integrated with Active Directory.

```text
corp.local
│
├── _msdcs
├── DC01
├── CLIENT01
└── CLIENT02
```

Benefits of Active Directory-integrated DNS include:

- Secure dynamic updates
- Automatic service record registration
- Simplified replication
- Improved reliability

---

# Authentication Process

1. The client starts and obtains network connectivity.
2. The client queries DNS for the Domain Controller.
3. DNS returns the address of DC01.
4. The client contacts the Domain Controller.
5. Kerberos validates the user's credentials.
6. Group Policies are downloaded and applied.
7. The user receives access to authorized resources.

---

# Network Verification

Verify the IP configuration.

```powershell
ipconfig /all
```

Verify connectivity to the Domain Controller.

```powershell
ping DC01
```

Verify DNS resolution.

```powershell
nslookup corp.local
```

Display network adapter information.

```powershell
Get-NetIPAddress
```

Verify DNS client configuration.

```powershell
Get-DnsClientServerAddress
```

---

# Security Considerations

The following security practices should be observed:

- Assign a static IP address to all infrastructure servers.
- Restrict administrative access to authorized users.
- Keep Windows Defender Firewall enabled.
- Avoid exposing the Domain Controller directly to the Internet.
- Regularly install Windows security updates.
- Monitor authentication logs for suspicious activity.
- Use strong administrative passwords.
- Limit Remote Desktop access.

---

# Best Practices

- Reserve IP addresses for infrastructure servers.
- Use descriptive hostnames.
- Keep network documentation up to date.
- Ensure all domain clients use the Domain Controller for DNS.
- Test DNS before joining clients to the domain.
- Verify time synchronization across all systems.
- Document all IP address assignments.

---

# Summary

This network topology provides a stable foundation for the Active Directory environment by ensuring reliable communication, centralized authentication, and proper DNS name resolution. Following this design supports efficient administration and aligns with Microsoft best practices for small to medium-sized Windows domain deployments.
