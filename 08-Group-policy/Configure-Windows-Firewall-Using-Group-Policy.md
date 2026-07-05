# Configure Windows Defender Firewall Using Group Policy

## Executive Summary

Windows Defender Firewall is a host-based firewall that helps protect computers by controlling inbound and outbound network traffic. In an Active Directory environment, administrators can centrally manage firewall settings using Group Policy, ensuring that all domain-joined computers follow a consistent security baseline.

In this guide, you will:

- Create a dedicated Firewall Group Policy Object (GPO).
- Configure the Domain, Private, and Public firewall profiles.
- Create an inbound firewall rule.
- Create an outbound firewall rule.
- Link the GPO to the appropriate Organizational Unit (OU).
- Apply the policy to client computers.
- Verify that the firewall configuration has been successfully deployed.

---

# Business Justification

Centralized firewall management provides:

- Consistent endpoint protection.
- Reduced attack surface.
- Standardized network access rules.
- Simplified administration.
- Improved compliance with organizational security policies.
- Easier auditing of firewall configurations.

Rather than configuring every workstation individually, administrators can manage Windows Defender Firewall from a single location using Group Policy.

---

# Prerequisites

Before beginning, ensure that:

- Active Directory Domain Services is operational.
- The **corp.local** domain is functioning correctly.
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
| Tool | Group Policy Management Console |

---

# What You Will Configure

This GPO will:

- Ensure Windows Defender Firewall is enabled.
- Configure the Domain Profile.
- Configure the Private Profile.
- Configure the Public Profile.
- Create a sample inbound firewall rule.
- Create a sample outbound firewall rule.

---

# Step 1 — Open Group Policy Management

1. Sign in to **DC01**.
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

2. Right-click **Group Policy Objects**.
3. Select **New**.
4. Name the GPO:

```text
Windows Firewall Policy
```

5. Leave **Source Starter GPO** as **(None)**.
6. Click **OK**.

---

# Step 3 — Link the GPO

1. Right-click the **Workstations** Organizational Unit.
2. Select **Link an Existing GPO...**
3. Choose:

```text
Windows Firewall Policy
```

4. Click **OK**.

---

# Step 4 — Edit the GPO

1. Right-click **Windows Firewall Policy**.
2. Select **Edit**.

The **Group Policy Management Editor** opens.

---

# Step 5 — Navigate to Windows Defender Firewall Policies

Expand the following path:

```text
Computer Configuration
    └── Policies
         └── Windows Settings
              └── Security Settings
                   └── Windows Defender Firewall with Advanced Security
```

Click:

```text
Windows Defender Firewall with Advanced Security
```

The management console for firewall policies is displayed.

---

# Step 6 — Configure the Domain Profile

1. In the right pane, click:

```text
Windows Defender Firewall Properties
```

2. Open the **Domain Profile** tab.

3. Configure the following settings:

| Setting | Value |
|---------|-------|
| Firewall State | On (Recommended) |
| Inbound Connections | Block (Default) |
| Outbound Connections | Allow (Default) |

4. Click **Apply**.

---

# Step 7 — Configure the Private Profile

1. Open the **Private Profile** tab.

2. Configure:

| Setting | Value |
|---------|-------|
| Firewall State | On |
| Inbound Connections | Block |
| Outbound Connections | Allow |

3. Click **Apply**.

---

# Step 8 — Configure the Public Profile

1. Open the **Public Profile** tab.

2. Configure:

| Setting | Value |
|---------|-------|
| Firewall State | On |
| Inbound Connections | Block |
| Outbound Connections | Allow |

3. Click **Apply**.
4. Click **OK**.

---

# Step 9 — Create an Inbound Firewall Rule

1. In the left pane, click:

```text
Inbound Rules
```

2. Right-click **Inbound Rules**.
3. Select:

```text
New Rule...
```

4. In the **New Inbound Rule Wizard**, select:

```text
Port
```

5. Click **Next**.

6. Select:

```text
TCP
```

7. Specify the port:

```text
3389
```

