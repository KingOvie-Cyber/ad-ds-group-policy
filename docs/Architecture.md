# Active Directory Architecture

## Overview

This document describes the architectural design of the Active Directory Domain Services (AD DS) environment implemented throughout this project. It provides a high-level view of the infrastructure, including the logical structure of the domain, server roles, authentication process, and supporting services.

The environment is designed as a single-forest, single-domain deployment, making it suitable for small to medium-sized organizations while following Microsoft best practices.

---

# Architecture Goals

The primary goals of this architecture are to:

- Centralize identity and access management.
- Simplify user and computer administration.
- Provide centralized authentication and authorization.
- Enable centralized policy management through Group Policy.
- Support scalability for future infrastructure growth.
- Provide a secure and manageable Windows domain environment.

---

# Architecture Overview

This deployment consists of:

- One Active Directory Forest
- One Active Directory Domain
- One Domain Controller
- Integrated DNS Server
- Windows Client Computers
- Organizational Units (OUs)
- Security Groups
- Group Policy Objects (GPOs)

---

# Logical Architecture

```text
                        Active Directory Forest
                              corp.local
                                   │
                     ┌─────────────┴─────────────┐
                     │                           │
               Active Directory Domain      DNS Zone
                     │
              Domain Controller (DC01)
                     │
      ┌──────────────┼──────────────┐
      │              │              │
  Users          Computers      Groups
      │
      ├── IT
      ├── HR
      ├── Finance
      └── Service Accounts
```

---

# Infrastructure Components

| Component | Description |
|-----------|-------------|
| Forest | Top-level security boundary for Active Directory. |
| Domain | Administrative and authentication boundary. |
| Domain Controller | Authenticates users and stores the Active Directory database. |
| DNS Server | Resolves names and enables Active Directory functionality. |
| Client Computers | Domain-joined Windows devices. |
| Organizational Units | Logical containers for users, computers, and groups. |
| Group Policy | Centralized configuration and security management. |

---

# Active Directory Forest

A forest is the highest-level container within Active Directory.

This deployment uses:

| Setting | Value |
|---------|-------|
| Forest Name | corp.local |
| Number of Forests | 1 |
| Forest Functional Level | Windows Server 2022 |

The forest contains a single domain and provides a shared configuration, schema, and global catalog.

---

# Active Directory Domain

The domain serves as the primary administrative boundary.

| Property | Value |
|----------|-------|
| Domain Name | corp.local |
| NetBIOS Name | CORP |
| Domain Type | Single Domain |
| Authentication | Kerberos |
| Directory Service | Active Directory Domain Services |

---

# Domain Controller

The Domain Controller hosts Active Directory Domain Services and provides centralized authentication and directory services.

| Server | Role |
|--------|------|
| DC01 | Primary Domain Controller |

Primary responsibilities include:

- User authentication
- Computer authentication
- Group Policy processing
- DNS services
- Active Directory replication (future expansion)
- Directory database management

---

# DNS Integration

Active Directory relies heavily on DNS for locating domain services.

The DNS server is installed alongside Active Directory and is configured as an Active Directory-integrated zone.

```text
Client
   │
   ▼
DNS Query
   │
   ▼
DNS Server (DC01)
   │
   ▼
Active Directory Services
```

---

# Authentication Flow

When a user signs in to a domain-joined computer, the authentication process follows these steps:

1. The client contacts the configured DNS server.
2. DNS locates the Domain Controller.
3. The client requests authentication.
4. The Domain Controller validates the credentials.
5. Kerberos issues a Ticket Granting Ticket (TGT).
6. Group Policies are processed.
7. The user receives access to authorized resources.

```text
User
 │
 ▼
Client Computer
 │
 ▼
DNS
 │
 ▼
Domain Controller
 │
 ▼
Kerberos Authentication
 │
 ▼
Group Policy Processing
 │
 ▼
Desktop Loaded
```

---

# Organizational Unit Design

To simplify administration, objects are organized into Organizational Units.

```text
CORP.LOCAL
│
├── Users
│
├── Groups
│
├── Computers
│
├── Servers
│
├── Workstations
│
├── IT
│
├── HR
│
├── Finance
│
└── Service Accounts
```

This structure allows administrators to delegate permissions and apply Group Policies to specific departments or object types.

---

# Group Policy Design

Group Policies are linked to Organizational Units rather than the entire domain whenever possible.

Planned Group Policies include:

- Password Policy
- Account Lockout Policy
- USB Storage Restriction
- Desktop Wallpaper Policy
- Windows Firewall Configuration
- Windows Update Configuration
- Disable Control Panel
- Disable Command Prompt
- Drive Mapping
- Logon Scripts

---

# Security Model

The environment follows several security best practices:

- Principle of Least Privilege
- Strong password requirements
- Account lockout policies
- Role-based administration
- Organizational Unit separation
- Centralized policy management
- DNS integrated with Active Directory
- Administrative access limited to authorized users

---

# Scalability

Although this project is built as a single-domain deployment, the architecture supports future expansion, including:

- Additional Domain Controllers
- Read-Only Domain Controllers (RODCs)
- Additional Sites
- Multiple Domains
- Trust Relationships
- Azure AD Connect
- Microsoft Entra ID Integration

---

# Advantages of This Architecture

- Centralized authentication
- Simplified administration
- Improved security
- Centralized policy enforcement
- Scalable infrastructure
- Simplified user management
- Efficient resource access
- Enterprise-ready design

---

# Design Considerations

During deployment, the following considerations were taken into account:

- Static IP addressing for infrastructure servers.
- DNS hosted on the Domain Controller.
- Consistent naming conventions.
- Logical Organizational Unit structure.
- Future scalability.
- Ease of administration.
- Security-first configuration.

---

# Summary

This architecture provides a stable, secure, and scalable foundation for deploying Active Directory Domain Services. By implementing a single-forest, single-domain design with integrated DNS and centralized management, administrators can efficiently manage users, computers, and security policies while preparing the environment for future growth.
