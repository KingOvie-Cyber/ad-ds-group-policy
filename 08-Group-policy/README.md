# Group Policy (GPO)

## Executive Summary

Group Policy is one of the most powerful management features available in Microsoft Active Directory. It enables administrators to centrally configure and enforce settings for users and computers across an entire domain without requiring manual configuration on individual devices.

Using Group Policy, administrators can deploy security settings, configure Windows features, restrict access to operating system components, deploy scripts, map network drives, redirect folders, configure Windows Defender, enforce password policies, and much more.

This section provides a comprehensive guide to creating, configuring, linking, managing, and troubleshooting Group Policy Objects (GPOs) within the **corp.local** domain.

Throughout this section, all Group Policy Objects will be linked to Organizational Units (OUs) that were created earlier in this repository to demonstrate real-world enterprise administration.

---

# Business Justification

Managing hundreds or thousands of Windows computers individually is inefficient and difficult to maintain.

Group Policy enables administrators to:

- Standardize workstation configurations.
- Enforce organizational security policies.
- Reduce manual administration.
- Improve regulatory compliance.
- Increase operational efficiency.
- Simplify software and configuration deployment.
- Prevent unauthorized configuration changes.
- Centralize Windows administration.

Without Group Policy, every workstation would require manual configuration, making large environments significantly more difficult to manage.

---

# Objectives

By completing this section, you will learn how to:

- Understand Group Policy architecture.
- Create Group Policy Objects (GPOs).
- Link GPOs to Organizational Units.
- Configure User Configuration settings.
- Configure Computer Configuration settings.
- Enforce password and account policies.
- Configure desktop restrictions.
- Disable Control Panel.
- Prevent Command Prompt access.
- Disable USB storage devices.
- Configure Windows Update policies.
- Configure Windows Defender policies.
- Configure login banners.
- Deploy logon scripts.
- Map network drives.
- Redirect folders.
- Configure software restrictions.
- Force Group Policy updates.
- Verify GPO application.
- Troubleshoot Group Policy issues.

---

# Environment

| Component | Value |
|----------|-------|
| Domain | corp.local |
| Domain Controller | DC01 |
| Client Computer | CLIENT01 |
| Management Tool | Group Policy Management Console (GPMC) |

---

# Group Policies Covered

The following Group Policy Objects will be created throughout this repository.

| Group Policy | Purpose |
|--------------|---------|
| Disable Control Panel | Prevent access to Control Panel and Settings |
| Disable Command Prompt | Prevent users from running Command Prompt |
| Desktop Wallpaper Policy | Configure a corporate desktop wallpaper |
| Password Policy | Enforce password complexity and length |
| Account Lockout Policy | Protect against brute-force attacks |
| Windows Defender Policy | Configure Microsoft Defender settings |
| Windows Update Policy | Manage Windows Update behavior |
| USB Storage Restriction | Block USB storage devices |
| Login Banner | Display an organizational legal notice |
| Folder Redirection | Redirect user folders to a central location |
| Drive Mapping | Automatically map network drives |
| Logon Script | Run scripts when users sign in |

---

# Documents in This Section

| Document | Description |
|----------|-------------|
| Understanding-Group-Policy.md | Introduction to Group Policy architecture |
| Create-First-GPO.md | Creating and linking a Group Policy Object |
| Configure-Password-Policy.md | Password and account lockout policies |
| Configure-Desktop-Restrictions.md | User interface restrictions |
| Configure-Windows-Defender.md | Microsoft Defender policies |
| Configure-Windows-Update.md | Windows Update policies |
| Configure-USB-Restrictions.md | Blocking USB storage devices |
| Configure-Login-Banner.md | Interactive logon messages |
| Configure-Logon-Scripts.md | Running scripts during user logon |
| Configure-Drive-Mapping.md | Mapping shared network drives |
| Configure-Folder-Redirection.md | Redirecting user profile folders |
| Group-Policy-Validation.md | Testing and validating Group Policy |
| Group-Policy-Troubleshooting.md | Diagnosing common Group Policy issues |

---

# Tools Used

The following tools will be used throughout this section:

- Group Policy Management Console (GPMC)
- Active Directory Users and Computers (ADUC)
- Windows PowerShell
- Command Prompt
- Event Viewer
- Resultant Set of Policy (RSoP)
- GPResult
- RSOP.msc
- GPUpdate

---

# Best Practices

- Link GPOs to Organizational Units instead of the domain whenever practical.
- Keep each GPO focused on a specific purpose.
- Test new GPOs in a lab before production deployment.
- Document every policy.
- Avoid unnecessary GPO complexity.
- Use Security Filtering when appropriate.
- Use Group Policy Modeling and Results to validate deployments.
- Regularly review and clean up unused GPOs.

---

# Expected Outcome

By the end of this section:

- Multiple production-style GPOs will be deployed.
- Users and computers will receive centralized configurations.
- Security settings will be enforced consistently.
- Administrative effort will be reduced through automation.
- The Active Directory environment will be easier to manage and secure.

---

# Conclusion

Group Policy is the primary mechanism for centralized Windows administration in an Active Directory environment. Understanding how to design, deploy, validate, and troubleshoot GPOs is an essential skill for system administrators, security engineers, and enterprise IT professionals.

The practical examples throughout this section demonstrate how Group Policy can be used to implement real-world administrative and security controls across a Windows domain.
