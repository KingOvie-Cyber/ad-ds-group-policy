# Configure USB Storage Restrictions Using Group Policy

## Executive Summary

Removable USB storage devices provide a convenient way to transfer files between computers. However, they can also introduce malware, facilitate unauthorized data exfiltration, and create compliance risks.

Using Group Policy, administrators can centrally control whether users are permitted to read from or write to USB storage devices connected to domain-joined computers.

In this guide, you will create a dedicated Group Policy Object (GPO) to restrict access to USB storage devices, link it to the **Workstations** Organizational Unit (OU), apply the policy, and verify that the restriction is enforced on a domain-joined client.

> **Important:** This policy affects **USB storage devices** (such as flash drives and external USB hard drives). It does **not** block USB keyboards, USB mice, or other Human Interface Devices (HIDs).

---

# Business Justification

Organizations often restrict the use of removable storage to reduce security risks and meet regulatory requirements.

Restricting USB storage can help:

- Prevent unauthorized copying of sensitive data.
- Reduce the risk of malware introduced through removable media.
- Support data loss prevention (DLP) initiatives.
- Improve compliance with organizational security policies.
- Standardize endpoint security controls.

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

This GPO will configure the following policy:

| Policy | Configuration |
|---------|---------------|
| All Removable Storage classes: Deny all access | Enabled |

Enabling this policy prevents users from reading from or writing to USB storage devices.

---

# Step 1 — Open Group Policy Management

1. Sign in to **DC01** using a Domain Administrator account.
2. Click the **Start** button or press the **Windows** key.
3. Search for **Server Manager** and open it.
4. In the upper-right corner of **Server Manager**, click **Tools**.
5. Select **Group Policy Management**.
6. Wait for the **Group Policy Management Console** to load completely.

---

# Step 2 — Create a New Group Policy Object

1. In the left navigation pane, expand:

```text
Forest: corp.local
    └── Domains
         └── corp.local
```

2. Click **Group Policy Objects**.
3. Right-click **Group Policy Objects**.
4. Select **New**.
5. In the **Name** field, enter:

```text
USB Storage Restriction Policy
```

6. Leave **Source Starter GPO** set to **(None)**.
7. Click **OK**.

The new GPO is created successfully.

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
USB Storage Restriction Policy
```

5. Click **OK**.

The GPO is now linked to the Workstations OU.

---

# Step 4 — Edit the GPO

1. Under the **Workstations** OU or **Group Policy Objects**, locate **USB Storage Restriction Policy**.
2. Right-click the GPO.
3. Select **Edit**.

The **Group Policy Management Editor** opens.

---

# Step 5 — Navigate to Removable Storage Access Policies

In the left navigation pane, expand:

```text
Computer Configuration
    └── Policies
         └── Administrative Templates
              └── System
                   └── Removable Storage Access
```

Click **Removable Storage Access**.

The right pane displays the available removable storage policies.

---

# Step 6 — Configure the USB Storage Restriction

1. In the right pane, locate:

```text
All Removable Storage classes: Deny all access
```

2. Double-click the policy.

3. Select:

```text
Enabled
```

4. Click **Apply**.

5. Click **OK**.

When enabled, this policy blocks access to all removable storage classes covered by the policy.

---

# Step 7 — Review Additional Removable Storage Policies

Within the **Removable Storage Access** folder, review other available policies such as:

- Removable Disks: Deny read access
- Removable Disks: Deny write access
- CD and DVD restrictions
- WPD (Windows Portable Devices) restrictions

For this exercise, leave these settings as **Not Configured** unless your organization has a specific requirement.

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

Wait for the command to complete successfully.

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

# Step 11 — Test the Policy

1. After Group Policy has been updated, connect a USB flash drive to **CLIENT01**.
2. Open **File Explorer**.
3. Attempt to browse the USB storage device.

Depending on the Windows version and the configured policy, access should be denied or the device should not be usable for file operations.

---

# Step 12 — Verify the Applied Policy

On **CLIENT01**, open **Windows PowerShell** and run:

```powershell
gpresult /r
```

Confirm that:

```text
USB Storage Restriction Policy
```

appears under **Applied Group Policy Objects**.

Generate a detailed report:

```powershell
gpresult /h C:\USBRestrictionPolicy.html
```

Open the report and verify that the policy has been applied.

---

# Expected Results

After completing this guide:

- A dedicated USB Storage Restriction GPO has been created.
- The GPO is linked to the **Workstations** OU.
- USB storage access is restricted for targeted computers.
- Group Policy successfully applies the configured restriction.

---

# Common Mistakes

- Linking the GPO to the wrong Organizational Unit.
- Testing before running `gpupdate /force`.
- Assuming the policy blocks all USB devices, including keyboards and mice.
- Forgetting to verify the policy with `gpresult`.

---

# Troubleshooting

## USB Storage Is Still Accessible

- Verify the computer account is located in the **Workstations** OU.
- Confirm the GPO is linked and enabled.
- Run:

```powershell
gpresult /r
```

- Verify that **USB Storage Restriction Policy** appears under **Applied Group Policy Objects**.
- Restart **CLIENT01** if the policy requires a reboot to take full effect.
- Review the **GroupPolicy** logs in **Event Viewer** for any processing errors.

---

# Rollback Procedure

To remove the USB restriction:

1. Open **Group Policy Management**.
2. Edit **USB Storage Restriction Policy**.
3. Set **All Removable Storage classes: Deny all access** to **Not Configured**.
4. Close the editor.
5. Run:

```powershell
gpupdate /force
```

6. Restart **CLIENT01** if necessary.
7. Test USB storage access again.

---

# Best Practices

- Use a dedicated GPO for removable storage policies.
- Apply the policy only to the Organizational Units that require it.
- Test the policy on a small group of computers before broad deployment.
- Document all removable storage restrictions.
- Review exceptions periodically to ensure they remain justified.

---

# Lessons Learned

Group Policy provides an effective mechanism for centrally controlling the use of removable storage devices. Restricting USB storage helps reduce the risk of malware infection and unauthorized data transfer while allowing organizations to enforce consistent endpoint security controls.

---

# Conclusion

You have successfully configured a Group Policy Object to restrict USB storage devices on domain-joined workstations. This policy strengthens endpoint security and demonstrates how Group Policy can be used to manage hardware-related security settings across an Active Directory environment.
