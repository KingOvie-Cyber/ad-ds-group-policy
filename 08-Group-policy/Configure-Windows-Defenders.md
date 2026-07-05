# Configure Microsoft Defender Using Group Policy

## Executive Summary

Microsoft Defender Antivirus is Microsoft's built-in endpoint protection solution for Windows operating systems. Through Group Policy, administrators can centrally configure Microsoft Defender settings across all domain-joined computers without requiring manual configuration on each endpoint.

Rather than disabling Defender, organizations typically use Group Policy to enforce security settings such as real-time protection, cloud-delivered protection, scheduled scanning, automatic sample submission, and exclusion management.

In this guide, you will create a dedicated Group Policy Object (GPO) to manage Microsoft Defender, link it to the **Workstations** Organizational Unit, configure common security settings, force a Group Policy update, and verify that the policy has been successfully applied on a domain-joined client.

---

# Business Justification

Endpoint protection is a critical component of an organization's security strategy. Centrally managing Microsoft Defender helps ensure consistent security configurations across all managed devices while reducing administrative overhead.

Benefits include:

- Standardized antivirus configuration.
- Improved malware detection.
- Consistent security posture.
- Centralized administration.
- Simplified compliance.
- Reduced configuration drift.

---

# Prerequisites

Before beginning:

- Active Directory Domain Services is operational.
- The **corp.local** domain is functioning.
- A **Workstations** Organizational Unit exists.
- **CLIENT01** is joined to the domain.
- You are signed in to **DC01** with a Domain Administrator account.
- Microsoft Defender Antivirus is installed and active on client computers.

---

# Environment

| Component | Value |
|----------|-------|
| Domain | corp.local |
| Domain Controller | DC01 |
| Client Computer | CLIENT01 |
| Target OU | Workstations |
| Tool | Group Policy Management Console |

---

# What You Will Configure

This GPO will:

- Enable Real-time Protection.
- Enable Cloud-delivered Protection.
- Configure Automatic Sample Submission.
- Configure Scheduled Scans.
- Demonstrate how exclusions are configured.
- Verify policy application.

---

# Step 1 — Open Group Policy Management

1. Sign in to **DC01**.
2. Open **Server Manager**.
3. Select **Tools**.
4. Click **Group Policy Management**.
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
5. Enter:

```text
Microsoft Defender Policy
```

6. Leave **Source Starter GPO** as **(None)**.
7. Click **OK**.

---

# Step 3 — Link the GPO

1. Locate the **Workstations** OU.
2. Right-click **Workstations**.
3. Select **Link an Existing GPO**.
4. Select **Microsoft Defender Policy**.
5. Click **OK**.

---

# Step 4 — Edit the GPO

1. Right-click **Microsoft Defender Policy**.
2. Select **Edit**.

The **Group Policy Management Editor** opens.

---

# Step 5 — Navigate to Microsoft Defender Policies

Expand:

```text
Computer Configuration
    └── Policies
         └── Administrative Templates
              └── Windows Components
                   └── Microsoft Defender Antivirus
```

Most Microsoft Defender settings are configured under **Computer Configuration** because they apply to the computer regardless of the signed-in user.

---

# Step 6 — Enable Real-time Protection

1. Expand **Real-time Protection**.
2. Locate the policy:

```text
Turn off real-time protection
```

3. Double-click the policy.

4. Select:

```text
Disabled
```

> Setting **Turn off real-time protection** to **Disabled** ensures that real-time protection remains enabled.

5. Click **Apply**.
6. Click **OK**.

---

# Step 7 — Configure Cloud-delivered Protection

1. Return to:

```text
Microsoft Defender Antivirus
```

2. Open:

```text
MAPS
```

3. Double-click:

```text
Join Microsoft MAPS
```

4. Select:

```text
Enabled
```

