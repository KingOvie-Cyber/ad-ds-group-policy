# Create Active Directory User Accounts

## Executive Summary

This document demonstrates how to create user accounts in Active Directory using **Active Directory Users and Computers (ADUC)** and Windows PowerShell.

The user accounts created in this guide represent employees from different departments within the organization. These accounts will be used throughout the remainder of this repository when configuring security groups, applying Group Policy Objects (GPOs), testing authentication, assigning permissions, and demonstrating other Active Directory administration tasks.

---

# Business Justification

User accounts provide the identity that allows individuals to authenticate to the Active Directory domain and access organizational resources.

Creating users within the appropriate Organizational Units improves:

- Administrative organization
- Security management
- Group Policy targeting
- Delegated administration
- Resource access control
- Compliance and auditing

---

# Prerequisites

Before creating user accounts, ensure that:

- The Domain Controller is operational.
- Active Directory Domain Services is functioning correctly.
- Organizational Units have already been created.
- You are signed in using a Domain Administrator account.
- Active Directory Users and Computers (ADUC) is available.

---

# Environment

| Component | Value |
|----------|-------|
| Domain Controller | DC01 |
| Domain | corp.local |
| Management Tool | Active Directory Users and Computers |
| PowerShell Module | ActiveDirectory |

---

# User Accounts to Create

| Full Name | Username | Department | OU |
|------------|-----------|------------|----|
| John Doe | john.doe | IT | IT |
| Alice Smith | alice.smith | Finance | Finance |
| Brian Johnson | brian.johnson | Human Resources | Human Resources |
| Chloe Wilson | chloe.wilson | Sales | Sales |
| David Martin | david.martin | Marketing | Marketing |

---

# Step-by-Step Implementation

## Step 1 — Open Active Directory Users and Computers

1. Sign in to **DC01** using a Domain Administrator account.
2. Click the **Start** button.
3. Open **Server Manager**.
4. Click **Tools** in the upper-right corner.
5. Select **Active Directory Users and Computers**.
6. Expand your domain (`corp.local`) in the left navigation pane.
7. Expand the Organizational Units that were created in the previous section.

---

## Step 2 — Create the IT User

1. Right-click the **IT** Organizational Unit.
2. Select **New** > **User**.
3. Complete the fields:

| Field | Value |
|-------|-------|
| First Name | John |
| Last Name | Doe |
| Full Name | John Doe |
| User Logon Name | john.doe |

4. Click **Next**.
5. Enter a temporary password that complies with your domain's password policy.
6. Select **User must change password at next logon** if this aligns with your organization's onboarding process.
7. Click **Next**.
8. Review the information.
9. Click **Finish**.

The new user account should now appear in the **IT** Organizational Unit.

---

## Step 3 — Create the Finance User

Repeat the same process.

Create the following account:

| Field | Value |
|-------|-------|
| First Name | Alice |
| Last Name | Smith |
| User Logon Name | alice.smith |
| Organizational Unit | Finance |

Click **Finish** after reviewing the information.

---

## Step 4 — Create the Human Resources User

Create a new user with the following information:

| Field | Value |
|-------|-------|
| First Name | Brian |
| Last Name | Johnson |
| User Logon Name | brian.johnson |
| Organizational Unit | Human Resources |

Complete the wizard and create the account.

---

## Step 5 — Create the Sales User

Create:

| Field | Value |
|-------|-------|
| First Name | Chloe |
| Last Name | Wilson |
| User Logon Name | chloe.wilson |
| Organizational Unit | Sales |

Finish the wizard.

---

## Step 6 — Create the Marketing User

Create:

| Field | Value |
|-------|-------|
| First Name | David |
| Last Name | Martin |
| User Logon Name | david.martin |
| Organizational Unit | Marketing |

Click **Finish**.

---

## Step 7 — Verify User Account Creation

Expand each Organizational Unit.

Confirm that the correct user account appears within its respective OU.

Example:

```text
corp.local
│
├── IT
│   └── John Doe
│
├── Finance
│   └── Alice Smith
│
├── Human Resources
│   └── Brian Johnson
│
├── Sales
│   └── Chloe Wilson
│
└── Marketing
    └── David Martin
```

---

# PowerShell Equivalent

Import the Active Directory module if it is not already available:

```powershell
Import-Module ActiveDirectory
```

Create the users:

```powershell
$Password = ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force

New-ADUser -Name "John Doe" -GivenName "John" -Surname "Doe" `
-SamAccountName "john.doe" `
-UserPrincipalName "john.doe@corp.local" `
-Path "OU=IT,DC=corp,DC=local" `
-AccountPassword $Password `
-Enabled $true `
-ChangePasswordAtLogon $true

New-ADUser -Name "Alice Smith" -GivenName "Alice" -Surname "Smith" `
-SamAccountName "alice.smith" `
-UserPrincipalName "alice.smith@corp.local" `
-Path "OU=Finance,DC=corp,DC=local" `
-AccountPassword $Password `
-Enabled $true `
-ChangePasswordAtLogon $true
```

Repeat the same pattern for the remaining users.

> **Note:** In production, avoid embedding passwords directly in scripts. Use secure credential handling or automation tools appropriate for your environment.

---

# Verification

Verify user creation using PowerShell:

```powershell
Get-ADUser -Filter * |
Select-Object Name, SamAccountName
```

To verify users within a specific OU:

```powershell
Get-ADUser -SearchBase "OU=IT,DC=corp,DC=local" -Filter *
```

---

# Expected Results

After completing this exercise:

- All user accounts have been created.
- Each user resides in the correct Organizational Unit.
- Accounts are enabled.
- Temporary passwords have been assigned.
- Users are ready for authentication and Group Policy processing.

---

# Common Mistakes

## Creating Users in the Default Users Container

Always create users in the appropriate Organizational Unit rather than leaving them in the default **Users** container.

---

## Weak or Non-Compliant Passwords

Ensure temporary passwords satisfy the domain's password policy.

---

## Incorrect Organizational Unit

Double-check that each account is created in the intended OU to simplify policy application and administration.

---

# Troubleshooting

## Unable to Create a User

Verify that:

- You have sufficient permissions.
- The Organizational Unit exists.
- The username is unique within the domain.

---

## Password Does Not Meet Complexity Requirements

Use a password that complies with the configured password policy.

---

## PowerShell Command Fails

Confirm that the Active Directory module is installed and imported:

```powershell
Import-Module ActiveDirectory
```

---

# Best Practices

- Use a standardized naming convention.
- Place users in the correct Organizational Unit.
- Require a password change at first logon when appropriate.
- Document account creation.
- Periodically review user accounts for accuracy and necessity.

---

# Lessons Learned

Properly organizing user accounts into Organizational Units establishes a strong foundation for applying Group Policy, assigning permissions, and managing identities efficiently within an Active Directory environment.

---

# Conclusion

The required user accounts have been successfully created and organized within their respective Organizational Units. These accounts will be used throughout the remaining sections of this repository to demonstrate group management, policy application, authentication, and access control.
