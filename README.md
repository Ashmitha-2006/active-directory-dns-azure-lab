# Active Directory & DNS Configuration on Azure


---

## Overview

This lab documents the end-to-end deployment of a DNS Server and Active Directory Domain Services (AD DS) environment on a Windows Server 2016 Azure virtual machine. The work covers four sequential activities: installing DNS, promoting the server to a domain controller, creating and delegating an Organisational Unit (OU), and managing user accounts within that OU.

The environment was accessed remotely via Azure Remote Desktop and all configuration was performed through Server Manager and Active Directory Users and Computers (ADUC).

---

## Environment

| Component | Detail |
|-----------|--------|
| Cloud Platform | Microsoft Azure |
| Operating System | Windows Server 2016 |
| Domain Name | S8200500.com |
| NetBIOS Name | S8200500 |
| Server Hostname | WIN-9N685LPPR2S |
| IPv4 Address | 10.0.2.15 |
| Default Gateway | 10.0.2.2 |
| Access Method | Remote Desktop Connection (RDP) |

---

## Table of Contents

1. [Activity 3-1 — Installing DNS Server](#activity-3-1--installing-dns-server)
2. [Activity 3-2 — Installing Active Directory](#activity-3-2--installing-active-directory)
3. [Activity 3-3 — Managing Organisational Units](#activity-3-3--managing-organisational-units)
4. [Activity 3-4 — Creating User Accounts](#activity-3-4--creating-user-accounts)
5. [Assessment Answers](#assessment-answers)

---

## Activity 3-1 — Installing DNS Server

**Objective:** Install the DNS Server role on Windows Server 2016 using Server Manager.

DNS (Domain Name System) is a prerequisite for Active Directory. It translates hostnames to IP addresses and is required before the AD DS role can be promoted. In Azure environments, it is recommended to install DNS on the same server as Active Directory.

---

### Step 1.0 — Verify Server Network Configuration

Before beginning any role installation, the server's current date and network configuration was verified using PowerShell. This establishes a baseline record of the machine identity and IP addressing.

```powershell
date; ipconfig
```

![Step 1.0 — PowerShell ipconfig output on Azure VM](screenshot1.png)

*The screenshot confirms the Azure VM is active and the network adapter is correctly configured with an IP address assigned via the Azure virtual network.*

---

### Step 1.8 — Add Required Features for DNS Server

In Server Manager, after selecting the **DNS Server** role, the Add Roles and Features Wizard prompted for the installation of additional management tools. These include **Remote Server Administration Tools**, **Role Administration Tools**, and **DNS Server Tools**.

Clicking **Add Features** ensures the full DNS management toolset is installed alongside the core role.

![Step 1.8 — Add Features required for DNS Server](screenshot12.png)

*The wizard lists all dependent tools: Remote Server Administration Tools → Role Administration Tools → DNS Server Tools.*

---

### Step 1.11 — DNS Installation Complete

After confirming the selection in the Confirmation window, installation was initiated. The Installation progress window confirms the DNS Server role was successfully installed on the server.

![Step 1.11 — DNS Server installation succeeded](screenshot10.png)

*Installation result: **DNS Server**, Remote Server Administration Tools, Role Administration Tools, and DNS Server Tools were all installed successfully on WIN-9N685LPPR2S.*

---

## Activity 3-2 — Installing Active Directory

**Objective:** Install the Active Directory Domain Services (AD DS) role and promote the server to a domain controller by creating a new forest.

AD DS is the core identity service that enables centralised authentication, user management, and Group Policy across the network. Promoting a server to a domain controller makes it the authoritative source for the domain.

---

### Step 2.0 — Verify Server Network Configuration

Network configuration was re-verified via PowerShell before beginning the AD DS installation, confirming the server was operating as Administrator and the network settings were unchanged.

```powershell
date; ipconfig
```

![Step 2.0 — PowerShell date and ipconfig output](screenshot5.png)

*Output confirms: Thursday, November 20, 2025 — 3:20 AM. IPv4 Address: 10.0.2.15. The server is ready for AD DS installation.*

---

### Step 2.8 — Add Required Features for Active Directory Domain Services

After selecting the **Active Directory Domain Services** role in Server Manager, the wizard prompted for additional dependencies. These include Group Policy Management, Remote Server Administration Tools, AD DS and AD LDS Tools, the Active Directory module for Windows PowerShell, and AD DS Snap-Ins and Command-Line Tools.

![Step 2.8 — Add Features required for AD DS](screenshot6.png)

*All required AD DS management tools and PowerShell modules are included. The "Include management tools (if applicable)" checkbox is selected, ensuring the full administration suite is installed.*

---

### Step 2.15 — Active Directory Installation Complete

Following the installation, the results window confirmed that the AD DS role was successfully installed. The wizard also flagged the next required action — promoting the server to a domain controller.

![Step 2.15 — AD DS installation progress and result](screenshot2.png)

*The results pane confirms installation succeeded and displays the message: "Additional steps are required to make this machine a domain controller." The **Promote this server to a domain controller** link is visible, which initiates the AD DS Configuration Wizard.*

---

### Step 2.18 — Configure New Forest and Root Domain

Using the Active Directory Domain Services Configuration Wizard, the server was configured as a new domain controller by creating a new forest. The root domain name was set to the student ID.

**Configuration selected:**
- Deployment operation: **Add a new forest**
- Root domain name: `S8200500.com`
- Forest functional level: Windows Server 2016
- Domain functional level: Windows Server 2016
- DSRM password: as configured

![Step 2.18 — Deployment Configuration — Add a new forest](screenshot14.png)

*"Add a new forest" is selected and the root domain name S8200500.com has been entered. This creates a brand-new Active Directory forest with this server as the first and only domain controller.*

---

### Step 2.25 — Review Configuration Script

Before finalising the domain controller promotion, the **View Script** button was clicked to inspect the auto-generated PowerShell script. This script can be used to replicate the exact configuration on additional servers, which is important for automation and disaster recovery.

![Step 2.25 — Auto-generated PowerShell AD DS deployment script](screenshot11.png)

*The script uses `Install-ADDSForest` with the configured parameters including `-DomainName "S8200500.com"`, `-DatabasePath "C:\Windows\NTDS"`, `-SysvolPath "C:\Windows\SYSVOL"`, and `-ForestMode "WinThreshold"` (Windows Server 2016). This confirms all paths and settings before installation begins.*

> **Key configuration paths recorded at Step 2.23:**
> | Folder | Default Path |
> |--------|-------------|
> | Database | `C:\Windows\NTDS` |
> | Log Files | `C:\Windows\NTDS` |
> | SYSVOL | `C:\Windows\SYSVOL` |

After reviewing, **Next** was clicked to run the prerequisites check, then **Install** to complete the promotion. The server rebooted automatically on completion.

---

## Activity 3-3 — Managing Organisational Units

**Objective:** Create an Organisational Unit (OU) within Active Directory and delegate user account management control over it to another user.

Organisational Units allow administrators to structure Active Directory to reflect an organisation's hierarchy — by department, location, or function. Delegation of Control enables a non-admin user to manage accounts within a specific OU without being granted full domain admin rights.

---

### Step 3.0 — Verify Server Network Configuration (Post-Reboot)

After the server rebooted following the AD DS promotion, network configuration was verified again to confirm the domain was operational and the IP configuration had persisted correctly.

```powershell
date; ipconfig
```

![Step 3.0 — PowerShell ipconfig after domain controller reboot](screenshot8.png)

*Output confirms: Thursday, November 20, 2025 — 3:37 AM. The server has successfully rebooted and the Administrator is now logged into the new domain S8200500.com. Network configuration is unchanged.*

---

### Step 3.3 — Create the Organisational Unit

Active Directory Users and Computers (ADUC) was opened via **Server Manager → Tools**. The domain `S8200500.com` was right-clicked in the left pane and **New → Organizational Unit** was selected. The OU was named `AUS8200500` following the naming convention `AUSYourID`.

![Step 3.3 — Active Directory Users and Computers with new domain structure](screenshot7.png)

*ADUC is open and the S8200500.com domain is visible. The domain containers are listed in the right pane: Builtin, Computers, Domain Controllers, ForeignSecurityPrincipals, Managed Service Accounts, and Users — confirming the domain was promoted successfully and is ready for OU creation.*

---

### Step 3.16 — Delegation of Control Complete

The Delegation of Control Wizard was completed, assigning the **Guest** account (S8200500\Guest) management rights over the `AUS8200500` OU. The delegated task was: *Create, delete, and manage user accounts*.

![Step 3.16 — Delegation of Control Wizard complete](screenshot3.png)

*The wizard summary confirms delegation was applied to `S8200500.com/` and that the selected user has been granted control. Clicking **Finish** commits the change in Active Directory.*

> **Question 3.5 — Options available on right-clicking the OU:**
> `Delegate Control` · `Move` · `Rename` · `Delete` · `Properties` · `New`

---

## Activity 3-4 — Creating User Accounts

**Objective:** Create a new user account in Active Directory, configure it, rename it, and move it into the delegated OU.

User accounts are the foundation of identity management in Active Directory. This activity covers the full lifecycle of a single account: creation, configuration, renaming, and relocation.

---

### Step 4.0 — Verify Server Network Configuration

Network configuration was verified for the final time before beginning user account work.

```powershell
date; ipconfig
```

![Step 4.0 — PowerShell date and ipconfig before user account creation](screenshot13.png)

*Output confirms: Thursday, November 20, 2025 — 3:53 AM. The domain controller is operational and the administrator session is active, ready for user account management.*

---

### Step 4.9 — Verify New User Account and Click Finish

A new user account was created through the MMC console with the Active Directory Users and Computers snap-in. The account details entered were:

- **Full name:** Dishini Ashmitha
- **User logon name:** `8200500@S8200500.com`
- **Password policy:** User must change password at next logon

The final confirmation screen was reviewed before clicking **Finish**.

![Step 4.9 — New user account verification before completion](screenshot9.png)

*The "New Object – User" dialog confirms the account will be created in `S8200500.com/` with the user logon name `8200500@S8200500.com`. The password change requirement at next logon is enforced, which is a security best practice.*

> **Question 4.3 — User accounts and groups in the Users folder:**
> - User accounts already created: **2**
> - Groups shown: **19**

> **Question 4.5 — Auto-completed vs manual fields:**
> | Status | Fields |
> |--------|--------|
> | Auto-completed | Full Name, User logon name (Pre-Windows 2000) |
> | Manual entry required | First Name, Last Name, User logon name (primary), Password |

---

### Step 4.17 — Account Renamed in Active Directory

The newly created account was renamed to reflect the lecturer's name — **Hao Shi** — and the user logon name was updated to `Hao.Shi`. The Active Directory Users and Computers console confirms the account now appears under the domain with the updated name.

![Step 4.17 — Active Directory Users and Computers showing renamed user Hao Shi](screenshot4.png)

*The ADUC console displays the renamed account "Hao Shi" with type User listed in the domain. The Rename User dialog was used to update the First name, Last name, and User logon name fields simultaneously.*

---

### Step 4.22 — Account Moved to Organisational Unit

The renamed account was right-clicked and **Move** was selected. The `AUS8200500` OU (created in Activity 3-3) was chosen as the destination. The left pane was then clicked to navigate to the OU and confirm the account appeared in the correct location.

![Step 4.22 — Hao Shi account confirmed in the domain after move](screenshot15.png)

*Active Directory Users and Computers confirms that the user account "Hao Shi" is present and selected (highlighted) within the domain. The account has been successfully relocated from the default Users container to the delegated Organisational Unit.*

---

## Assessment Answers

| Step | Question | Answer |
|------|----------|--------|
| 2.11 | Minimum number of recommended domain controllers for a single domain? | **Two** |
| 2.23 | Location of Database folder | `C:\Windows\NTDS` |
| 2.23 | Location of Log Files folder | `C:\Windows\NTDS` |
| 2.23 | Location of SYSVOL folder | `C:\Windows\SYSVOL` |
| 3.5 | Options on right-clicking an OU | Delegate Control, Move, Rename, Delete, Properties, New |
| 4.3 | User accounts / Groups in Users folder | 2 user accounts / 19 groups |
| 4.5 | Auto-completed fields when creating a user | Full Name, User logon name (Pre-Windows 2000) |

---

## Key Concepts Demonstrated

**DNS before AD DS** — DNS was installed first because Active Directory relies on DNS for domain controller location, replication, and client authentication. The DNS Server and AD DS roles were co-located on the same server, which is standard practice for small deployments.

**New Forest creation** — A brand-new Active Directory forest was created rather than joining an existing one. This makes the server the first and only domain controller, the Schema Master, the RID Master, and all other FSMO role holders by default.

**Delegation of Control** — Granting a non-admin user the ability to create and manage accounts within a specific OU without granting domain-wide admin privileges. This follows the principle of least privilege.

**User account lifecycle** — The lab covered creation, configuration, renaming, and relocation of an account — reflecting real-world identity management workflows where accounts are frequently restructured as staff roles change.

---

*NIT2122 Lab Assessment 1 · Lab 3 · Victoria University*  
*Note: This was a late submission due to login issues on the VU Collaborate platform.*
