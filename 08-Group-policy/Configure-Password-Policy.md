# Configure Password Policy and Account Lockout Policy

## Executive Summary

Password Policy and Account Lockout Policy are fundamental security controls in an Active Directory environment. Together, they help protect user accounts by enforcing strong password requirements and limiting repeated authentication attempts that could indicate a brute-force attack.

Unlike many other Group Policy settings, these policies are **domain account policies**. In a traditional Active Directory deployment, they should be configured in the **Default Domain Policy** or in another Group Policy Object that is linked at the **domain level** and intended to provide the domain's account policy.

This guide demonstrates how to configure both policies using the **Group Policy Management Console (GPMC)**, force policy replication, verify that the settings are active, and test them from a domain-joined client.

---

# Business Justification

Weak passwords and unlimited sign-in attempts significantly increase the risk of unauthorized access.

A properly configured Password Policy helps ensure that users create complex passwords that are difficult to guess or crack.

An Account Lockout Policy reduces the effectiveness of password-guessing attacks by temporarily locking an account after a defined number of failed sign-in attempts.

Together, these policies strengthen identity security across the domain.

---

# Prerequisites

Before continuing, verify the following:

- Active Directory Domain Services is installed and functioning.
- The domain **corp.local** is operational.
- The Domain Controller (**DC01**) is online.
- A domain-joined client (**CLIENT01**) is available for testing.
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

# What You Will Configure

### Password Policy

| Setting | Value |
|----------|-------|
| Enforce Password History | 24 passwords remembered |
| Maximum Password Age | 90 days |
| Minimum Password Age | 1 day |
| Minimum Password Length | 14 characters |
| Password Must Meet Complexity Requirements | Enabled |
| Store Passwords Using Reversible Encryption | Disabled |

---

### Account Lockout Policy

| Setting | Value |
|----------|-------|
| Account Lockout Threshold | 5 invalid logon attempts |
| Account Lockout Duration | 15 minutes |
| Reset Account Lockout Counter After | 15 minutes |

> **Note:** These values are example settings. Organizations should configure them according to their own security requirements and compliance obligations.

---

# Step 1 — Open Group Policy Management

1. Sign in to **DC01**.
2. Open **Server Manager**.
3. Click **Tools**.
4. Select **Group Policy Management**.
5. Wait for the console to load.

---

# Step 2 — Expand the Domain

In the left pane:

1. Expand **Forest: corp.local**.
2. Expand **Domains**.
3. Expand **corp.local**.

You should now see:

```text
Default Domain Policy
Default Domain Controllers Policy
```

---

# Step 3 — Edit the Default Domain Policy

1. Under **corp.local**, locate **Default Domain Policy**.
2. Right-click **Default Domain Policy**.
3. Select **Edit**.

The **Group Policy Management Editor** opens.

---

# Step 4 — Navigate to Password Policy

In the left navigation pane, expand the following path:

```text
Computer Configuration
    └── Policies
          └── Windows Settings
                └── Security Settings
                      └── Account Policies
                            └── Password Policy
```

Click **Password Policy**.

The right pane displays the available password policy settings.

---

# Step 5 — Configure Password History

1. Double-click **Enforce password history**.
2. Select **Define this policy setting** if necessary.
3. Enter:

```text
24
```

4. Click **Apply**.
5. Click **OK**.

---

# Step 6 — Configure Maximum Password Age

1. Double-click **Maximum password age**.
2. Enter:

```text
90
```

3. Click **Apply**.
4. Click **OK**.

---

# Step 7 — Configure Minimum Password Age

1. Double-click **Minimum password age**.
2. Enter:

```text
1
```

3. Click **Apply**.
4. Click **OK**.

---

# Step 8 — Configure Minimum Password Length

1. Double-click **Minimum password length**.
2. Enter:

```text
14
```

3. Click **Apply**.
4. Click **OK**.

---

# Step 9 — Enable Password Complexity

1. Double-click **Password must meet complexity requirements**.
2. Select **Enabled**.
3. Click **Apply**.
4. Click **OK**.

---

# Step 10 — Verify Reversible Encryption Is Disabled

1. Double-click **Store passwords using reversible encryption**.
2. Confirm that the setting is **Disabled**.
3. Click **OK** if no changes are required.

---

# Step 11 — Configure Account Lockout Policy

In the left pane, click:

```text
Account Lockout Policy
```

---

## Configure Account Lockout Threshold

1. Double-click **Account lockout threshold**.
2. Enter:

```text
5
```

3. Windows will suggest values for the remaining lockout settings. Review them and adjust if your organization's policy requires different values.
4. Click **OK**.

---

## Configure Account Lockout Duration

1. Double-click **Account lockout duration**.
2. Set:

```text
15 minutes
```

3. Click **Apply**.
4. Click **OK**.

---

## Configure Reset Counter

1. Double-click **Reset account lockout counter after**.
2. Set:

```text
15 minutes
```

3. Click **Apply**.
4. Click **OK**.

---

# Step 12 — Close the Group Policy Management Editor

Close the editor window after saving all changes.

---

# Step 13 — Force Group Policy Update

On **DC01**, open **Windows PowerShell** as Administrator and run:

```powershell
gpupdate /force
```

Wait for the update to complete successfully.

---

# Step 14 — Test the Policy on CLIENT01

1. Sign in to **CLIENT01** with a domain account.
2. Open **Command Prompt** or **Windows PowerShell**.
3. Run:

```powershell
gpupdate /force
```

4. Sign out and sign back in if prompted.

---

# Step 15 — Verify the Password Policy

On **CLIENT01**, open **Command Prompt** and run:

```cmd
net accounts
```

Review the output to confirm that the password and account lockout settings match the configured policy.

You can also generate a Group Policy report:

```powershell
gpresult /r
```

Confirm that the **Default Domain Policy** is listed under **Applied Group Policy Objects**.

---

# Expected Results

After completing this exercise:

- Password complexity is enforced.
- Password history is configured.
- Minimum and maximum password ages are active.
- Account lockout protects against repeated failed sign-in attempts.
- Domain users receive the configured account policy.

---

# Common Mistakes

- Configuring Password Policy in an OU-linked GPO instead of the domain policy.
- Forgetting to run `gpupdate /force`.
- Testing with a local account instead of a domain account.
- Assuming policy changes apply instantly without refreshing Group Policy.

---

# Troubleshooting

## Policy Does Not Apply

- Verify the policy was configured in the correct GPO.
- Confirm the GPO is linked at the domain level.
- Run `gpresult /r`.
- Check the **System** and **GroupPolicy** logs in **Event Viewer** for processing errors.

---

# Best Practices

- Use strong password requirements that align with your organization's security policy.
- Regularly review account lockout settings to balance usability and security.
- Document all changes to domain account policies.
- Test policy changes in a lab before deploying to production.

---

# Lessons Learned

Password Policy and Account Lockout Policy are among the most important security controls in Active Directory. Configuring them correctly at the domain level helps protect user identities and reduces the risk of unauthorized access.

---

# Conclusion

You have successfully configured domain-wide Password Policy and Account Lockout Policy. These settings now provide a baseline level of authentication security for all domain users in the **corp.local** environment.
