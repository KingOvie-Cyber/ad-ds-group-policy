# Configure Windows Update Using Group Policy

## Executive Summary

Keeping Windows systems updated is one of the most effective ways to reduce security vulnerabilities and improve system stability. Rather than relying on individual users to manage updates, organizations use Group Policy to centrally control how Windows Update behaves on domain-joined computers.

In this guide, you will create a dedicated Group Policy Object (GPO) named **Windows Update Policy**, configure common Windows Update settings, link the GPO to the **Workstations** Organizational Unit (OU), apply the policy, and verify that it has been successfully deployed to a domain-joined client computer.

This guide demonstrates a standalone Active Directory environment where clients receive updates directly from Microsoft. In enterprise environments using **Windows Server Update Services (WSUS)**, additional configuration is required to specify the internal update server.

---

# Business Justification

Unpatched operating systems are a common target for attackers. Centralized update management helps ensure that all managed computers receive security updates in a consistent and timely manner.

Benefits include:

- Improved security posture.
- Reduced vulnerability exposure.
- Consistent update configuration.
- Simplified administration.
- Improved compliance with organizational policies.
- Reduced user intervention.

---

# Prerequisites

Before beginning, verify that:

- Active Directory Domain Services is operational.
- The **corp.local** domain is functioning correctly.
- The **Workstations** Organizational Unit exists.
- **CLIENT01** has joined the domain.
- You are signed in to **DC01** with a Domain Administrator account.
- Internet connectivity is available if updates will be obtained directly from Microsoft.

---

# Environment

| Component | Value |
|----------|-------|
| Domain | corp.local |
| Domain Controller | DC01 |
| Client Computer | CLIENT01 |
| Target OU | Workstations |
| Tool | Group Policy Management Console (GPMC) |

---

# What You Will Configure

This GPO will configure the following settings:

| Policy | Configuration |
|---------|---------------|
| Configure Automatic Updates | Enabled |
| Automatic Updating Option | Auto download and schedule install |
| Scheduled Install Day | Every day |
| Scheduled Install Time | 3:00 AM |
| No Auto-Restart with Logged-on Users | Enabled |

> **Note:** These settings are examples for a lab environment. Organizations should choose update schedules that align with maintenance windows and operational requirements.

---

# Step 1 — Open Group Policy Management

1. Sign in to **DC01**.
2. Open **Server Manager**.
3. Click **Tools**.
4. Select **Group Policy Management**.
5. Wait for the **Group Policy Management Console** to load.

---

# Step 2 — Create a New Group Policy Object

1. Expand:

```text
Forest: corp.local
    └── Domains
         └── corp.local
```

2. Click **Group Policy Objects**.
3. Right-click **Group Policy Objects**.
4. Select **New**.
5. Enter the following name:

```text
Windows Update Policy
```

6. Leave **Source Starter GPO** as **(None)**.
7. Click **OK**.

---

# Step 3 — Link the GPO

1. Locate the **Workstations** Organizational Unit.
2. Right-click **Workstations**.
3. Select:

```text
Link an Existing GPO...
```

4. Select:

```text
Windows Update Policy
```

5. Click **OK**.

The GPO is now linked to the Workstations OU.

---

# Step 4 — Edit the GPO

1. Right-click **Windows Update Policy**.
2. Select **Edit**.

The **Group Policy Management Editor** opens.

---

# Step 5 — Navigate to Windows Update Policies

In the left navigation pane, expand:

```text
Computer Configuration
    └── Policies
         └── Administrative Templates
              └── Windows Components
                   └── Windows Update
                   └── Manage end user experience
```

> Depending on your Windows Server administrative templates, some Windows Update settings may appear directly under **Windows Update**, while others are located in subfolders such as **Manage end user experience**.

---

# Step 6 — Configure Automatic Updates

1. Locate:

```text
Configure Automatic Updates
```

2. Double-click the policy.

3. Select:

```text
Enabled
```

4. Under **Configure automatic updating**, choose:

```text
4 - Auto download and schedule the install
```

5. Set:

```text
Scheduled install day:
Every day
```

6. Set:

```text
Scheduled install time:
3:00 AM
```

7. Click **Apply**.

8. Click **OK**.

---

# Step 7 — Prevent Automatic Restart While Users Are Logged On

1. Locate the policy that prevents automatic restart when users are actively signed in (the exact wording may vary slightly depending on the Windows administrative templates installed).

2. Double-click the policy.

3. Select:

```text
Enabled
```

4. Click **Apply**.

5. Click **OK**.

This reduces the likelihood of interrupting users with unexpected restarts.

---

# Step 8 — Close the Group Policy Editor

Review the configured settings.

Close the editor window.

The changes are saved automatically.

---

# Step 9 — Force Group Policy Update on DC01

Open **Windows PowerShell** as Administrator.

Run:

```powershell
gpupdate /force
```

Wait until the update completes successfully.

---

# Step 10 — Update CLIENT01

1. Sign in to **CLIENT01**.
2. Open **Windows PowerShell**.
3. Run:

```powershell
gpupdate /force
```

Restart the computer if prompted.

---

# Step 11 — Verify the Applied Policy

On **CLIENT01**, run:

```powershell
gpresult /r
```

Verify that:

```text
Windows Update Policy
```

appears under **Applied Group Policy Objects**.

Generate a detailed report:

```powershell
gpresult /h C:\WindowsUpdatePolicy.html
```

Open the report and review the applied settings.

---

# Step 12 — Verify Windows Update Configuration

On **CLIENT01**:

1. Open **Settings**.
2. Select **Windows Update**.
3. Review the displayed update behavior.

Some options may indicate that they are **managed by your organization**, demonstrating that Group Policy is controlling the configuration.

---

# Expected Results

After completing this guide:

- A dedicated Windows Update GPO has been created.
- The GPO is linked to the **Workstations** OU.
- Automatic update behavior is centrally managed.
- Restart behavior is configured according to policy.
- Clients receive update settings through Group Policy.

---

# Common Mistakes

- Linking the GPO to the wrong OU.
- Forgetting to refresh Group Policy.
- Expecting settings to apply immediately without running `gpupdate /force`.
- Confusing standalone Windows Update configuration with WSUS configuration.

---

# Troubleshooting

## The Policy Does Not Apply

- Verify the GPO is linked to the correct Organizational Unit.
- Run:

```powershell
gpresult /r
```

- Confirm that **Windows Update Policy** appears under **Applied Group Policy Objects**.
- Review the **GroupPolicy** logs in **Event Viewer** for processing errors.

---

# Rollback Procedure

To remove the Windows Update configuration:

1. Open **Group Policy Management**.
2. Edit **Windows Update Policy**.
3. Return modified settings to **Not Configured**.
4. Close the editor.
5. Run:

```powershell
gpupdate /force
```

6. Verify the updated behavior on **CLIENT01**.

---

# Best Practices

- Use a dedicated GPO for Windows Update settings.
- Test update configurations before broad deployment.
- Schedule installations during maintenance windows.
- Document all update policies.
- Review update compliance regularly.

---

# Lessons Learned

Group Policy provides a centralized method for managing Windows Update behavior across all domain-joined computers. Separating update settings into a dedicated GPO simplifies administration and helps ensure a consistent patch management process.

---

# Conclusion

You have successfully configured Windows Update settings using Group Policy. Domain-joined workstations now receive centrally managed update behavior, improving security and reducing the need for manual configuration.
