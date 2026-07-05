# Network Configuration

## Overview

A reliable network configuration is one of the most critical prerequisites for deploying Active Directory Domain Services (AD DS). Since Active Directory relies heavily on DNS for service discovery and authentication, proper IP addressing and DNS configuration are essential for a stable domain environment.

As part of this implementation, the server was configured with a static IPv4 address to ensure consistent communication with domain clients and other network services.

---

# Purpose

The objectives of this phase were to:

- Configure a static IPv4 address.
- Configure the preferred DNS server.
- Verify network connectivity.
- Validate network adapter configuration.
- Ensure the server was ready for Active Directory deployment.

---

# Network Configuration Summary

| Setting | Value |
|----------|-------|
| Computer Name | DC01 |
| IP Address | 192.168.1.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |
| Preferred DNS Server | 192.168.1.10* |

> **Note**
>
> Prior to installing Active Directory, the Preferred DNS Server may temporarily point to another internal DNS server or the router if one exists. After promoting the server to a Domain Controller, the Preferred DNS Server should be configured to point to the Domain Controller itself.

---

# Why a Static IP Address?

Domain Controllers should always use static IP addresses.

Using DHCP on a Domain Controller may result in:

- Changes to the server's IP address.
- DNS registration issues.
- Authentication failures.
- Client connectivity problems.
- Group Policy processing failures.

A static IP ensures that clients can consistently locate the Domain Controller and associated services.

---

# Network Adapter Configuration

The server's primary network adapter was configured with the following settings:

- IPv4 enabled
- Static IP address assigned
- Default gateway configured
- DNS server configured
- Network profile verified

The adapter status was confirmed to be operational before continuing with the deployment.

---

# DNS Configuration

DNS is a core dependency of Active Directory.

For this implementation:

- DNS resolution was verified.
- Network connectivity was confirmed.
- The DNS client configuration was reviewed.

Following Domain Controller promotion, the server will host the Active Directory-integrated DNS zone.

---

# Verification

The following PowerShell commands were used to verify the network configuration.

## Display IP Configuration

```powershell
ipconfig /all
```

---

## Display Assigned IP Addresses

```powershell
Get-NetIPAddress
```

---

## Display Network Adapter Information

```powershell
Get-NetAdapter
```

---

## Display DNS Client Configuration

```powershell
Get-DnsClientServerAddress
```

---

## Display Network Interface Configuration

```powershell
Get-NetIPConfiguration
```

---

# Connectivity Testing

The following tests were performed to verify network connectivity.

## Test the Default Gateway

```powershell
ping 192.168.1.1
```

Expected Result:

- Successful replies received.
- No packet loss observed.

---

## Verify Local Network Connectivity

```powershell
Test-NetConnection 192.168.1.1
```

Expected Result:

```text
TcpTestSucceeded : True
```

---

## Verify Internet Connectivity (Optional)

```powershell
ping 8.8.8.8
```

This test confirms outbound network connectivity but is not required for Active Directory deployment.

---

## Verify DNS Resolution

```powershell
nslookup microsoft.com
```

Expected Result:

The DNS server successfully resolves external domain names (if Internet access is available).

---

# Network Validation Checklist

The following items were verified before proceeding:

- [x] Network adapter detected.
- [x] Static IPv4 address assigned.
- [x] Correct subnet mask configured.
- [x] Default gateway configured.
- [x] Preferred DNS server configured.
- [x] Successful gateway connectivity.
- [x] Network profile operational.
- [x] PowerShell network tests completed successfully.

---

# Common Issues

## Incorrect IP Address

### Symptoms

- Unable to communicate with other devices.
- Domain services fail after installation.

### Resolution

Verify the configured IP address and subnet mask.

---

## Incorrect DNS Server

### Symptoms

- Clients cannot locate the Domain Controller.
- Domain join operations fail.
- Active Directory installation reports DNS warnings.

### Resolution

Ensure the correct DNS server is configured before promoting the server.

---

## Gateway Unreachable

### Symptoms

- Unable to communicate outside the local subnet.

### Resolution

Verify:

- Gateway address
- Network cable (physical environments)
- Virtual switch configuration (virtual environments)

---

## Network Adapter Disabled

### Symptoms

- No network connectivity.

### Resolution

Verify adapter status.

```powershell
Enable-NetAdapter -Name "Ethernet"
```

---

# Security Considerations

During network configuration, the following security practices were observed:

- Static addressing for infrastructure systems.
- Windows Defender Firewall remained enabled.
- Administrative access limited to authorized users.
- Network configuration documented for future reference.

---

# Best Practices

- Assign static IP addresses to all infrastructure servers.
- Use descriptive hostnames before domain promotion.
- Verify connectivity before installing Active Directory.
- Validate DNS configuration before joining clients.
- Document all IP address assignments.
- Test network connectivity after every major configuration change.

---

# Outcome

At the conclusion of this phase:

- The server was successfully configured with a static IP address.
- Network communication was verified.
- DNS settings were reviewed.
- The environment satisfied the networking requirements for Active Directory Domain Services.

The server was now ready to be renamed (if required) and prepared for Domain Controller promotion.

---

# Summary

Network configuration forms the backbone of a successful Active Directory deployment. By implementing a consistent addressing scheme, validating connectivity, and ensuring proper DNS configuration, the infrastructure is prepared for the installation of Active Directory Domain Services.
