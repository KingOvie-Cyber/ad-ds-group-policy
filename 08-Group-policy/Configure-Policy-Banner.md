# Configure an Interactive Logon Banner Using Group Policy

## Executive Summary

An Interactive Logon Banner, also known as a **Legal Notice**, displays a message to users before they sign in to a Windows computer. Organizations commonly use login banners to inform users that systems are intended for authorized use only and that activity may be monitored in accordance with organizational policies.

Using Group Policy, administrators can centrally configure this message so that it is consistently displayed across all domain-joined computers.

In this guide, you will create a dedicated Group Policy Object (GPO) to configure an Interactive Logon Banner, link it to the **Workstations** Organizational Unit (OU), apply the policy, and verify that it is displayed on a domain-joined client computer.

---

# Business Justification

Organizations implement login banners for several reasons, including:

- Informing users that the system is owned by the organization.
- Providing a legal notice before authentication.
- Supporting security awareness.
- Helping meet organizational and regulatory requirements.
- Reinforcing acceptable use policies.

Although a login banner does not prevent attacks by itself, it forms part of an organization's overall security and compliance strategy.

---

# Prerequisites

Before beginning, verify that:

- Active Directory Domain Services is operational.
- The **corp.local** domain is functioning.
- The **Workstations** Organizational Unit exists.
- **CLIENT01** is joined to the domain.
- You are signed in to **DC01** using a Domain Administrator account.
- The **Group Policy Management Console (GPMC)** is installed.

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

This GPO will configure:

| Policy | Value |
|---------|-------|
| Interactive logon: Message title | Authorized Access Only |
| Interactive logon: Message text | Organization-approved legal notice |

---

# Step 1 — Open Group Policy Management

1. Sign in to **DC01** using a Domain Administrator account.
2. Open **Server Manager**.
3. Click **Tools**.
4. Select **Group Policy Management**.
5. Wait for the console to load.

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
Interactive Logon Banner Policy
```

6. Leave **Source Starter GPO** as **(None)**.
7. Click **OK**.

---

# Step 3 — Link the GPO

1. Locate the **Workstations** Organizational Unit.
2. Right-click **Workstations**.
3. Select **Link an Existing GPO...**
4. Select **Interactive Logon Banner Policy**.
5. Click **OK**.

---

# Step 4 — Edit the GPO

1. Right-click **Interactive Logon Banner Policy**.
2. Select **Edit**.

The **Group Policy Management Editor** opens.

---

# Step 5 — Navigate to Security Options

Expand the following path:

```text
Computer Configuration
    └── Policies
         └── Windows Settings
              └── Security Settings
                   └── Local Policies
                        └── Security Options
```

The right pane now displays numerous Windows security policies.

---

# Step 6 — Configure the Message Title

1. Locate:

```text
Interactive logon: Message title for users attempting to log on
```

2. Double-click the policy.
3. In the text box, enter:

```text
Authorized Access Only
```

4. Click **Apply**.
5. Click **OK**.

---

# Step 7 — Configure the Message Text

1. Locate:

```text
Interactive logon: Message text for users attempting to log on
```

2. Double-click the policy.

3. Enter a legal notice. For example:

```text
This computer system is the property of the organization.

Access is restricted to authorized users only.

By continuing, you acknowledge that your activities may be monitored, recorded, and reviewed in accordance with organizational policies and applicable laws.

Unauthorized access or misuse may result in disciplinary action and/or legal consequences.

If you are not an authorized user, disconnect immediately.
```

> Replace this sample text with wording approved by your organization's legal, compliance, or security teams where applicable.

4. Click **Apply**.
5. Click **OK**.

---

# Step 8 — Close the Group Policy Editor

Review the configured settings.

Close the editor window.

The settings are saved automatically.

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

4. Restart or sign out of the computer if prompted.

---

# Step 11 — Verify the Login Banner

1. Sign out of **CLIENT01**.
2. Return to the Windows sign-in screen.
3. Select the user account to sign in.

Before the password prompt (or immediately after selecting the account, depending on the Windows version), the configured login banner should appear.

Verify that:

- The title displays **Authorized Access Only**.
- The configured legal notice is displayed.
- Users must acknowledge the message before continuing with the sign-in process.

---

# Step 12 — Verify the Applied Policy

On **CLIENT01**, open **Windows PowerShell**.

Run:

```powershell
gpresult /r
```

Verify that:

```text
Interactive Logon Banner Policy
```

appears under **Applied Group Policy Objects**.

Generate a detailed report:

```powershell
gpresult /h C:\LoginBannerPolicy.html
```

Open the report and confirm that the policy has been applied.

---

# Expected Results

After completing this guide:

- A dedicated GPO for the Interactive Logon Banner has been created.
- The GPO is linked to the **Workstations** OU.
- The configured title and legal notice are displayed before users sign in.
- The policy is successfully applied to targeted computers.

---

# Common Mistakes

- Linking the GPO to the wrong Organizational Unit.
- Forgetting to refresh Group Policy.
- Using wording that has not been approved by the organization.
- Expecting the banner to appear without signing out or restarting when required.

---

# Troubleshooting

## The Login Banner Does Not Appear

- Run:

```powershell
gpupdate /force
```

- Sign out and sign back in.
- Verify the GPO is linked to the correct OU.
- Confirm the computer account is located in the targeted OU.
- Run:

```powershell
gpresult /r
```

- Review **Event Viewer** for Group Policy processing errors if necessary.

---

# Rollback Procedure

To remove the login banner:

1. Open **Group Policy Management**.
2. Edit **Interactive Logon Banner Policy**.
3. Clear the **Message title** and **Message text** fields, or revert the settings to their previous values.
4. Close the editor.
5. Run:

```powershell
gpupdate /force
```

6. Sign out and sign back in to verify that the banner no longer appears.

---

# Best Practices

- Use concise and professionally approved wording.
- Obtain approval from legal or compliance teams before deploying organization-wide.
- Store the policy in a dedicated GPO.
- Test the banner on a small group of computers before broad deployment.

- Review the message periodically to ensure it remains accurate and relevant.

---

# Lessons Learned

Interactive Logon Banners provide a centralized method for displaying legal notices and acceptable use information before user authentication. While they are not a standalone security control, they support organizational governance, compliance, and user awareness when implemented as part of a broader security strategy.

---

# Conclusion

You have successfully configured an Interactive Logon Banner using Group Policy. The banner is now displayed on targeted domain-joined computers before user authentication, providing a consistent organizational notice across the environment.
