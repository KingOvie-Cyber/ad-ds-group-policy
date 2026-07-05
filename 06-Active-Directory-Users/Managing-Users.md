# Managing Active Directory User Accounts

## Executive Summary

After creating user accounts, administrators are responsible for managing them throughout their lifecycle. This includes updating user information, resetting passwords, enabling and disabling accounts, unlocking locked accounts, moving users between Organizational Units (OUs), and deleting accounts when they are no longer required.

This document provides a practical guide to performing these common administrative tasks using **Active Directory Users and Computers (ADUC)**, along with PowerShell equivalents where appropriate.

---

# Business Justification

Effective user lifecycle management is essential for maintaining a secure and organized Active Directory environment.

Proper administration helps to:

- Maintain accurate user information.
- Enforce password security.
- Prevent unauthorized access.
- Support employee onboarding and offboarding.
- Ensure compliance with organizational policies.
- Reduce administrative overhead.

---

# Prerequisites

Before performing these tasks:

- The Domain Controller is operational.
- Active Directory Domain Services is functioning correctly.
- User accounts already exist.
- You are signed in with a Domain Administrator account or an account with delegated permissions.
- Active Directory Users and Computers is available.

---

# Environment

| Component | Value |
|----------|-------|
| Domain Controller | DC01 |
| Domain | corp.local |
| Management Tool | Active Directory Users and Computers |
| PowerShell Module | ActiveDirectory |

---

# Step-by-Step Implementation

## Task 1 — Modify User Properties

1. Sign in to **DC01**.
2. Open **Server Manager**.
3. Select **Tools** > **Active Directory Users and Computers**.
4. Navigate to the Organizational Unit containing the user account.
5. Locate the user you want to modify.
6. Right-click the user account and select **Properties**.
7. Review and update any required information, such as:
   - First name
   - Last name
   - Display name
   - Description
   - Office
   - Telephone number
   - Email address
   - Department
   - Job title
8. Click **Apply**, then **OK** to save the changes.

---

## Task 2 — Reset a User Password

1. In **Active Directory Users and Computers**, locate the user account.
2. Right-click the account.
3. Select **Reset Password**.
4. Enter a new password that complies with the domain password policy.
5. Confirm the password.
6. If appropriate, select **User must change password at next logon**.
7. Click **OK**.

---

## Task 3 — Disable a User Account

Disabling an account prevents the user from signing in while preserving the account and its associated permissions.

1. Locate the user account.
2. Right-click the account.
3. Select **Disable Account**.
4. Confirm the action if prompted.

A disabled account will display a down-arrow icon in ADUC.

---

## Task 4 — Enable a User Account

1. Locate the disabled user account.
2. Right-click the account.
3. Select **Enable Account**.
4. Confirm the action.

The account is now available for authentication.

---

## Task 5 — Unlock a Locked User Account

If a user's account is locked due to repeated failed sign-in attempts:

1. Open the user's **Properties**.
2. Select the **Account** tab.
3. If the account is locked, select **Unlock account**.
4. Click **Apply**, then **OK**.

> **Note:** The account lockout option is only available when the account is actually locked.

---

## Task 6 — Move a User to a Different Organizational Unit

1. Locate the user account.
2. Right-click the user.
3. Select **Move**.
4. Browse to the destination Organizational Unit.
5. Select the target OU.
6. Click **OK**.

The user account is now located in the new Organizational Unit and will receive any Group Policies linked to that OU after policy processing.

---

## Task 7 — Delete a User Account

Before deleting an account, verify that it is no longer required.

1. Locate the user account.
2. Right-click the account.
3. Select **Delete**.
4. Review the confirmation prompt.
5. Click **Yes** to permanently remove the account.

> **Recommendation:** In production environments, many organizations disable accounts before deleting them to preserve audit history and support recovery if needed.

---

# PowerShell Equivalents

## Reset a Password

```powershell
Set-ADAccountPassword `
-Identity "john.doe" `
-Reset `
-NewPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force)
```

---

## Force Password Change at Next Logon

```powershell
Set-ADUser `
-Identity "john.doe" `
-ChangePasswordAtLogon $true
```

---

## Disable an Account

```powershell
Disable-ADAccount `
-Identity "john.doe"
```

---

## Enable an Account

```powershell
Enable-ADAccount `
-Identity "john.doe"
```

---

## Unlock an Account

```powershell
Unlock-ADAccount `
-Identity "john.doe"
```

---

## Move a User

```powershell
Get-ADUser "john.doe" |
Move-ADObject `
-TargetPath "OU=Finance,DC=corp,DC=local"
```

---

## Delete a User

```powershell
Remove-ADUser `
-Identity "john.doe"
```

---

# Verification

Verify account status:

```powershell
Get-ADUser "john.doe" -Properties Enabled |
Select-Object Name, Enabled
```

Verify account location:

```powershell
Get-ADUser "john.doe" |
Select-Object DistinguishedName
```

Verify user details:

```powershell
Get-ADUser "john.doe" -Properties *
```

---

# Expected Results

After completing these tasks:

- User information has been updated as required.
- Passwords can be reset securely.
- Accounts can be enabled or disabled as needed.
- Locked accounts can be unlocked.
- Users can be moved between Organizational Units.
- Accounts can be removed when no longer required.

---

# Common Mistakes

## Deleting an Account Instead of Disabling It

In many organizations, disabling an account is the preferred first step during offboarding.

---

## Moving Users to the Wrong Organizational Unit

Placing users in the wrong OU can result in incorrect Group Policy application.

---

## Resetting Passwords Without Requiring a Password Change

Where organizational policy permits, require users to change temporary passwords at their next sign-in.

---

## Granting Excessive Administrative Privileges

Only authorized administrators should manage user accounts.

---

# Troubleshooting

## Unable to Modify a User

Verify that:

- You have sufficient permissions.
- The account exists.
- Active Directory replication has completed if changes were made recently in a multi-DC environment.

---

## Unable to Reset a Password

Confirm that the new password complies with the domain password policy.

---

## User Cannot Sign In After Being Enabled

Check:

- Account status.
- Password validity.
- Group Policy restrictions.
- Time synchronization.
- DNS configuration.

---

# Best Practices

- Use Organizational Units to organize user accounts.
- Disable accounts before deleting them when appropriate.
- Document significant account changes.
- Follow the principle of least privilege.
- Review inactive accounts regularly.
- Audit user account management activities.

---

# Lessons Learned

Managing user accounts is a continuous administrative responsibility. Consistent user lifecycle management improves security, supports compliance, and ensures that Active Directory remains accurate and well organized.

---

# Conclusion

The Active Directory user accounts created in the previous section have now been successfully managed through common administrative tasks. These procedures provide the foundation for secure identity management and prepare the environment for implementing security groups and Group Policy Objects.