5. Choose the appropriate membership option for your environment (for example, **Basic** or **Advanced**, depending on your organization's policy and privacy requirements).

6. Click **Apply**.

7. Click **OK**.

---

# Step 8 — Configure Automatic Sample Submission

1. In the **MAPS** folder, locate:

```text
Send file samples when further analysis is required
```

2. Double-click the policy.

3. Select:

```text
Enabled
```

4. Choose the sample submission option that aligns with your organization's security and privacy policy.

5. Click **Apply**.

6. Click **OK**.

---

# Step 9 — Configure Scheduled Scans

Expand:

```text
Microsoft Defender Antivirus
    └── Scan
```

Configure the desired scan schedule based on your organization's requirements. For example, you may configure a weekly quick scan or a regular full scan during maintenance windows. Select the appropriate policies, enable them where required, and define the schedule.

> Scan scheduling options can vary between Windows and Windows Server versions. Verify the available settings in your environment.

---

# Step 10 — Configure Exclusions (Lab Demonstration)

Expand:

```text
Microsoft Defender Antivirus
    └── Exclusions
```

For demonstration purposes only, you may configure a sample exclusion.

For example:

```text
C:\Lab
```

> **Important:** Exclusions reduce antivirus coverage. Only create exclusions when there is a documented business or technical requirement, and review them regularly.

---

# Step 11 — Close the Group Policy Editor

Close the editor window.

The settings are saved automatically.

---

# Step 12 — Force Group Policy Update

On **DC01**, open **Windows PowerShell** as Administrator.

Run:

```powershell
gpupdate /force
```

Wait until the command completes successfully.

---

# Step 13 — Update CLIENT01

Sign in to **CLIENT01**.

Open **Windows PowerShell**.

Run:

```powershell
gpupdate /force
```

Restart the computer if prompted.

---

# Step 14 — Verify Policy Application

On **CLIENT01**, run:

```powershell
gpresult /r
```

Confirm that:

```text
Microsoft Defender Policy
```

appears under **Applied Group Policy Objects**.

You can also generate a detailed report:

```powershell
gpresult /h C:\DefenderPolicyReport.html
```

Open the generated report and review the applied settings.

---

# Step 15 — Verify Defender Configuration

On **CLIENT01**, open **Windows Security**.

Navigate to:

```text
Virus & threat protection
```

Review the configured settings and confirm that they reflect the applied Group Policy where applicable.

---

# Expected Results

After completing this guide:

- Microsoft Defender settings are centrally managed through Group Policy.
- Real-time protection remains enabled.
- Cloud-delivered protection is configured.
- Sample submission is configured according to organizational policy.
- Clients receive the Defender configuration after Group Policy refresh.

---

# Common Mistakes

- Disabling Defender instead of configuring it.
- Applying the GPO to the wrong Organizational Unit.
- Forgetting to refresh Group Policy.
- Creating unnecessary antivirus exclusions.

---

# Troubleshooting

## Defender Policies Do Not Apply

- Confirm the GPO is linked to the correct OU.
- Run:

```powershell
gpresult /r
```

- Verify the GPO appears under applied policies.
- Review the **Applications and Services Logs → Microsoft → Windows → GroupPolicy** logs in **Event Viewer** for processing errors.

---

# Rollback Procedure

To remove these configurations:

1. Open **Group Policy Management**.
2. Edit **Microsoft Defender Policy**.
3. Return modified settings to **Not Configured**.
4. Close the editor.
5. Run:

```powershell
gpupdate /force
```

6. Verify the updated configuration on **CLIENT01**.

---

# Best Practices

- Use a dedicated GPO for Microsoft Defender settings.
- Test changes before deploying broadly.
- Review exclusions periodically.
- Keep endpoint protection enabled unless another centrally managed antivirus solution has replaced it.
- Document all security policy changes.

---

# Lessons Learned

Microsoft Defender can be centrally managed using Group Policy to provide a consistent and secure endpoint protection configuration across domain-joined computers. Separating Defender settings into a dedicated GPO improves organization, simplifies maintenance, and supports enterprise security practices.

---

# Conclusion

You have successfully created and deployed a Group Policy Object to manage Microsoft Defender Antivirus settings. Centralized endpoint protection helps ensure that all managed computers receive consistent security configurations and reduces the need for manual administration.
