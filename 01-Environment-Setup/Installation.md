# Windows Server 2022 Installation

## Overview

This document provides a step-by-step guide for installing Windows Server 2022. A properly installed operating system forms the foundation for deploying Active Directory Domain Services (AD DS), Domain Name System (DNS), and other enterprise infrastructure services.

The installation performed in this project uses **Windows Server 2022 Standard (Desktop Experience)** to simplify administration using both the graphical user interface (GUI) and Windows PowerShell.

---

# Objectives

Upon completion of this guide, you will have:

- Installed Windows Server 2022.
- Configured the local Administrator account.
- Successfully logged into the server.
- Verified a successful operating system installation.
- Prepared the server for initial configuration.

---

# Requirements

Before beginning the installation, ensure the following resources are available:

- Windows Server 2022 ISO
- Virtual Machine (Hyper-V, VMware Workstation, or VirtualBox) or Physical Server
- Minimum 4 GB RAM (8 GB Recommended)
- Minimum 60 GB Storage (100 GB Recommended)
- Internet Connection (Optional)

---

# Virtual Machine Configuration

The following virtual machine specifications were used for this deployment.

| Setting | Value |
|----------|-------|
| Operating System | Windows Server 2022 Standard |
| CPU | 2 vCPUs |
| Memory | 8 GB RAM |
| Disk Size | 100 GB |
| Network Adapter | Bridged / NAT / Internal Network |
| Firmware | UEFI |

---

# Installation Procedure

## Step 1 — Mount the Installation Media

Mount the Windows Server 2022 ISO to the virtual machine or physical server.

Power on the machine.

The Windows Setup wizard will start automatically.

---

## Step 2 — Select Regional Settings

Configure the installation preferences.

| Setting | Example |
|----------|---------|
| Language | English (United States) |
| Time and Currency Format | English (United States) |
| Keyboard Layout | US |

Click **Next**.

Select **Install Now**.

> **Screenshot Placeholder**
>
> `assets/images/windows-installation/language-selection.png`

---

## Step 3 — Select the Operating System Edition

Choose:

**Windows Server 2022 Standard Evaluation (Desktop Experience)**

or

**Windows Server 2022 Standard (Desktop Experience)**

> **Note**
>
> The Desktop Experience edition includes the graphical user interface (GUI), making administration easier for learning and management.

Click **Next**.

---

## Step 4 — Accept the License Agreement

Read the Microsoft Software License Terms.

Select:

- **I accept the Microsoft Software License Terms**

Click **Next**.

---

## Step 5 — Choose Installation Type

Select:

**Custom: Install Microsoft Server Operating System only (advanced)**

This performs a clean installation.

---

## Step 6 — Select the Installation Disk

Select the virtual hard disk.

If necessary:

- Create a new partition.
- Format the partition.
- Select the primary partition.

Click **Next**.

Windows Server installation will begin.

---

## Step 7 — Wait for Installation

The installation process includes:

- Copying Windows files
- Installing features
- Installing updates
- Completing installation

The server will restart automatically several times.

No user interaction is required during this stage.

---

## Step 8 — Configure the Administrator Password

After installation completes, Windows prompts for the local Administrator password.

Choose a password that meets Microsoft's complexity requirements.

Example:

```
Password@123
```

> **Note**
>
> Never use weak passwords in production environments.

Click **Finish**.

---

## Step 9 — Sign In

Press:

```
Ctrl + Alt + Delete
```

For virtual machines:

- VMware Workstation
- Hyper-V
- VirtualBox

Use the hypervisor's "Send Ctrl+Alt+Delete" option if required.

Sign in using:

```
Username:
Administrator
```

Enter the password configured in the previous step.

---

# Verify the Installation

Open **Command Prompt**.

Verify the hostname.

```cmd
hostname
```

Example Output

```text
WIN-4L9K2YJX2LQ
```

---

Verify the Windows version.

```cmd
winver
```

Confirm the operating system displays:

```
Microsoft Windows Server 2022 Standard
```

---

Verify the operating system information.

Open PowerShell.

```powershell
Get-ComputerInfo
```

Review the following properties:

- Windows Version
- Build Number
- Computer Name
- BIOS Information
- Installed Memory

---

Verify available disk space.

```powershell
Get-Volume
```

---

Verify network adapters.

```powershell
Get-NetAdapter
```

---

# Expected Result

Upon successful installation:

- Windows Server boots normally.
- Administrator login is successful.
- Desktop Experience is available.
- PowerShell launches successfully.
- Network adapters are detected.
- Storage volumes are accessible.

---

# Troubleshooting

## Installation Does Not Start

Possible Causes

- ISO not mounted
- Incorrect boot order
- Corrupted installation media

Resolution

- Verify the ISO image.
- Configure the VM to boot from the DVD/ISO.
- Restart the installation.

---

## No Disk Detected

Possible Causes

- Virtual disk not attached
- Storage controller misconfigured

Resolution

- Verify the virtual hard disk exists.
- Confirm the storage controller configuration.
- Restart the VM.

---

## Unable to Log In

Possible Causes

- Incorrect Administrator password
- Keyboard layout mismatch

Resolution

- Verify the keyboard layout.
- Confirm password complexity requirements.
- Reset the Administrator password if necessary.

---

# Best Practices

- Install the Desktop Experience edition unless Server Core is specifically required.
- Allocate sufficient memory and storage before deployment.
- Use UEFI firmware where supported.
- Install Windows on a dedicated system volume.
- Record the initial computer name.
- Document all installation settings.
- Create a virtual machine snapshot after the operating system installation.

---

# Installation Checklist

- [ ] Windows Server ISO mounted
- [ ] Operating System installed
- [ ] Administrator password configured
- [ ] Initial login successful
- [ ] Windows version verified
- [ ] Network adapter detected
- [ ] Disk space verified
- [ ] PowerShell operational
- [ ] Snapshot created (Optional)

---

# Summary

Windows Server 2022 has been successfully installed and verified. The server is now ready for post-installation configuration, including applying Windows updates, renaming the server, configuring networking, and preparing it for the installation of Active Directory Domain Services.
