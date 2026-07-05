# Configure Folder Redirection Using Group Policy

## Executive Summary

Folder Redirection allows administrators to redirect commonly used user folders, such as **Documents**, **Desktop**, and **Pictures**, from a local computer to a network location. This enables centralized storage of user data, simplifies backups, and allows users to access their files from different domain-joined computers.

In this guide, you will:

- Create a shared folder to host redirected user data.
- Configure the required Share and NTFS permissions.
- Create a dedicated Group Policy Object (GPO).
- Redirect the **Documents** folder using Group Policy.
- Link the GPO to the appropriate Organizational Unit (OU).
- Apply the policy to a domain user.
- Verify that redirection is working correctly.

> **Note:** This guide demonstrates redirecting the **Documents** folder. The same process can be used for other supported folders such as **Desktop**, **Pictures**, and **Favorites**.

---

# Business Justification

Folder Redirection is widely used because it:

- Centralizes user data.
- Simplifies backup and disaster recovery.
- Enables users to access their files from multiple domain-joined computers.
- Reduces the risk of data loss due to workstation failure.
- Simplifies computer replacement and migration.
- Supports organizational data management policies.

---

# Prerequisites

Before beginning, ensure that:

- Active Directory Domain Services is operational.
- The **corp.local** domain is functioning correctly.
- The **Workstations** OU exists.
- **CLIENT01** is joined to the domain.
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
| Redirected Folder Share | `\\DC01\UserData` |

> **Note:** In production environments, Folder Redirection is commonly hosted on a dedicated file server instead of a domain controller.

---

# Step 1 — Create the Redirected Folder

1. Sign in to **DC01**.
2. Open **File Explorer**.
3. Navigate to the `C:` drive.
4. Create a folder named:

```text
UserData
```

5. Right-click the folder and select **Properties**.

---

# Step 2 — Share the Folder

1. Open the **Sharing** tab.
2. Click **Advanced Sharing**.
3. Check **Share this folder**.
4. Confirm the share name:

```text
UserData
```

5. Click **Permissions**.
6. Configure Share permissions according to your organization's requirements.
7. Click **OK** to close all dialog boxes.

---

# Step 3 — Configure NTFS Permissions

1. Open the **Security** tab.
2. Configure NTFS permissions so that users can create and access only their own redirected folders.
3. Apply the principle of least privilege when assigning permissions.
4. Click **Apply**, then **OK**.

> **Important:** Microsoft provides recommended NTFS and Share permission configurations for Folder Redirection. Follow those recommendations for production environments.

---

# Step 4 — Open Group Policy Management

1. Open **Server Manager**.
2. Select **Tools**.
3. Click **Group Policy Management**.

---

# Step 5 — Create a New Group Policy Object

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
Folder Redirection Policy
```

5. Click **OK**.

---

# Step 6 — Link the GPO

1. Right-click the **Workstations** OU.
2. Select **Link an Existing GPO...**
3. Choose:

```text
Folder Redirection Policy
```

4. Click **OK**.

---

# Step 7 — Edit the GPO

1. Right-click **Folder Redirection Policy**.
2. Select **Edit**.

The **Group Policy Management Editor** opens.

---

# Step 8 — Navigate to Folder Redirection

Expand:

```text
User Configuration
    └── Policies
         └── Windows Settings
              └── Folder Redirection
```

Click:

```text
Documents
```

---

# Step 9 — Configure Folder Redirection

1. Right-click **Documents**.
2. Select **Properties**.

Configure the following settings:

| Setting | Value |
|---------|-------|
| Setting | Basic – Redirect everyone's folder to the same location |
| Target folder location | Create a folder for each user under the root path |
| Root Path | `\\DC01\UserData` |

Click **Apply**.

If prompted to create the folder automatically, select **Yes**.

Click **OK**.

---

# Step 10 — Review Additional Options

Review the **Settings** tab.

Common options include:

- Grant the user exclusive rights to Documents.
- Move the contents of Documents to the new location.
- Redirect the folder back to the local user profile when the policy is removed.

Choose the options that align with your organization's requirements.

---

# Step 11 — Close the Group Policy Editor

Review the configuration.

Close the editor.

The settings are saved automatically.

---

# Step 12 — Force Group Policy Update

On **DC01**, open **Windows PowerShell** as Administrator.

Run:

```powershell
gpupdate /force
```

---

# Step 13 — Update CLIENT01

1. Sign in to **CLIENT01** using a domain user account.
2. Open **Windows PowerShell**.
3. Run:

```powershell
gpupdate /force
```

4. Sign out and sign back in if prompted.

---

# Step 14 — Verify Folder Redirection

On **CLIENT01**:

1. Open **File Explorer**.
2. Right-click **Documents**.
3. Select **Properties**.
4. Open the **Location** tab.

Verify that the path now points to:

```text
\\DC01\UserData\<username>\Documents
```

(where `<username>` represents the signed-in user's account).

Create a test document in **Documents** and confirm that it is stored in the redirected network location.

---

# Step 15 — Verify the Applied Policy

On **CLIENT01**, open **Windows PowerShell**.

Run:

```powershell
gpresult /r
```

Confirm that:

```text
Folder Redirection Policy
```

appears under **Applied Group Policy Objects**.

Generate a report:

```powershell
gpresult /h C:\FolderRedirectionReport.html
```

Review the report to verify successful policy application.

---

# Expected Results

After completing this guide:

- The shared folder is configured.
- Folder Redirection is enabled.
- The user's **Documents** folder points to the network share.
- New files created in **Documents** are stored centrally.
- Group Policy successfully applies the Folder Redirection settings.

---

# Common Mistakes

- Incorrect Share or NTFS permissions.
- Incorrect UNC path.
- Linking the GPO to the wrong Organizational Unit.
- Forgetting to refresh Group Policy.
- Assuming Folder Redirection works without properly configured file share permissions.

---

# Troubleshooting

## Documents Folder Is Not Redirected

- Verify that the user account is located in the targeted OU.
- Confirm the UNC path is accessible.
- Check Share and NTFS permissions.
- Run:

```powershell
gpresult /r
```

- Verify that **Folder Redirection Policy** appears under **Applied Group Policy Objects**.
- Review the **Applications and Services Logs → Microsoft → Windows → Folder Redirection** and **GroupPolicy** logs in **Event Viewer** if available.

---

# Rollback Procedure

To remove Folder Redirection:

1. Open **Group Policy Management**.
2. Edit **Folder Redirection Policy**.
3. Right-click **Documents** and open **Properties**.
4. Change **Setting** to **Not Configured** or choose the appropriate option to redirect the folder back to the local profile.
5. Close the editor.
6. Run:

```powershell
gpupdate /force
```

7. Sign out and sign back in on **CLIENT01**.

---

# Best Practices

- Host redirected folders on a dedicated file server in production.
- Configure Share and NTFS permissions according to Microsoft's recommendations.
- Test Folder Redirection in a lab before broad deployment.
- Monitor available storage on the file server.
- Back up redirected user data regularly.

---

# Lessons Learned

Folder Redirection enables centralized storage of user data while providing a consistent user experience across multiple domain-joined computers. Properly configured permissions and Group Policy ensure that user files remain secure, accessible, and easier to protect through centralized backup strategies.

---

# Conclusion

You have successfully configured Folder Redirection using Group Policy. User documents are now stored in a centralized network location, simplifying data protection, backups, and workstation replacement while demonstrating another practical use of Active Directory Group Policy.
