# Managing Security Group Membership

## Executive Summary

Creating Security Groups is only the first step in implementing Role-Based Access Control (RBAC). To make Security Groups useful, administrators must assign the appropriate users to the appropriate groups.

Instead of granting permissions directly to user accounts, permissions are assigned to Security Groups. Users inherit those permissions by becoming members of the appropriate group.

This document demonstrates how to add users to Security Groups, remove users from groups, verify group membership, and manage memberships using both **Active Directory Users and Computers (ADUC)** and **Windows PowerShell**.

---

# Business Justification

Managing permissions through Security Groups simplifies administration and improves security.

Instead of changing permissions every time an employee joins, transfers departments, or leaves the organization, administrators simply update group membership.

Benefits include:

- Simplified permission management.
- Reduced administrative overhead.
- Easier onboarding and offboarding.
- Consistent access control.
- Improved auditing.
- Support for Role-Based Access Control (RBAC).

---

# Prerequisites

Before beginning:

- Active Directory Domain Services is operational.
- User accounts have already been created.
- Security Groups have already been created.
- You are signed in with a Domain Administrator account or delegated administrator account.
- Active Directory Users and Computers is available.

---

# Environment

| Component | Value |
|-----------|-------|
| Domain Controller | DC01 |
| Domain | corp.local |
| Tool | Active Directory Users and Computers |

---

# Planned Memberships

The following users will be added to Security Groups.

| User | Department | Security Group |
|------|------------|----------------|
| John Doe | IT | IT_Admins |
| Alice Smith | Finance | Finance_Users |
| Brian Johnson | Human Resources | HR_Users |
| Chloe Wilson | Sales | Sales_Users |
| David Martin | Marketing | Marketing_Users |

---

# Step-by-Step Implementation

## Step 1 — Open Active Directory Users and Computers

1. Sign in to **DC01**.
2. Open **Server Manager**.
3. Click **Tools**.
4. Select **Active Directory Users and Computers**.
5. Expand your domain (`corp.local`).
6. Expand the **Security Groups** Organizational Unit.

You should now see all previously created Security Groups.

---

# Adding Members to a Security Group

## Step 2 — Open the IT_Admins Group

1. Locate the **IT_Admins** Security Group.
2. Double-click **IT_Admins**.

Alternatively:

- Right-click **IT_Admins**
- Select **Properties**

The Group Properties window opens.

---

## Step 3 — Open the Members Tab

1. Click the **Members** tab.
2. Review the current member list.

If this is a new deployment, the list should be empty.

---

## Step 4 — Add John Doe

1. Click **Add**.
2. The **Select Users, Contacts, Computers, Service Accounts, or Groups** window opens.
3. In the **Enter the object names to select** box, type:

```text
John Doe
```

or

```text
john.doe
```

4. Click **Check Names**.

The name should underline, indicating Active Directory successfully located the account.

5. Click **OK**.

The user now appears in the Members list.

6. Click **Apply**.

7. Click **OK**.

John Doe is now a member of the **IT_Admins** Security Group.

---

## Step 5 — Add Alice Smith

Repeat the previous process.

Open:

```
Finance_Users
```

Add:

```
Alice Smith
```

Click:

- Apply
- OK

---

## Step 6 — Add Brian Johnson

Open:

```
HR_Users
```

Add:

```
Brian Johnson
```

Save the changes.

---

## Step 7 — Add Chloe Wilson

Open:

```
Sales_Users
```

Add:

```
Chloe Wilson
```

Click **Apply**.

Click **OK**.

---

## Step 8 — Add David Martin

Open:

```
Marketing_Users
```

Add:

```
David Martin
```

Save the changes.

---

# Verify Membership

## Step 9 — Verify from the Group

1. Open the **IT_Admins** group.
2. Select the **Members** tab.
3. Confirm that **John Doe** appears in the list.

Repeat for each Security Group.

---

## Step 10 — Verify from the User

Instead of checking the group, you can verify membership from the user account.

1. Navigate to the user's Organizational Unit.

Example:

```
IT
```

2. Locate:

```
John Doe
```

3. Right-click the account.

4. Select **Properties**.

5. Open the **Member Of** tab.

You should see:

```
IT_Admins
```

Repeat for the remaining users.

---

# Removing a User from a Group

Occasionally users change departments or no longer require access.

To remove a member:

1. Open the Security Group.
2. Open the **Members** tab.
3. Select the user.
4. Click **Remove**.
5. Click **Apply**.
6. Click **OK**.

The user immediately loses permissions granted through that Security Group once access tokens are refreshed (typically after the next sign-in).

---

# PowerShell Equivalent

Import the Active Directory module if required:

```powershell
Import-Module ActiveDirectory
```

Add members:

```powershell
Add-ADGroupMember `
-Identity "IT_Admins" `
-Members "john.doe"

Add-ADGroupMember `
-Identity "Finance_Users" `
-Members "alice.smith"

Add-ADGroupMember `
-Identity "HR_Users" `
-Members "brian.johnson"

Add-ADGroupMember `
-Identity "Sales_Users" `
-Members "chloe.wilson"

Add-ADGroupMember `
-Identity "Marketing_Users" `
-Members "david.martin"
```

---

# View Group Members

```powershell
Get-ADGroupMember `
-Identity "IT_Admins"
```

View all members:

```powershell
Get-ADGroupMember "Finance_Users"
```

---

# Remove a Member

```powershell
Remove-ADGroupMember `
-Identity "IT_Admins" `
-Members "john.doe"
```

You will be prompted for confirmation.

To suppress confirmation:

```powershell
Remove-ADGroupMember `
-Identity "IT_Admins" `
-Members "john.doe" `
-Confirm:$false
```

---

# Verification

Run:

```powershell
Get-ADPrincipalGroupMembership `
-Identity "john.doe"
```

Expected output:

```text
IT_Admins
Domain Users
```

Repeat for each user account.

---

# Expected Results

After completing this exercise:

- Every user belongs to the correct Security Group.
- Group memberships accurately reflect departmental roles.
- Permissions can now be assigned to groups instead of individual users.
- The environment is ready for Role-Based Access Control.

---

# Common Mistakes

## Adding Permissions Directly to Users

Always assign permissions to Security Groups rather than individual user accounts whenever possible.

---

## Adding Users to the Wrong Group

Verify the user's department and intended role before assigning group membership.

---

## Forgetting to Remove Old Memberships

When users change departments or responsibilities, remove memberships that are no longer required to maintain least-privilege access.

---

# Troubleshooting

## User Cannot Be Found

- Verify the user account exists.
- Confirm the correct spelling of the username.
- Use **Check Names** in ADUC to validate the object.

---

## User Does Not Receive Expected Access

Check:

- The user is a member of the correct Security Group.
- The user has signed out and back in to refresh the access token.
- Permissions have been assigned to the Security Group.

---

# Best Practices

- Use Security Groups to manage permissions instead of assigning rights directly to users.
- Review group memberships regularly.
- Remove inactive or unnecessary memberships promptly.
- Follow the principle of least privilege.
- Document the purpose of each Security Group.

---

# Lessons Learned

Security Group membership is the core of Role-Based Access Control in Active Directory. Properly managing memberships simplifies administration, improves security, and ensures consistent access to organizational resources.

---

# Conclusion

The users created earlier have now been assigned to their respective Security Groups. This completes the RBAC foundation for the environment and prepares the domain for implementing file permissions, delegated administration, and Group Policy targeting.