> This example allows Remote Desktop Protocol (RDP) traffic. Only enable this rule if your environment requires Remote Desktop access.

8. Click **Next**.

9. Select:

```text
Allow the connection
```

10. Click **Next**.

11. Select the profiles where the rule should apply (typically **Domain**).

12. Click **Next**.

13. Name the rule:

```text
Allow RDP
```

14. Click **Finish**.

---

# Step 10 — Create an Outbound Firewall Rule

1. In the left pane, click:

```text
Outbound Rules
```

2. Right-click **Outbound Rules**.
3. Select:

```text
New Rule...
```

4. Follow the wizard to create a sample outbound rule if your environment requires one.

> In many enterprise environments, outbound traffic is allowed by default. Only create outbound rules when there is a specific security or business requirement.

---

# Step 11 — Close the Group Policy Editor

Review the configuration.

Close the editor window.

The settings are saved automatically.

---

# Step 12 — Force Group Policy Update

On **DC01**, open **Windows PowerShell** as Administrator.

Run:

```powershell
gpupdate /force
```

Wait until the update completes successfully.

---

# Step 13 — Update CLIENT01

1. Sign in to **CLIENT01**.
2. Open **Windows PowerShell**.
3. Run:

```powershell
gpupdate /force
```

Restart the computer if prompted.

---

# Step 14 — Verify Firewall Configuration

On **CLIENT01**:

1. Open **Windows Defender Firewall with Advanced Security**.
2. Review the **Domain**, **Private**, and **Public** profiles.
3. Confirm that the configured settings match the Group Policy.
4. Verify that the **Allow RDP** inbound rule appears if it was created.

---

# Step 15 — Verify the Applied Policy

Open **Windows PowerShell** on **CLIENT01**.

Run:

```powershell
gpresult /r
```

Confirm that:

```text
Windows Firewall Policy
```

appears under **Applied Group Policy Objects**.

Generate a detailed report:

```powershell
gpresult /h C:\FirewallPolicyReport.html
```

Review the report to confirm successful policy application.

---

# Expected Results

After completing this guide:

- Windows Defender Firewall is centrally managed.
- Firewall profiles are configured consistently.
- Required firewall rules are deployed through Group Policy.
- Client computers automatically receive firewall settings.

---

# Common Mistakes

- Disabling the firewall instead of configuring it.
- Allowing unnecessary inbound ports.
- Applying the GPO to the wrong OU.
- Forgetting to update Group Policy before testing.
- Creating duplicate or conflicting firewall rules.

---

# Troubleshooting

## Firewall Settings Are Not Applied

- Verify the computer is in the correct OU.
- Confirm the GPO is linked and enabled.
- Run:

```powershell
gpupdate /force
```

- Verify the policy with:

```powershell
gpresult /r
```

- Review the **Group Policy Operational** log in **Event Viewer**.
- Ensure no conflicting local firewall rules override the intended configuration where applicable.

---

# Rollback Procedure

To remove the firewall configuration:

1. Open **Group Policy Management**.
2. Edit **Windows Firewall Policy**.
3. Remove or revert the configured firewall rules and profile settings.
4. Close the editor.
5. Run:

```powershell
gpupdate /force
```

6. Verify that the updated configuration is applied on **CLIENT01**.

---

# Best Practices

- Keep Windows Defender Firewall enabled on all profiles unless there is a documented exception.
- Allow only the network traffic required for business operations.
- Use separate GPOs for firewall management where practical.
- Document every custom firewall rule and its business justification.
- Review firewall rules periodically to remove obsolete or unnecessary entries.

---

# Lessons Learned

Windows Defender Firewall is a critical layer of endpoint security. Managing firewall settings through Group Policy enables administrators to enforce consistent configurations, reduce the attack surface, and simplify firewall administration across domain-joined computers.

---

# Conclusion

You have successfully configured Windows Defender Firewall using Group Policy. Firewall profiles and rules are now centrally managed, providing a consistent and secure network configuration for domain-joined endpoints.
