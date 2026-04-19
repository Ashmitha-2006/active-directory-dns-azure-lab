# 🖥️ Lab 3 — Active Directory & Account Management on Azure

> **Course:** NIT2122 | **Assessment:** Lab Assessment 1  
> **Platform:** Azure Virtual Machine (Windows Server 2016)  
> **Total Points:** 100 pts — 15 screenshots × 5 pts + 5 questions × 5 pts

---

## 📋 Table of Contents

- [Overview](#overview)
- [Submission Requirements](#submission-requirements)
- [Activity 3-1: Installing DNS Server](#activity-3-1-installing-dns-server)
- [Activity 3-2: Installing Active Directory](#activity-3-2-installing-active-directory)
- [Activity 3-3: Manage Organisational Units (OUs)](#activity-3-3-manage-organisational-units-ous)
- [Activity 3-4: Creating User Accounts in Active Directory](#activity-3-4-creating-user-accounts-in-active-directory)
- [Penalty & Deduction Policy](#penalty--deduction-policy)

---

## Overview

This lab walks through the full lifecycle of standing up an Active Directory environment on a Windows Server 2016 Azure VM — from installing the DNS Server role through to creating, renaming, and relocating user accounts inside a delegated Organisational Unit (OU).

| Activity | Topic | Screenshots Required |
|----------|-------|---------------------|
| 3-1 | Installing DNS Server | 3 |
| 3-2 | Installing Active Directory | 5 |
| 3-3 | Managing Organisational Units | 3 |
| 3-4 | Creating User Accounts | 4 |
| **Total** | | **15 screenshots + 5 answers** |

> ⚠️ **All screenshots must show the date and time from the Azure VM taskbar.**

---

## Submission Requirements

- Rename the Word document to: `Lab_Assessment_1_YourName_YourID.docx`
- Insert all 15 captured screens underneath their respective step instructions
- Upload via the **dropbox on VU Collaborate**

---

## Activity 3-1: Installing DNS Server

**Objective:** Install the DNS Server role on Windows Server 2016.

**Screenshots required at:** Steps 1.0, 1.8, and 1.11

---

### Step 1.0 — Verify Server Info via PowerShell

Open a **PowerShell** prompt and run:

```powershell
date; ipconfig
```

> Confirm the date/time is visible in the Azure VM taskbar before capturing.

![Figure 1.0 – Windows Server 2016 Info 1](screenshot1.png)

---

### Steps 1.1 – 1.7 — Open Server Manager & Add DNS Role

| Step | Action |
|------|--------|
| 1.1 | Open **Server Manager** |
| 1.2 | Ensure **Dashboard** is selected in the left pane |
| 1.3 | Click **Add roles and features** in the right pane |
| 1.4 | If the *Before you begin* window appears, click **Next** |
| 1.5 | Ensure **Role-based or feature-based installation** is selected → **Next** |
| 1.6 | Verify your server is selected in *Select destination server* → **Next** |
| 1.7 | Check the box for **DNS Server** |

---

### Step 1.8 — Add Features for DNS Server

In the **Add Roles and Features Wizard** dialog box, note the tools that will be installed alongside DNS. Click **Add Features**.

![Figure 1.8 – Add Roles and Features Wizard (DNS)](screenshot2.png)

---

### Steps 1.9 – 1.10 — Proceed Through the Wizard

| Step | Action |
|------|--------|
| 1.9 | If a warning appears, click **Continue**. Confirm **DNS Server** box is checked → **Next** |
| 1.10 | In *Select Features*, click **Next**. In *DNS Server*, click **Next** |

---

### Step 1.11 — Install DNS Server

In the **Confirmation** window, click **Install** and wait for the installation to complete.

![Figure 1.11 – DNS Server Installation Complete](screenshot3.png)

---

## Activity 3-2: Installing Active Directory

**Objective:** Install the Active Directory Domain Services (AD DS) role and promote the server to a domain controller.

**Screenshots required at:** Steps 2.0, 2.8, 2.15, 2.18, and 2.25  
**Questions required at:** Steps 2.11 and 2.23

---

### Step 2.0 — Verify Server Info via PowerShell

Open a **PowerShell** prompt and run:

```powershell
date; ipconfig /all
```

> Confirm the date/time is visible in the Azure VM taskbar before capturing.

![Figure 2.0 – Windows Server 2016 Info 2](screenshot4.png)

---

### Steps 2.1 – 2.7 — Open Server Manager & Add AD DS Role

| Step | Action |
|------|--------|
| 2.1 | Open **Server Manager** |
| 2.2 | Ensure **Dashboard** is selected in the left pane |
| 2.3 | Click **Add roles and features** |
| 2.4 | If *Before you begin* appears, click **Next** |
| 2.5 | Ensure **Role-based or feature-based installation** is selected → **Next** |
| 2.6 | Verify your server is selected → **Next** |
| 2.7 | Check the box for **Active Directory Domain Services** |

---

### Step 2.8 — Add Features for AD DS

In the **Add Roles and Features Wizard** dialog box, note the tools to be installed with Active Directory. Click **Add Features**.

![Figure 2.8 – Add Roles and Features Wizard (AD DS)](screenshot5.png)

---

### Steps 2.9 – 2.14 — Proceed Through the Wizard

| Step | Action |
|------|--------|
| 2.9 | Confirm **Active Directory Domain Services** box is checked → **Next** |
| 2.10 | Click **Next** in *Select features* |
| 2.12 | Click **Next** after reading the AD DS information |
| 2.13 | In *Confirm installation selections*, click **Install** |
| 2.14 | Wait for the installation to complete |

> **❓ Question 2.11** — *How many domain controllers are recommended as a minimum for a single domain?*  
> **Answer:** Two

---

### Step 2.15 — Review Installation Progress

Review the information in the **Installation progress** window confirming the installation succeeded.

![Figure 2.15 – AD DS Installation Progress](screenshot6.png)

---

### Steps 2.16 – 2.17 — Promote Server to Domain Controller

1. Click **Close**
2. In Server Manager, click the **yellow caution exclamation point** → **Promote this server to a domain controller**
3. The **Active Directory Domain Services Configuration Wizard** opens

---

### Step 2.18 — Configure New Forest

Select **Add a new forest**. Enter your root domain name as **YourID** (e.g. `s1234567`) and click **Next**.

![Figure 2.18 – Add New Forest Configuration](screenshot7.png)

---

### Steps 2.19 – 2.24 — Complete Forest Configuration

| Step | Action |
|------|--------|
| 2.19 | Set **Forest functional level** to **Windows Server 2016** |
| 2.20 | Set DSRM password to `NITpassw0rd!` → confirm → **Next** |
| 2.21 | Ignore DNS delegation warning → **Next** |
| 2.22 | Verify **NetBIOS name** (e.g. YourID) → **Next** |
| 2.24 | Click **Next** in the Paths window (leave defaults) |

> **❓ Question 2.23** — *Record the location of the database, log files, and SYSVOL folders:*
>
> | Folder | Default Path |
> |--------|-------------|
> | Database | `C:\Windows\NTDS` |
> | Log Files | `C:\Windows\NTDS` |
> | SYSVOL | `C:\Windows\SYSVOL` |

---

### Step 2.25 — Review Selections & View Script

Review all selections you have made, then click **View Script** to inspect the PowerShell script generated by the wizard.

![Figure 2.25 – Review Selections and View Script](screenshot8.png)

---

### Steps 2.26 – 2.29 — Finalise Installation

| Step | Action |
|------|--------|
| 2.26 | Click **Next** — the wizard runs a prerequisites check |
| 2.27 | Click **Install** (takes a few minutes) |
| 2.28 | Click **Close** and wait for the server to reboot |
| 2.29 | Sign back in after the reboot |

---

## Activity 3-3: Manage Organisational Units (OUs)

**Objective:** Create an OU and delegate control over it.

**Screenshots required at:** Steps 3.0, 3.3, and 3.16  
**Questions required at:** Step 3.5

---

### Step 3.0 — Verify Server Info via PowerShell

Open a **PowerShell** prompt and run:

```powershell
date; ipconfig /all
```

> Confirm the date/time is visible in the Azure VM taskbar before capturing.

![Figure 3.0 – Windows Server 2016 Info 3](screenshot9.png)

---

### Steps 3.1 – 3.2 — Open Active Directory Users and Computers

1. In **Server Manager**, click **Tools** → **Active Directory Users and Computers**
2. Right-click the top domain in the left pane → **New** → **Organizational Unit**

---

### Step 3.3 — Create the OU

Enter **AUSYourID** (e.g. `AUS1234567`, using your 7-digit student ID **without** the `s`). Click **OK**.

![Figure 3.3 – New Organisational Unit Created](screenshot10.png)

---

### Steps 3.4 – 3.15 — Delegate Control Over the OU

| Step | Action |
|------|--------|
| 3.4 | Expand the domain tree to view the new OU |
| 3.6 | Click **Delegate Control** |
| 3.7 | Click **Next** in the Delegation of Control Wizard |
| 3.8 | Click **Add** |
| 3.9 | Click **Advanced** |
| 3.10 | Click **Find Now** |
| 3.11 | Select **Student** → **OK** |
| 3.12 | Click **OK** in the *Select Users, Computers, or Groups* dialog |
| 3.13 | Click **Next** |
| 3.14 | Check **Create, delete, and manage user accounts** |
| 3.15 | Click **Next** |

> **❓ Question 3.5** — *What options are available on the right-click menu for the OU?*
>
> - Delegate Control  
> - Move  
> - Rename  
> - Delete  
> - Properties  
> - New

---

### Step 3.16 — Review Delegation Tasks & Finish

Review the delegated tasks and click **Finish**.

![Figure 3.16 – Delegation of Control Complete](screenshot11.png)

> Close the **Active Directory Users and Computers** window.

---

## Activity 3-4: Creating User Accounts in Active Directory

**Objective:** Create, configure, rename, and move a user account in Active Directory.

**Screenshots required at:** Steps 4.0, 4.9, 4.17, and 4.22  
**Questions required at:** Steps 4.3 and 4.5

---

### Step 4.0 — Verify Server Info via PowerShell

Open a **PowerShell** prompt and run:

```powershell
date; ipconfig /all
```

> Confirm the date/time is visible in the Azure VM taskbar before capturing.

![Figure 4.0 – Windows Server 2016 Info 4](screenshot12.png)

---

### Steps 4.1 – 4.2 — Open MMC with AD Users and Computers Snap-in

1. Right-click **Start** → **Run** → type `mmc` → **OK**
2. Click **File** → **Add/Remove Snap-in** → select **Active Directory Users and Computers** → **Add** → **OK**
3. Expand the domain name in the left pane

> **❓ Question 4.3** — *How many user accounts and groups are already in the Users folder?*
>
> - **User Accounts:** 2  
> - **Groups:** 19

---

### Steps 4.4 – 4.8 — Create a New User Account

1. Click **Action** menu (or right-click **Users**) → **New** → **User**
2. Enter your **First name** and **Last name**
3. Set **User logon name** to `YourFirstName.LastName`

> **❓ Question 4.5** — *Which fields are automatically completed? Which are not?*
>
> | Status | Fields |
> |--------|--------|
> | ✅ Auto-completed | Full Name, User logon name (Pre-Windows 2000) |
> | ✗ Manual entry | First Name, Last Name, User logon name (main), Password |

4. Click **Next** → enter and confirm a password → check **User must change password at next logon** → **Next**

---

### Step 4.9 — Verify and Finish Account Creation

Verify all entered information and click **Finish**.

![Figure 4.9 – New User Account Verification](screenshot13.png)

---

### Steps 4.10 – 4.16 — Configure and Rename the Account

| Step | Action |
|------|--------|
| 4.10 | Double-click the new account to open Properties |
| 4.12 | In the **General** tab, add a description (e.g. `Test account`) |
| 4.15 | Click **OK** |
| 4.16 | Right-click the account → **Rename** → enter your lecturer's name (e.g. `Hao Shi`) → press **Enter** |

---

### Step 4.17 — Update Rename User Details

In the **Rename User** dialog box, update:
- **First name** and **Last name** to the new name
- **User logon name** (e.g. `Hao.Shi`)

Click **OK**.

![Figure 4.17 – Rename User Dialog](screenshot14.png)

---

### Steps 4.18 – 4.21 — Move Account to OU

1. Confirm the account is listed as `Hao Shi` in the Users folder
2. Right-click the renamed account → **Move**
3. In the **Move** dialog, select the OU created in Activity 3-3 (e.g. `AUSYourID`) → **OK**

---

### Step 4.22 — Verify Account is in the OU

Click the OU in the left pane and confirm in the middle pane that the account has been successfully moved.

![Figure 4.22 – Account Moved to OU Successfully](screenshot15.png)

---

## 🎉 Lab Complete!

Upload `Lab_Assessment_1_YourName_YourID.docx` via the dropbox on **VU Collaborate**, then power off your Azure Virtual Machine.

> **Note:** The submission was a late submission due to login issues on the VU platform.

---

## Penalty & Deduction Policy

| Condition | Penalty |
|-----------|---------|
| < 1 day late | −20 points |
| 1–3 days late | −30 points per day |
| After 3 days | **0 marks** |
| Wrong file name | −10 points |
| Screenshots missing Azure VM taskbar date/time | Up to −50 points |

---

*NIT2122 Lab Assessment 1 — Lab 3*
