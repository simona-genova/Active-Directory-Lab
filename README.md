# Enterprise Windows Domain & Active Directory Lab

## Overview
This project demonstrates setting up an enterprise Active Directory (AD) environment in VirtualBox. It covers Domain Controller configuration, user provisioning following IAM security standards, domain joins, Help Desk password reset workflows, and Group Policy Object (GPO) enforcement.

---

## Technical Infrastructure
- **Domain Controller (`DC-Server`):** Windows Server 2022 | IP: `192.168.10.1` | Domain: `company.local`
- **Client Workstation (`Client-PC`):** Windows 10/11 Pro | IP: `192.168.10.2` | Joined to `company.local`
- **Virtual Network:** VirtualBox Internal Network (`AdminLab`)

---

## Step-by-Step Implementation

### Step 1: Network & Virtual Machine Configuration
1. Created two VMs in VirtualBox: `DC-Server` (Windows Server 2022) and `Client-PC` (Windows 10/11).
2. Set both network adapters to **Internal Network** with the name `AdminLab` to isolate traffic.
3. Configured static IP on `DC-Server`: `192.168.10.1`, Subnet: `255.255.255.0`, DNS: `127.0.0.1`.

### Step 2: Active Directory DS Deployment & Server Promotion
1. Installed Windows Server 2022 Standard (Desktop Experience).
2. Added **Active Directory Domain Services (AD DS)** role via Server Manager.
3. Promoted server to Domain Controller for a new forest named `company.local`.

### Step 3: Identity Management (IAM) & Client Domain Join
1. Provisioned a domain user account for **David Miller** (`dmiller`).
2. Applied NIST-aligned IAM standards: set temporary password and checked `User must change password at next logon`.

<img width="1022" height="727" alt="01_AD_User_Provisioning_dmiller" src="https://github.com/user-attachments/assets/cb25f5de-5394-4829-b42b-35f19de29d3a" />

**Figure 1:** Active Directory Users and Computers showing the newly created user account for David Miller (dmiller) under the company.local domain.

3. Installed `Client-PC`, set static IP to `192.168.10.2`, and pointed DNS to `192.168.10.1`.
4. Joined `Client-PC` to `company.local` using domain administrator credentials.

<img width="1022" height="727" alt="02_Client_Domain_Join_Success" src="https://github.com/user-attachments/assets/8660a535-108e-4c21-9633-3b83d5c5560b" />

**Figure 2:** Confirmation popup on the Windows client workstation showing a successful domain join to company.local.

5. Verified initial logon as `dmiller` and forced initial password update.

### Step 4: Enterprise Support Operations & GPO Enforcement

#### Case 1: Active Directory Password Reset Workflow
1. Simulated a Help Desk support ticket requesting a user password reset for `dmiller`.
2. Executed password reset in AD DS and forced password change on next logon.

<img width="1022" height="727" alt="03_Support_Case_Password_Reset" src="https://github.com/user-attachments/assets/a2c4752b-dabd-4042-ab4a-303f757e80b3" />

**Figure 3:** Password reset process for user dmiller in Active Directory with the option to force a password change at next logon enabled.

#### Case 2: Group Policy Object (GPO) Deployment
1. Created and linked a GPO named `Block-ControlPanel` under `company.local`.
2. Enabled setting: `User Configuration -> Policies -> Administrative Templates -> Control Panel -> Prohibit access to Control Panel and PC settings`.
3. Applied policy on `Client-PC` using `gpupdate /force` in Command Prompt.
4. Attempted opening Control Panel under `dmiller` account to verify policy restriction.

<img width="1021" height="725" alt="04_GPO_ControlPanel_Restriction" src="https://github.com/user-attachments/assets/e7b46ff3-d675-4ae5-a1d5-739aea07c3c0" />

**Figure 4:** System restriction error message displayed when user dmiller attempts to open Control Panel, verifying the active Block-ControlPanel Group Policy Object.

---

## Key Skills Demonstrated
- Active Directory Domain Services (AD DS) setup & management
- Identity and Access Management (IAM) best practices
- Network interface configuration (IPv4 & DNS)
- Group Policy Management & security enforcement
- Technical troubleshooting & Help Desk operations
