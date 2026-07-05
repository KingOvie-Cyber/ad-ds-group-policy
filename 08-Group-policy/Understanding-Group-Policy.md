# Understanding Group Policy

## Executive Summary

Group Policy is a feature of Microsoft Active Directory that allows administrators to centrally manage and configure operating system settings, security settings, applications, and user environments across domain-joined computers.

Instead of configuring each computer individually, administrators create a **Group Policy Object (GPO)** on a Domain Controller and link it to a site, domain, or Organizational Unit (OU). The configured settings are then automatically applied to users and computers that fall within the scope of the policy.

Understanding how Group Policy works is essential before creating and deploying Group Policy Objects. This document introduces the architecture, components, processing order, inheritance, and management concepts that form the foundation of Group Policy administration.

---

# Business Justification

In a small environment with only a few computers, administrators could manually configure every workstation.

However, in enterprise environments containing hundreds or thousands of computers, manual configuration is inefficient, inconsistent, and difficult to maintain.

Group Policy provides centralized administration by allowing organizations to:

- Standardize workstation configurations.
- Enforce security settings.
- Restrict access to Windows features.
- Configure Windows automatically.
- Reduce administrative effort.
- Improve compliance with organizational policies.
- Increase operational efficiency.

---

# What is a Group Policy Object (GPO)?

A **Group Policy Object (GPO)** is a collection of configuration settings that control the behavior of Windows operating systems for users and computers.

A GPO can contain:

- Password policies
- Desktop restrictions
- Windows Defender settings
- Firewall rules
- Software deployment settings
- Windows Update configuration
- Drive mappings
- Folder redirection
- Logon and startup scripts
- Administrative Templates
- Security Options

A single GPO can contain one setting or hundreds of settings.

---

# Components of Group Policy

Every Group Policy Object consists of two primary sections.

## Computer Configuration

Computer Configuration settings apply to the computer itself, regardless of which user signs in.

Examples include:

- Windows Defender
- Firewall configuration
- Windows Update
- Startup scripts
- BitLocker settings
- USB device restrictions
- Security settings

Computer policies are processed when the computer starts.

---

## User Configuration

User Configuration settings apply only to the user account that signs in.

Examples include:

- Desktop wallpaper
- Start Menu restrictions
- Control Panel restrictions
- Folder Redirection
- Drive Mapping
- Logon scripts
- Microsoft Edge policies

User policies are processed when the user signs in.

---

# Group Policy Processing Order (LSDOU)

Windows processes Group Policy in a specific order known as **LSDOU**.

The processing sequence is:

1. **Local Policy**
2. **Site**
3. **Domain**
4. **Organizational Unit (OU)**

If multiple policies configure the same setting, the policy processed last generally takes precedence unless inheritance is blocked or enforcement is used.

Example:

```text
Local Policy
      ↓
Site Policy
      ↓
Domain Policy
      ↓
Parent OU Policy
      ↓
Child OU Policy
```

---

# Group Policy Inheritance

By default, Group Policies linked at higher levels are inherited by lower-level containers.

Example:

```text
corp.local
│
├── Finance
│
├── IT
│
└── Human Resources
```

If a GPO is linked to **corp.local**, it is inherited by:

- Finance
- IT
- Human Resources

unless inheritance is blocked.

---

# Block Inheritance

Block Inheritance prevents lower-level Organizational Units from inheriting policies from parent containers.

This is useful when a department requires different settings from the rest of the organization.

Example:

```text
Finance OU

Block Inheritance Enabled
```

The Finance OU will ignore inherited GPOs unless one of those GPOs has been **Enforced**.

---

# Enforced Policies

An **Enforced** GPO overrides Block Inheritance.

If a GPO is marked as **Enforced**, it continues to apply even when child OUs block inheritance.

Use enforcement sparingly because it overrides normal policy processing and can make troubleshooting more difficult.

---

# Security Filtering

Security Filtering determines which users or groups are allowed to process a Group Policy Object.

Instead of applying a policy to everyone in an Organizational Unit, you can apply it only to members of a specific Security Group.

Example:

```
IT_Admins
```

Only members of the **IT_Admins** Security Group receive the policy.

---

# WMI Filtering

Windows Management Instrumentation (WMI) filters allow administrators to apply Group Policies only when specific conditions are met.

Examples include:

- Windows 11 computers only
- Laptops only
- Computers with more than 8 GB RAM
- Specific operating system versions

WMI filters are useful when a policy should not apply to every computer.

---

# Group Policy Refresh

Group Policy is automatically refreshed at regular intervals.

Typical refresh intervals are:

- Domain Controllers: every 5 minutes.
- Member computers: approximately every 90 minutes with a random offset.

Administrators can also force an immediate refresh.

Example:

```powershell
gpupdate /force
```

---

# Tools Used to Manage Group Policy

The following tools are commonly used when working with Group Policy.

| Tool | Purpose |
|------|---------|
| Group Policy Management (GPMC) | Create and manage GPOs |
| Group Policy Management Editor | Configure policy settings |
| GPUpdate | Refresh Group Policy |
| GPResult | Display applied policies |
| RSOP.msc | View Resultant Set of Policy |
| Event Viewer | Troubleshoot policy processing |

---

# Common Group Policy Commands

Refresh policies:

```powershell
gpupdate /force
```

View applied policies:

```powershell
gpresult /r
```

Generate an HTML report:

```powershell
gpresult /h C:\GPReport.html
```

Open Resultant Set of Policy:

```powershell
rsop.msc
```

---

# Best Practices

- Create separate GPOs for different purposes.
- Avoid placing unrelated settings in the same GPO.
- Link GPOs to Organizational Units instead of the entire domain whenever practical.
- Test new GPOs before deploying them to production.
- Document every Group Policy Object.
- Use Security Filtering when only a subset of users or computers should receive a policy.
- Review unused GPOs periodically.

---

# Common Mistakes

- Linking every GPO to the domain root.
- Combining unrelated settings into a single GPO.
- Using Enforced unnecessarily.
- Forgetting to verify policy application.
- Not documenting policy changes.

---

# Lessons Learned

Group Policy is the primary mechanism for centralized Windows administration in an Active Directory environment. Understanding how policies are processed, inherited, filtered, and applied is essential before configuring individual settings.

---

# Conclusion

You should now understand the fundamental concepts of Group Policy, including its architecture, processing order, inheritance, security filtering, and management tools. This knowledge provides the foundation for creating and deploying Group Policy Objects throughout the remainder of this repository.
