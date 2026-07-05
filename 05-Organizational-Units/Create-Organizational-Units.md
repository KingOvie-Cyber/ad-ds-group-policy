# Create Organizational Units (OUs)

## Executive Summary

After planning the Organizational Unit (OU) hierarchy, the next step is to create the Organizational Units within Active Directory.

Organizational Units provide a logical way to organize users, computers, groups, and service accounts while allowing administrators to apply Group Policy Objects (GPOs) and delegate administrative permissions.

This guide demonstrates how to create Organizational Units using **Active Directory Users and Computers (ADUC)** and concludes with the equivalent PowerShell commands for automation.

---

# Business Justification

A properly designed Organizational Unit structure simplifies Active Directory administration by separating objects according to business functions or administrative requirements.

Benefits include:

- Easier user management.
- Simplified computer management.
- Targeted Group Policy deployment.
- Delegated administration.
- Better scalability.
- Improved organization.

---

# Prerequisites

Before beginning:

- The Domain Controller is operational.
- Active Directory Domain Services is installed.
- You are signed in with a Domain Administrator account.
- Active Directory Users and Computers (ADUC) is available.

---

# Organizational Units to Create

The following OUs will be created during this exercise.

| Organizational Unit | Purpose |
|----------------------|---------|
| Administration | Administrative accounts |
| IT | IT personnel |
| Human Resources | HR staff |
| Finance | Finance department |
| Sales | Sales department |
| Marketing | Marketing department |
| Workstations | Domain-joined client computers |
| Servers | Member servers |
| Service Accounts | Service accounts |
| Security Groups | Security groups |

---

# Step-by-Step Implementation

## Step 1 — Open Active Directory Users and Computers

1. Sign in to **DC01** using a Domain Administrator account.
2. Click the **Start** button.
3. Search for **Server Manager** and open it.
4. In the upper-right corner, click **Tools**.
5. Select **Active Directory Users and Computers**.
6. Wait for the console to load.
7. Expand your domain.

Example:

```text
corp.local
```

You should now see the default Active Directory containers.

---

## Step 2 — Create the Administration OU

1. Right-click your domain name.

Example:

```text
corp.local
```

2. Select:

```text
New
    └── Organizational Unit
```

3. Enter the following name:

```text
Administration
```

4. Enable:

```text
Protect container from accidental deletion
```

5. Click **OK**.

The new Organizational Unit should immediately appear beneath the domain.

---

## Step 3 — Create the IT OU

Repeat the previous process.

Right-click:

```text
corp.local
```

Select:

```text
New
    └── Organizational Unit
```

Enter:

```text
IT
```

Enable accidental deletion protection.

Click **OK**.

---

## Step 4 — Create the Human Resources OU

Repeat the same process.

Name the OU:

```text
Human Resources
```

Enable accidental deletion protection.

Click **OK**.

---

## Step 5 — Create the Finance OU

Create another Organizational Unit named:

```text
Finance
```

Enable accidental deletion protection.

Click **OK**.

---

## Step 6 — Create the Sales OU

Create another Organizational Unit.

Name:

```text
Sales
```

Click **OK**.

---

## Step 7 — Create the Marketing OU

Create another Organizational Unit.

Name:

```text
Marketing
```

Click **OK**.

---

## Step 8 — Create the Workstations OU

Create another Organizational Unit.

Name:

```text
Workstations
```

Enable accidental deletion protection.

Click **OK**.

This OU will later contain all domain-joined client computers.

---

## Step 9 — Create the Servers OU

Create another Organizational Unit.

Name:

```text
Servers
```

Enable accidental deletion protection.

Click **OK**.

This OU will later contain member servers.

---

## Step 10 — Create the Service Accounts OU

Create another Organizational Unit.

Name:

```text
Service Accounts
```

Enable accidental deletion protection.

Click **OK**.

---

## Step 11 — Create the Security Groups OU

Create another Organizational Unit.

Name:

```text
Security Groups
```

Enable accidental deletion protection.

Click **OK**.

---

## Step 12 — Verify the Organizational Units

Expand your domain.

Your structure should resemble the following:

```text
corp.local
│
├── Administration
├── Finance
├── Human Resources
├── IT
├── Marketing
├── Sales
├── Security Groups
├── Servers
├── Service Accounts
└── Workstations
```

Verify that each Organizational Unit appears in the correct location.

---

# PowerShell Equivalent

The same Organizational Units can be created using Windows PowerShell.

```powershell
$OUs = @(
"Administration",
"IT",
"Human Resources",
"Finance",
"Sales",
"Marketing",
"Workstations",
"Servers",
"Service Accounts",
"Security Groups"
)

foreach ($OU in $OUs)
{
    New-ADOrganizationalUnit `
        -Name $OU `
        -Path "DC=corp,DC=local" `
        -ProtectedFromAccidentalDeletion $true
}
```

Verify the Organizational Units:

```powershell
Get-ADOrganizationalUnit -Filter *
```

---

# Verification

Confirm that:

- All Organizational Units exist.
- The names are correct.
- Each OU is located directly beneath the domain.
- Protection against accidental deletion is enabled where configured.

You can also verify using PowerShell:

```powershell
Get-ADOrganizationalUnit -Filter * |
Select Name
```

---

# Expected Results

At the conclusion of this exercise:

- All planned Organizational Units have been created.
- The Active Directory environment is logically organized.
- The structure is ready for user accounts, computer accounts, groups, and Group Policy Objects.

---

# Common Mistakes

## Creating OUs in the Wrong Location

Always create Organizational Units beneath the correct domain unless a nested OU structure has been intentionally designed.

---

## Inconsistent Naming

Use consistent names throughout the environment to simplify administration.

---

## Forgetting Protection Against Accidental Deletion

Leaving this option disabled increases the risk of unintentionally removing an Organizational Unit and all objects it contains.

---

# Troubleshooting

## Unable to Create an Organizational Unit

Verify that:

- You are signed in with sufficient privileges.
- The domain is available.
- Active Directory Users and Computers is connected to the correct domain.

---

## OU Does Not Appear

Refresh the console by pressing **F5** or right-clicking the domain and selecting **Refresh**.

---

## PowerShell Command Fails

Ensure the Active Directory module is installed and imported:

```powershell
Import-Module ActiveDirectory
```

---

# Best Practices

- Follow a documented OU design.
- Enable accidental deletion protection.
- Use meaningful names.
- Minimize unnecessary nesting.
- Review the OU structure periodically as the environment grows.

---

# Lessons Learned

Creating Organizational Units is a foundational administrative task that establishes the structure used for user management, computer management, delegated administration, and Group Policy deployment. A consistent and well-planned hierarchy simplifies long-term Active Directory management.

---

# Conclusion

The Organizational Unit hierarchy has been successfully created within the **corp.local** domain. The environment is now prepared for the creation and organization of users, computers, groups, and the application of Group Policy Objects.
