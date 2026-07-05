# Configure Security Filtering for Group Policy Objects

## Executive Summary

Security Filtering allows administrators to control exactly which users, computers, or security groups receive a Group Policy Object (GPO). Rather than applying a GPO to every object within an Organizational Unit (OU), Security Filtering enables administrators to target policies only to authorized recipients.

This approach is widely used in enterprise environments to deploy different configurations to different departments, teams, or computer groups without creating unnecessary Organizational Units.

In this guide, you will:

- Create a security group in Active Directory.
- Add users to the security group.
- Create a dedicated Group Policy Object.
- Configure Security Filtering.
- Remove the default **Authenticated Users** filter.
- Apply the GPO only to members of the security group.
- Verify that the policy applies correctly.

---

# Business Justification

Security Filtering enables organizations to:

- Apply policies only to specific users.
- Reduce unnecessary GPO processing.
- Simplify GPO administration.
- Support role-based administration.
- Deploy different configurations to different departments.
- Reduce the number of Organizational Units required.

---

# Prerequisites

Before beginning, ensure that:

- Active Directory Domain Services is operational.
- The **corp.local** domain is functioning.
- The **Workstations** Organizational Unit exists.
- You are signed in to **DC01** using a Domain Administrator account.
- The **Group Policy Management Console (GPMC)** is installed.
- Active Directory Users and Computers (ADUC) is installed.

---

# Environment

| Component | Value |
|----------|-------|
| Domain | corp.local |
| Domain Controller | DC01 |
| Target OU | Workstations |
| Security Group | IT Support |
| Test User | John Doe |

---

# Understanding Security Filtering

By default, most new GPOs include:

```text
Authenticated Users
```

This means that every authenticated user or computer within the linked scope is eligible to receive the policy, provided they also have the required permissions.

Security Filtering allows you to replace this default behavior by specifying exactly which security principals can apply the policy.

---

# Step 1 — Create a Security Group

1. Sign in to **DC01**.
2. Open **Server Manager**.
3. Click **Tools**.
4. Select **Active Directory Users and Computers**.
5. Navigate to the Organizational Unit where you want to create the group.
6. Right-click the OU.
7. Select:

```text
New → Group
```

8. Configure the following:

| Setting | Value |
|---------|-------|
| Group Name | IT Support |
| Group Scope | Global |
| Group Type | Security |

9. Click **OK**.

---

# Step 2 — Add Users to the Group

1. Double-click the **IT Support** group.
2. Open the **Members** tab.
3. Click **Add**.
4. Enter the user account(s) that should receive the policy.
5. Click **Check Names**.
6. Click **OK**.
7. Click **Apply**, then **OK**.

---

# Step 3 — Open Group Policy Management

1. Open **Server Manager**.
2. Click **Tools**.
3. Select **Group Policy Management**.

---

# Step 4 — Create a New GPO

1. Expand:

```text
Forest: corp.local
    └── Domains
         └── corp.local
```

2. Right-click **Group Policy Objects**.
3. Select **New**.
4. Name the GPO:

```text
IT Support Policy
```

5. Click **OK**.

---

# Step 5 — Link the GPO

1. Right-click the **Workstations** OU.
2. Select:

```text
Link an Existing GPO...
```

3. Select:

```text
IT Support Policy
```

4. Click **OK**.

---

# Step 6 — Configure a Sample Policy

Edit the GPO and configure any simple policy for testing.

For example:

```text
Desktop Wallpaper
```

or

```text
Control Panel Restrictions
```

The purpose is simply to verify that Security Filtering works correctly.

---

# Step 7 — Configure Security Filtering

1. Return to **Group Policy Management**.
2. Select:

```text
IT Support Policy
```

3. Locate the **Scope** tab.
4. Under:

```text
Security Filtering
```

you will see:

```text
Authenticated Users
```

5. Select **Authenticated Users**.
6. Click **Remove**.

> **Important:** Removing **Authenticated Users** from the Security Filtering section does not remove the underlying read permissions required for Group Policy processing. Ensure appropriate read permissions remain in place according to your organization's design.

---

# Step 8 — Add the Security Group

1. Click **Add**.
2. Enter:

```text
IT Support
```

3. Click **Check Names**.
4. Click **OK**.

The Security Filtering section should now display:

```text
IT Support
```

---

# Step 9 — Verify Delegation Permissions

1. Select the GPO.
2. Open the **Delegation** tab.
3. Click **Advanced**.

Confirm that the **IT Support** group has:

- Read
- Apply Group Policy

permissions.

If necessary:

1. Click **Add**.
2. Select **IT Support**.
3. Grant the required permissions.
4. Click **Apply**.
5. Click **OK**.

---

# Step 10 — Force Group Policy Update

On **DC01**, open **Windows PowerShell** as Administrator.

Run:

```powershell
gpupdate /force
```

---

# Step 11 — Update CLIENT01

1. Sign in as a member of the **IT Support** group.
2. Open **Windows PowerShell**.
3. Run:

```powershell
gpupdate /force
```

Sign out and sign back in if prompted.

---

# Step 12 — Verify Policy Application

Run:

```powershell
gpresult /r
```

Confirm that:

```text
IT Support Policy
```

appears under **Applied Group Policy Objects**.

Now test with a user account that is **not** a member of **IT Support**.

Run:

```powershell
gpresult /r
```

The policy should not appear under the applied GPOs.

---

# Step 13 — Generate a Group Policy Report

Run:

```powershell
gpresult /h C:\SecurityFilteringReport.html
```

Open the report and verify:

- Applied GPOs
- Filtered GPOs
- Security Filtering
- Processing results

---

# Expected Results

After completing this guide:

- A security group controls GPO application.
- Only authorized users receive the policy.
- Users outside the group do not receive the policy.
- Group Policy processing remains centralized and manageable.

---

# Common Mistakes

- Forgetting to add users to the security group.
- Removing required permissions from the GPO.
- Assuming OU membership alone controls policy application.
- Testing before refreshing Group Policy.
- Forgetting that group membership changes may require the user to sign out and back in before taking effect.

---

# Troubleshooting

## The Policy Does Not Apply

Verify:

- The user belongs to the correct security group.
- The GPO is linked correctly.
- The GPO is enabled.
- The security group has both **Read** and **Apply Group Policy** permissions.
- The user has refreshed Group Policy by running:

```powershell
gpupdate /force
```

Use:

```powershell
gpresult /r
```

to verify whether the policy was applied or filtered.

---

# Rollback Procedure

To remove Security Filtering:

1. Open **Group Policy Management**.
2. Select the GPO.
3. Open the **Scope** tab.
4. Under **Security Filtering**, remove the **IT Support** group.
5. Add **Authenticated Users** back if you want the GPO to apply broadly within its linked scope.
6. Run:

```powershell
gpupdate /force
```

7. Verify the updated behavior on the client computer.

---

# Best Practices

- Use Security Filtering to target policies instead of creating unnecessary Organizational Units.
- Assign policies to security groups rather than individual users whenever possible.
- Keep group memberships well documented.
- Periodically review security groups to remove obsolete accounts.
- Test Security Filtering before deploying to production.

---

# Lessons Learned

Security Filtering provides granular control over Group Policy deployment without requiring additional Organizational Units. By combining Active Directory security groups with Group Policy, administrators can efficiently target configurations to the appropriate users and computers while maintaining a clean and scalable directory structure.

---

# Conclusion

You have successfully configured Security Filtering for a Group Policy Object. The policy is now applied only to members of the designated security group, demonstrating how organizations implement targeted policy deployment in enterprise Active Directory environments.

