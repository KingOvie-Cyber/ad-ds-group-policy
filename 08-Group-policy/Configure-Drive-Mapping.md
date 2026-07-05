# Configure Network Drive Mapping Using Group Policy Preferences

## Executive Summary

Network drive mapping allows users to access shared folders on file servers through a drive letter (for example, **H:** or **S:**). Instead of manually mapping drives on every workstation, administrators can use **Group Policy Preferences (GPP)** to automatically create, update, or remove mapped drives for domain users.

In this guide, you will:

- Create a shared folder on a server.
- Assign the appropriate NTFS and Share permissions.
- Create a dedicated Group Policy Object (GPO).
- Configure a mapped network drive using Group Policy Preferences.
- Link the GPO to an Organizational Unit (OU).
- Apply the policy to a client computer.
- Verify that the mapped drive appears automatically after user sign-in.

---

# Business Justification

Automatically mapping network drives provides users with consistent access to shared resources while reducing administrative effort.

Benefits include:

- Centralized management of shared folders.
- Consistent user experience.
- Simplified access to departmental resources.
- Reduced help desk requests.
- Easier migration of file servers.
- Improved scalability.

---

# Prerequisites

Before beginning, verify that:

- Active Directory Domain Services is operational.
- The **corp.local** domain is functioning correctly.
- The **Workstations** Organizational Unit exists.
- **CLIENT01** is joined to the domain.
- A file share is available on **DC01** or another file server.
- You are signed in to **DC01** using a Domain Administrator account.
- The **Group Policy Management Console (GPMC)** is installed.

---

# Environment

| Component | Value |
|----------|-------|
| Domain | corp.local |
| Domain Controller / File Server | DC01 |
| Client Computer | CLIENT01 |
| Target OU | Workstations |
| Network Share | `\\DC01\Departments` |
| Drive Letter | H: |

> **Note:** In production environments, file services are typically hosted on dedicated file servers rather than on domain controllers. This guide uses **DC01** for simplicity in a lab environment.

---

# Step 1 — Create the Shared Folder

1. Sign in to **DC01**.
2. Open **File Explorer**.
3. Navigate to the `C:` drive.
4. Create a new folder named:

```text
Departments
```

5. Right-click the **Departments** folder.
6. Select **Properties**.
7. Open the **Sharing** tab.
8. Click **Advanced Sharing**.
9. Check **Share this folder**.
10. Confirm the share name is:

```text
Departments
```

11. Click **Permissions**.
12. Grant the appropriate permissions (for example, **Read** or **Change**) to the required security groups.
13. Click **OK** to close each dialog box.

---

# Step 2 — Configure NTFS Permissions

1. While still in the folder properties, open the **Security** tab.
2. Review the existing permissions.
3. Add or modify permissions based on your organization's requirements.
4. Apply the principle of least privilege by granting users only the access they require.
5. Click **Apply**, then **OK**.

> Remember that effective access is determined by the combination of Share permissions and NTFS permissions.

---

# Step 3 — Open Group Policy Management

1. Open **Server Manager**.
2. Click **Tools**.
3. Select **Group Policy Management**.

Wait for the console to load.

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
Department Drive Mapping Policy
```

5. Leave **Source Starter GPO** as **(None)**.
6. Click **OK**.

---

# Step 5 — Link the GPO

1. Right-click the **Workstations** OU.
2. Select **Link an Existing GPO...**
3. Choose:

```text
Department Drive Mapping Policy
```

4. Click **OK**.

---

# Step 6 — Edit the GPO

1. Right-click **Department Drive Mapping Policy**.
2. Select **Edit**.

The **Group Policy Management Editor** opens.

---

# Step 7 — Navigate to Drive Maps

Expand:

```text
User Configuration
    └── Preferences
         └── Windows Settings
              └── Drive Maps
```

Right-click **Drive Maps**.

Select:

```text
New
    └── Mapped Drive
