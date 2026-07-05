# Create Active Directory Security Groups

## Executive Summary

Security Groups provide a centralized method of assigning permissions within an Active Directory environment. Rather than assigning permissions directly to individual users, administrators assign permissions to groups and then add users to those groups. This simplifies administration and supports the principle of least privilege.

This guide demonstrates how to create Security Groups using **Active Directory Users and Computers (ADUC)** and Windows PowerShell. The groups created in this document will be used throughout the remainder of this project when configuring file permissions, shared folders, delegated administration, and Group Policy.

---

# Business Justification

As organizations grow, assigning permissions directly to individual users becomes difficult to maintain and increases the risk of inconsistent access control.

Creating Security Groups allows administrators to:

- Simplify permission management.
- Implement Role-Based Access Control (RBAC).
- Reduce administrative effort.
- Improve auditing and compliance.
- Prepare the environment for future expansion.

---

# Prerequisites

Before creating Security Groups, verify that:

- Active Directory Domain Services is installed and operational.
- The Domain Controller (**DC01**) is online.
- The **Security Groups** Organizational Unit has already been created.
- You are signed in with a Domain Administrator account or an account with delegated permissions.
- Active Directory Users and Computers (ADUC) is available.

---

# Environment

| Component | Value |
|----------|-------|
| Domain Controller | DC01 |
| Domain | corp.local |
| Organizational Unit | Security Groups |
| Management Tool | Active Directory Users and Computers |

---

# Security Groups to Create

| Group Name | Group Scope | Group Type |
|------------|-------------|------------|
| IT_Admins | Global | Security |
| Finance_Users | Global | Security |
| HR_Users | Global | Security |
| Sales_Users | Global | Security |
| Marketing_Users | Global | Security |
| Workstation_Admins | Global | Security |
| Server_Admins | Global | Security |

---

# Step-by-Step Implementation

## Step 1 — Open Active Directory Users and Computers

1. Sign in to **DC01** using a Domain Administrator account.
2. Click the **Start** button or press the **Windows** key.
3. Search for **Server Manager** and open it.
4. In the upper-right corner of Server Manager, click **Tools**.
5. From the drop-down menu, select **Active Directory Users and Computers**.
6. Wait for the console to load completely.
7. In the left navigation pane, expand your domain (for example, `corp.local`).
8. Locate and click the **Security Groups** Organizational Unit that was created earlier.

You should now be viewing an empty Organizational Unit where the new Security Groups will be created.

---

## Step 2 — Create the IT_Admins Group

1. Right-click the **Security Groups** Organizational Unit.
2. Select **New**.
3. Click **Group**.
4. The **New Object – Group** window opens.
5. In the **Group name** field, type:

```text
IT_Admins
```

6. Verify that **Group scope** is set to **Global**.
7. Verify that **Group type** is set to **Security**.
8. Review the information to ensure it is correct.
9. Click **OK**.

The **IT_Admins** Security Group should now appear inside the **Security Groups** Organizational Unit.

---

## Step 3 — Create the Finance_Users Group

Repeat the same process.

Enter the following values:

| Setting | Value |
|---------|-------|
| Group Name | Finance_Users |
| Group Scope | Global |
| Group Type | Security |

Click **OK**.

---

## Step 4 — Create the HR_Users Group

Right-click the **Security Groups** OU.

Select:

**New** → **Group**

Configure:

| Setting | Value |
|---------|-------|
| Group Name | HR_Users |
| Group Scope | Global |
| Group Type | Security |

Click **OK**.

---

## Step 5 — Create the Sales_Users Group

Create another Security Group using the following settings:

| Setting | Value |
|---------|-------|
| Group Name | Sales_Users |
| Group Scope | Global |
| Group Type | Security |

Click **OK**.

---

## Step 6 — Create the Marketing_Users Group

Repeat the same procedure.

| Setting | Value |
|---------|-------|
| Group Name | Marketing_Users |
| Group Scope | Global |
| Group Type | Security |

Click **OK**.

---

## Step 7 — Create the Workstation_Admins Group

Create another Security Group.

| Setting | Value |
|---------|-------|
| Group Name | Workstation_Admins |
| Group Scope | Global |
| Group Type | Security |

Click **OK**.

---

## Step 8 — Create the Server_Admins Group

Create the final Security Group.

| Setting | Value |
|---------|-------|
| Group Name | Server_Admins |
| Group Scope | Global |
| Group Type | Security |

Click **OK**.

---

## Step 9 — Verify the Groups

1. Select the **Security Groups** Organizational Unit.
2. Review the list of objects displayed in the right pane.
3. Confirm that all seven Security Groups have been created successfully.

The Organizational Unit should now contain:

```text
Security Groups
│
├── Finance_Users
├── HR_Users
├── IT_Admins
├── Marketing_Users
├── Sales_Users
├── Server_Admins
└── Workstation_Admins
```

---

# PowerShell Equivalent

Open **Windows PowerShell** as Administrator.

If the Active Directory module is not already loaded, run:

```powershell
Import-Module ActiveDirectory
```

Create the groups:

```powershell
$Groups = @(
"IT_Admins",
"Finance_Users",
"HR_Users",
"Sales_Users",
"Marketing_Users",
"Workstation_Admins",
"Server_Admins"
)

foreach ($Group in $Groups)
{
    New-ADGroup `
        -Name $Group `
        -SamAccountName $Group `
        -GroupCategory Security `
        -GroupScope Global `
        -Path "OU=Security Groups,DC=corp,DC=local"
}
```

Verify the groups:

```powershell
Get-ADGroup -SearchBase "OU=Security Groups,DC=corp,DC=local" -Filter * |
Select-Object Name, GroupScope, GroupCategory
```

---

# Verification

Confirm that:

- All Security Groups exist.
- Each group is located in the **Security Groups** Organizational Unit.
- Every group has a **Global** scope.
- Every group is configured as a **Security** group.

---

# Expected Results

After completing this exercise:

- All required Security Groups have been created.
- The groups are organized within the **Security Groups** Organizational Unit.
- The environment is prepared for assigning users to groups and implementing Role-Based Access Control (RBAC).

---

# Common Mistakes

## Selecting the Wrong Group Type

Choosing **Distribution** instead of **Security** prevents the group from being used to assign permissions.

---

## Selecting the Wrong Group Scope

For this single-domain environment, use **Global** groups unless there is a specific requirement for another scope.

---

## Creating Groups Outside the Security Groups OU

Store Security Groups in the designated Organizational Unit to maintain a clean and organized Active Directory structure.

---

# Troubleshooting

## Unable to Create a Group

Verify that:

- You have sufficient permissions.
- The **Security Groups** Organizational Unit exists.
- The group name is unique within the domain.

---

## PowerShell Command Fails

Ensure the Active Directory module is installed and imported:

```powershell
Import-Module ActiveDirectory
```

Check that the `-Path` value matches the distinguished name of your Organizational Unit.

---

# Best Practices

- Use meaningful group names that clearly indicate their purpose.
- Keep naming conventions consistent across the environment.
- Assign permissions to groups instead of individual users.
- Review group memberships regularly.
- Remove unused groups when they are no longer required.

---

# Lessons Learned

Creating Security Groups before assigning permissions provides a structured approach to access management. This makes future administration easier, supports scalability, and aligns with enterprise Active Directory best practices.

---

# Conclusion

The required Security Groups have been successfully created and organized within the **Security Groups** Organizational Unit. These groups provide the foundation for assigning permissions, managing access to resources, and implementing Role-Based Access Control throughout the Active Directory environment.
