# Create Your First Group Policy Object (GPO)

## Executive Summary

A **Group Policy Object (GPO)** is a collection of settings used to centrally manage computers and users within an Active Directory environment.

Creating a GPO is the first step in deploying standardized configurations across domain-joined computers. However, creating a GPO alone does not affect any users or computers. A GPO must also be **linked** to a Site, Domain, or Organizational Unit (OU) before its settings can be processed.

In this guide, you will create your first Group Policy Object using the **Group Policy Management Console (GPMC)**, link it to an Organizational Unit, verify that it was created successfully, and prepare it for future configuration.

---

# Business Justification

Organizations use Group Policy Objects to:

- Enforce security standards.
- Standardize workstation configurations.
- Prevent unauthorized configuration changes.
- Simplify administration.
- Reduce manual configuration.
- Ensure compliance with organizational policies.

Creating dedicated GPOs for individual purposes makes management easier and simplifies troubleshooting.

---

# Prerequisites

Before beginning this exercise, verify the following:

- Active Directory Domain Services is installed.
- The Domain Controller (**DC01**) is online.
- The **corp.local** domain is functioning correctly.
- The Organizational Units (OUs) created in previous sections already exist.
- A client computer (**CLIENT01**) has successfully joined the domain.
- You are signed in using an account that is a member of the **Domain Admins** group.

---

# Environment

| Component | Value |
|----------|-------|
| Domain | corp.local |
| Domain Controller | DC01 |
| Client Computer | CLIENT01 |
| Tool | Group Policy Management Console (GPMC) |

---

# What You Will Create

In this exercise, you will create the following Group Policy Object:

| Setting | Value |
|----------|-------|
| GPO Name | Workstation Baseline Policy |
| Link Target | Workstations OU |

This GPO will serve as the foundation for future computer configuration policies.

---

# Step-by-Step Implementation

## Step 1 — Open Server Manager

1. Sign in to **DC01** using a Domain Administrator account.
2. Click the **Start** button or press the **Windows** key.
3. Search for **Server Manager**.
4. Click **Server Manager** to open it.
5. Wait for the Server Manager dashboard to finish loading.

---

## Step 2 — Open Group Policy Management

1. In the upper-right corner of **Server Manager**, click **Tools**.
2. From the drop-down menu, select **Group Policy Management**.
3. Wait for the **Group Policy Management Console (GPMC)** to load.

You should now see the Group Policy Management console.

---

## Step 3 — Expand the Domain

In the left navigation pane:

1. Expand **Forest: corp.local**.
2. Expand **Domains**.
3. Expand **corp.local**.

You should now see several folders including:

```text
Group Policy Objects
Domain Controllers
Workstations
Finance
IT
Sales
Marketing
Human Resources
```

Depending on your environment, the Organizational Units displayed may vary.

---

## Step 4 — Create a New Group Policy Object

1. Locate the **Group Policy Objects** container.
2. Right-click **Group Policy Objects**.
3. Click **New**.

The **New GPO** dialog box appears.

---

## Step 5 — Configure the New GPO

Complete the dialog box using the following settings:

| Setting | Value |
|----------|-------|
| Name | Workstation Baseline Policy |
| Source Starter GPO | (None) |

Leave **Source Starter GPO** unchanged unless you are intentionally using a Starter GPO.

Click **OK**.

The new Group Policy Object is now created but is **not yet applied** to any users or computers.

---

## Step 6 — Verify the GPO Exists

1. Click the **Group Policy Objects** container.
2. Look in the right pane.

You should now see:

```text
Workstation Baseline Policy
```

If it appears, the GPO has been successfully created.

---

# Linking the GPO

Creating a GPO does **not** automatically apply it.

The GPO must be linked to an Organizational Unit.

---

## Step 7 — Link the GPO to the Workstations OU

1. In the left navigation pane, locate the **Workstations** Organizational Unit.
2. Right-click **Workstations**.
3. Select:

```text
Link an Existing GPO...
```

The **Select GPO** window opens.

---

## Step 8 — Select the Group Policy Object

1. Highlight:

```text
Workstation Baseline Policy
```

2. Click **OK**.

The Group Policy Object is now linked to the **Workstations** Organizational Unit.

---

## Step 9 — Verify the Link

Expand:

```text
Workstations
```

You should now see:

```text
Linked Group Policy Objects

    Workstation Baseline Policy
```

This confirms that the GPO has been linked successfully.

---

# Verify Using Group Policy Management

Click the **Workstation Baseline Policy**.

Review the following tabs:

- Scope
- Details
- Settings
- Delegation

At this stage:

- The GPO exists.
- The GPO is linked.
- No settings have been configured yet.

This is expected.

---

# PowerShell Verification

Open **Windows PowerShell** as Administrator.

List all GPOs:

```powershell
Get-GPO -All
```

Locate a specific GPO:

```powershell
Get-GPO -Name "Workstation Baseline Policy"
```

Verify that the GPO exists without errors.

---

# Expected Results

After completing this exercise:

- A new Group Policy Object has been created.
- The GPO is stored within the **Group Policy Objects** container.
- The GPO has been linked to the **Workstations** Organizational Unit.
- The GPO is ready for configuration.

---

# Common Mistakes

## Creating a GPO Without Linking It

A GPO that is not linked to a Site, Domain, or Organizational Unit will not be processed.

---

## Linking the GPO to the Wrong Organizational Unit

Always verify that you are linking the GPO to the intended target.

---

## Editing the Default Domain Policy

Avoid modifying the **Default Domain Policy** unless you are configuring domain-wide settings such as password or account lockout policies.

Create new GPOs for most administrative configurations.

---

# Troubleshooting

## The Group Policy Management Console Is Missing

Verify that the **Group Policy Management** feature is installed.

---

## Unable to Create a GPO

Check that:

- You have Domain Admin or delegated permissions.
- The domain is online.
- Active Directory replication is healthy (in multi-DC environments).

---

## The GPO Does Not Appear

Press **F5** to refresh the console.

If necessary, close and reopen the Group Policy Management Console.

---

# Best Practices

- Give every GPO a descriptive name.
- Create one GPO for one primary purpose.
- Avoid placing unrelated settings in the same GPO.
- Link GPOs only where they are needed.
- Test GPOs in a lab before deploying to production.

---

# Lessons Learned

Creating a Group Policy Object is only the beginning of the policy deployment process. A GPO must be linked to the appropriate Organizational Unit before it can affect users or computers. Organizing GPOs with clear names and specific purposes simplifies administration and troubleshooting.

---

# Conclusion

You have successfully created your first Group Policy Object and linked it to the **Workstations** Organizational Unit. Although the GPO does not yet contain any configured settings, it is now ready to be used for deploying centralized computer configurations across domain-joined workstations.