```

---

# Step 8 — Configure the Mapped Drive

Complete the configuration using the following settings:

| Setting | Value |
|---------|-------|
| Action | Create |
| Location | `\\DC01\Departments` |
| Label As | Department Files |
| Drive Letter | H: |
| Reconnect | Enabled |

After entering the values:

1. Click **Apply**.
2. Click **OK**.

The mapped drive preference is now saved.

---

# Step 9 — Close the Group Policy Editor

Review the configuration.

Close the editor.

The settings are saved automatically.

---

# Step 10 — Force Group Policy Update

On **DC01**, open **Windows PowerShell** as Administrator.

Run:

```powershell
gpupdate /force
```

Wait for the command to complete successfully.

---

# Step 11 — Update CLIENT01

1. Sign in to **CLIENT01** using a domain user account.
2. Open **Windows PowerShell**.
3. Run:

```powershell
gpupdate /force
```

4. Sign out and sign back in if prompted.

---

# Step 12 — Verify the Mapped Drive

1. Open **File Explorer**.
2. Select **This PC**.

Confirm that drive:

```text
H:
```

appears with the label:

```text
Department Files
```

Double-click the drive to verify that it opens the shared folder.

---

# Step 13 — Verify the Applied Policy

On **CLIENT01**, run:

```powershell
gpresult /r
```

Verify that:

```text
Department Drive Mapping Policy
```

appears under **Applied Group Policy Objects**.

Generate a detailed report:

```powershell
gpresult /h C:\DriveMappingPolicy.html
```

Review the report to confirm the policy has been applied.

---

# Expected Results

After completing this guide:

- The shared folder is available on the server.
- A dedicated GPO has been created for drive mapping.
- The GPO is linked to the **Workstations** OU.
- Domain users automatically receive the mapped network drive.
- The drive reconnects automatically after future sign-ins.

---

# Common Mistakes

- Incorrect UNC path (for example, typing the server or share name incorrectly).
- Forgetting to configure both Share permissions and NTFS permissions.
- Linking the GPO to the wrong Organizational Unit.
- Using a drive letter that is already assigned.
- Forgetting to refresh Group Policy before testing.

---

# Troubleshooting

## The Drive Does Not Appear

- Verify that the shared folder is accessible by browsing to:

```text
\\DC01\Departments
```

- Confirm that the user has both Share and NTFS permissions.
- Run:

```powershell
gpresult /r
```

- Verify that **Department Drive Mapping Policy** appears under **Applied Group Policy Objects**.
- Review the **Applications and Services Logs → Microsoft → Windows → GroupPolicy** logs in **Event Viewer** for any errors.

---

# Rollback Procedure

To remove the mapped drive:

1. Open **Group Policy Management**.
2. Edit **Department Drive Mapping Policy**.
3. Navigate to:

```text
User Configuration
    └── Preferences
         └── Windows Settings
              └── Drive Maps
```

4. Delete the mapped drive preference or change the **Action** to **Delete**.
5. Close the editor.
6. Run:

```powershell
gpupdate /force
```

7. Sign out and sign back in on **CLIENT01**.

---

# Best Practices

- Use **Group Policy Preferences** instead of legacy logon scripts for drive mapping.
- Use descriptive labels for mapped drives.
- Host file shares on dedicated file servers in production environments.
- Apply the principle of least privilege when assigning permissions.
- Test drive mappings before deploying them to production.

---

# Lessons Learned

Group Policy Preferences provide a flexible and reliable method for automatically mapping network drives for domain users. By combining properly configured file shares with centralized policy management, administrators can simplify access to shared resources while maintaining consistent permissions across the organization.

---

# Conclusion

You have successfully configured automatic network drive mapping using Group Policy Preferences. Users in the targeted Organizational Unit now receive a consistent mapped drive each time they sign in, improving access to shared resources and reducing manual configuration.

---

# Next Phase

Proceed to:

**Configure-Folder-Redirection.md**

In the next guide, you will configure **Folder Redirection** to redirect user folders such as **Documents** and **Desktop** to a centralized network location, improving data protection, simplifying backups, and supporting roaming user environments.
