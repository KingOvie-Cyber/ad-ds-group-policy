# Configure Logon Scripts Using Group Policy

## Executive Summary

Logon scripts allow administrators to automatically execute commands, batch files, PowerShell scripts, or other supported scripts whenever a domain user signs in to a Windows computer.

Although many configuration tasks are now better handled using **Group Policy Preferences**, logon scripts remain useful for automation, legacy applications, and administrative tasks that require custom logic.

In this guide, you will:

- Create a simple PowerShell logon script.
- Store the script in the domain's SYSVOL Scripts folder.
- Create a dedicated Group Policy Object (GPO).
- Assign the logon script through Group Policy.
- Link the GPO to an Organizational Unit (OU).
- Apply the policy to a client computer.
- Verify that the script executes successfully when a user signs in.

---

# Business Justification

Organizations use logon scripts to automate repetitive tasks that occur whenever a user signs in.

Common examples include:

- Launching approved applications.
- Creating folders.
- Setting environment variables.
- Mapping legacy resources.
- Running inventory scripts.
- Displaying informational messages.
- Performing custom administrative actions.

Whenever possible, use native Group Policy settings or Group Policy Preferences before choosing logon scripts.

---

# Prerequisites

Before beginning, verify that:

- Active Directory Domain Services is operational.
- The **corp.local** domain is functioning.
- The **Workstations** Organizational Unit exists.
- **CLIENT01** has joined the domain.
- You are signed in to **DC01** with a Domain Administrator account.
- The **Group Policy Management Console (GPMC)** is installed.

---

# Environment

| Component | Value |
|----------|-------|
| Domain | corp.local |
| Domain Controller | DC01 |
| Client Computer | CLIENT01 |
| Target OU | Workstations |
| Script Type | PowerShell |

---

# Step 1 — Create a Logon Script

1. Sign in to **DC01**.
2. Open **Notepad** or another text editor.
3. Create a new file with the following content:

```powershell
New-Item -Path "$env:USERPROFILE\Documents\LogonTest" -ItemType Directory -Force
```

This script creates a folder named **LogonTest** inside the signed-in user's **Documents** folder if it does not already exist.

4. Save the file as:

```text
CreateLogonFolder.ps1
```

---

# Step 2 — Store the Script in SYSVOL

1. Open **File Explorer**.
2. Browse to:

```text
C:\Windows\SYSVOL\sysvol\corp.local\scripts
```

3. Copy **CreateLogonFolder.ps1** into the **scripts** folder.

> The SYSVOL folder is automatically replicated between domain controllers in multi-domain controller environments, making it the recommended location for domain logon scripts.

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
User Logon Script Policy
```

5. Click **OK**.

---

# Step 5 — Link the GPO

1. Right-click the **Workstations** OU.
2. Select **Link an Existing GPO...**
3. Choose:

```text
User Logon Script Policy
```

4. Click **OK**.

---

# Step 6 — Edit the GPO

1. Right-click **User Logon Script Policy**.
2. Select **Edit**.

The **Group Policy Management Editor** opens.

---

# Step 7 — Navigate to Logon Scripts

Expand:

```text
User Configuration
    └── Policies
         └── Windows Settings
              └── Scripts (Logon/Logoff)
```

Click:

```text
Logon
```

---

# Step 8 — Add the Script

1. Click **Add**.
2. Select **Browse**.
3. Browse to the script stored in the SYSVOL Scripts folder.
4. Select:

```text
CreateLogonFolder.ps1
```

5. Click **Open**.
6. Leave **Script Parameters** blank unless your script requires arguments.
7. Click **OK**.
8. Click **Apply**.
9. Click **OK**.

---

# Step 9 — Close the Group Policy Editor

Review the configuration.

Close the editor window.

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

1. Sign in to **CLIENT01**.
2. Open **Windows PowerShell**.
3. Run:

```powershell
gpupdate /force
```

4. Sign out of the computer.

5. Sign back in using the same domain user account.

The logon script should execute automatically during the sign-in process.

---

# Step 12 — Verify the Script

After signing in:

1. Open **File Explorer**.
2. Navigate to:

```text
Documents
```

3. Confirm that the following folder has been created:

```text
LogonTest
```

The existence of this folder confirms that the script executed successfully.

---

# Step 13 — Verify the Applied Policy

Open **Windows PowerShell** on **CLIENT01**.

Run:

```powershell
gpresult /r
```

Verify that:

```text
User Logon Script Policy
```

appears under **Applied Group Policy Objects**.

Generate a detailed report:

```powershell
gpresult /h C:\LogonScriptReport.html
```

Review the report to verify that the policy has been applied.

---

# Expected Results

After completing this guide:

- A PowerShell logon script is stored in the SYSVOL Scripts folder.
- A dedicated GPO is created.
- The GPO is linked to the **Workstations** OU.
- The logon script executes whenever a targeted user signs in.
- The **LogonTest** folder is automatically created in the user's **Documents** folder.

---

# Common Mistakes

- Saving the script outside the SYSVOL Scripts folder.
- Assigning the script under **Computer Configuration** instead of **User Configuration**.
- Forgetting to sign out and sign back in after updating Group Policy.
- Using a local account instead of a domain user account for testing.

---

# Troubleshooting

## The Script Does Not Run

- Confirm the script exists in the SYSVOL Scripts folder.
- Verify the GPO is linked to the correct OU.
- Run:

```powershell
gpresult /r
```

- Ensure **User Logon Script Policy** appears under **Applied Group Policy Objects**.
- Review **Event Viewer** under **Applications and Services Logs → Microsoft → Windows → GroupPolicy** for errors.
- If using PowerShell scripts, verify that execution policy and Group Policy settings permit them to run in your environment.

---

# Rollback Procedure

To remove the logon script:

1. Open **Group Policy Management**.
2. Edit **User Logon Script Policy**.
3. Navigate to:

```text
User Configuration
    └── Policies
         └── Windows Settings
              └── Scripts (Logon/Logoff)
```

4. Open **Logon**.
5. Select **CreateLogonFolder.ps1**.
6. Click **Remove**.
7. Click **Apply**.
8. Click **OK**.
9. Run:

```powershell
gpupdate /force
```

10. Sign out and sign back in to verify the script no longer executes.

---

# Best Practices

- Prefer native Group Policy settings or Group Policy Preferences when they provide the required functionality.
- Keep logon scripts lightweight to minimize sign-in delays.
- Store domain logon scripts in the SYSVOL Scripts folder.
- Document every script and its purpose.
- Test scripts in a lab before deploying to production.

---

# Lessons Learned

Logon scripts provide a flexible way to automate user sign-in tasks that cannot easily be accomplished through standard Group Policy settings. Properly managed scripts can improve consistency and reduce manual administrative work while remaining easy to maintain.

---

# Conclusion

You have successfully configured a PowerShell logon script using Group Policy. Users targeted by the GPO now execute the script automatically during sign-in, demonstrating how Active Directory can automate user-specific administrative tasks across the domain.
