# Windows Server 2025 & Windows 11 Enterprise Lab

> A fully documented, professional grade Active Directory home lab built in Hyper-V, demonstrating hands on proficiency in System Administration, Active Directory Domain Services, Group Policy, Network Infrastructure, and PowerShell Automation.

---

## Table of Contents

- [Lab Overview](#lab-overview)
- [Environment Summary](#environment-summary)
- [VM Setup — Hyper-V Infrastructure](#vm-setup--hyper-v-infrastructure)
- [Phase 1 — Promote to Domain Controller](#phase-1--promote-to-domain-controller)
- [Phase 2 — OUs, Users & Groups](#phase-2--ous-users--groups)
- [Phase 3 — Group Policy Objects](#phase-3--group-policy-objects)
- [Phase 4 — Domain Join & Initial User Logon](#phase-4--domain-join--initial-user-logon)
- [Phase 5 — DNS, DHCP & Shared Folders](#phase-5--dns-dhcp--shared-folders)
- [Phase 6 — Help Desk Simulation Scenarios](#phase-6--help-desk-simulation-scenarios)
- [Phase 50 (Z) — PowerShell Automation Script](#phase-50-z--powershell-automation-script)
- [Skills Demonstrated](#skills-demonstrated)

---

## Lab Overview

This repository documents the end-to-end deployment of a Windows Server 2025 Active Directory environment, built entirely from scratch inside Hyper-V. Every phase is fully documented with annotated screenshots that explain not just *what* was done, but *why* each step matters in a real enterprise environment.

The lab simulates the kind of infrastructure a help desk technician or junior sysadmin would interact with daily — domain-joined workstations, user account management, Group Policy enforcement, DNS/DHCP configuration, shared folder permissions, and real help desk scenarios like account lockouts and password resets.

---

## Environment Summary

| Machine | OS | Role | Hostname | IP |
|---|---|---|---|---|
| Domain Controller | Windows Server 2025 Standard | AD DS, DNS, DHCP | Technocrat-DC01 | 192.168.1.10 (Static) |
| Workstation | Windows 11 Enterprise Evaluation | Domain-joined client | Workstation.1 | DHCP |

**Domain:** `technocrat.local`

---

## VM Setup — Hyper-V Infrastructure

**Goal:** Configure the virtual hardware and install the base operating systems for both machines.

### 1. Hyper-V Infrastructure

- **Generation 2 VMs** — Selected for modern UEFI support and enhanced security features over the legacy Generation 1 option.
- **Memory** — Assigned 4096 MB RAM with Dynamic Memory enabled to optimize physical host resources.
- **Storage** — Created dedicated 127 GB VHDX files for both the workstation and the server.
- **Security Fix** — Manually enabled TPM (Trusted Platform Module) and Secure Boot in VM settings to satisfy Windows 11's hardware requirements and bypass installation blocks.

<details>
  <summary><b>Click to expand: Phase 0 - Virtual Machine Environment Setup (45 Screenshots)</b></summary>

  ### 1. Hyper-V Console & ISO Downloads
  *Establishing the foundation by acquiring the necessary OS images and preparing the virtualization environment.*

  ![Hyper-V Manager Console](./Phase0.Virtual_Machines_Setup/001.%20Hyper_V_Manager_Console.png)
  *Opening view of Hyper-V Manager on the host machine*

  ![Windows Server Evaluation ISO Search](./Phase0.Virtual_Machines_Setup/002.%20Windows_Server_Evaluation_ISO_Search.png)
  *Searching for the Windows Server 2025 Evaluation ISO*

  ![Microsoft Evaluation Center Win11 Select](./Phase0.Virtual_Machines_Setup/003.%20Microsoft_Evaluation_Center_Win11_Select.png)
  *Selecting Windows 11 Enterprise Evaluation from Microsoft's Evaluation Center*

  ![Win11 Enterprise ISO Download Selection](./Phase0.Virtual_Machines_Setup/004.%20Win11_Enterprise_ISO_Download_Selection.png)
  *Choosing the ISO download format for Windows 11 Enterprise*

  ![Win11 ISO Language Selection](./Phase0.Virtual_Machines_Setup/005.%20Win11_ISO_Language_Selection.png)
  *Selecting language for the Windows 11 ISO*

  ![Windows Server 2025 Evaluation Select](./Phase0.Virtual_Machines_Setup/006.%20Windows_Server_2025_Evaluation_Select.png)
  *Selecting Windows Server 2025 Evaluation edition*

  ![Windows Server 2025 ISO Download Selection](./Phase0.Virtual_Machines_Setup/007.%20Windows_Server_2025_ISO_Download_Selection.png)
  *Choosing the ISO download format for Server 2025*

  ![Windows Server 2025 ISO Language Selection](./Phase0.Virtual_Machines_Setup/008.%20Windows_Server_2025_ISO_Language_Selection.png)
  *Selecting language for the Server 2025 ISO*

  ![ISO File Organization](./Phase0.Virtual_Machines_Setup/009.%20ISO_File_Organization.png)
  *Both ISOs organized in a dedicated folder on the host machine*

  ---

  ### 2. New VM Wizard — Shared Settings
  *Configuring the virtual hardware specifications for both the Domain Controller and the Workstation.*

  ![Hyper-V New Virtual Machine Wizard Start](./Phase0.Virtual_Machines_Setup/010.%20Hyper-V_New_Virtual_Machine_Wizard_Start.png)
  *Launching the New Virtual Machine Wizard in Hyper-V Manager*

  ![New VM Wizard Before You Begin](./Phase0.Virtual_Machines_Setup/011.%20New_VM_Wizard_Before_You_Begin.png)
  *The Before You Begin screen of the New VM Wizard*

  ![New VM Specify Name Win11](./Phase0.Virtual_Machines_Setup/012.%20New_VM_Specify_Name_Win11.png)
  *Naming the Windows 11 VM*

  ![New VM Specify Generation Gen 2](./Phase0.Virtual_Machines_Setup/013.%20New_VM_Specify_Generation_Gen2.png)
  *Selecting Generation 2 for UEFI support and modern security features*

  ![New VM Assign Memory 4GB Dynamic](./Phase0.Virtual_Machines_Setup/014.%20New_VM_Assign_Memory_4GB_Dynamic.png)
  *Assigning 4096 MB RAM with Dynamic Memory enabled*

  ![New VM Connect Virtual Hard Disk 127GB](./Phase0.Virtual_Machines_Setup/015.%20New_VM_Connect_Virtual_Hard_Disk_127GB.png)
  *Creating a 127 GB VHDX virtual hard disk*

  ![New VM Mount Win11 Enterprise ISO](./Phase0.Virtual_Machines_Setup/016.%20New_VM_Mount_Win11_Enterprise_ISO.png)
  *Mounting the Windows 11 Enterprise ISO to the virtual DVD drive*

  ![New VM Summary Win11 Enterprise](./Phase0.Virtual_Machines_Setup/017.%20New_VM_Summary_Win11_Enterprise.png)
  *Summary screen confirming all VM settings before creation*

  ![Hyper-V Manage VMs Created Off State](./Phase0.Virtual_Machines_Setup/018.%20Hyper_V_Manager_VMs_Created_Off_State.png)
  *Both VMs visible in Hyper-V Manager in the Off state, ready to boot*

  ![Hyper-V VM Security Enable TPM](./Phase0.Virtual_Machines_Setup/026.%20Hyper-V_VM_Security_Enable_TPM.png)
  *Manually enabling TPM and Secure Boot in VM settings to satisfy Windows 11 hardware requirements*

  ---

  ### 3. Windows 11 Workstation Deployment
  *Deployment of the client machine to be used for future help desk and administrative scenarios.*

  | Setting | Value |
  |---|---|
  | Version | Windows 11 Enterprise Evaluation |
  | Local Admin Account | `Workstation.1` |
  | Privacy | Customized to minimize background telemetry |

  ![Win11 Workstation Right-Click Start](./Phase0.Virtual_Machines_Setup/019.%20Win11_Workstation_Right-Click_Start.png)
  *Right-clicking the VM in Hyper-V to start it for the first time*

  ![Win11 Workstation Running State](./Phase0.Virtual_Machines_Setup/020.%20Win11_Workstation_Running_State.png)
  *VM successfully booted into the Windows 11 installer*

  ![Win11 Boot Prompt Press Any Key](./Phase0.Virtual_Machines_Setup/021.%20Win11_Boot_Prompt_Press_Any_Key.png)
  *Press any key to boot from the ISO prompt*

  ![Win11 Setup Language and Regional Settings](./Phase0.Virtual_Machines_Setup/022.%20Win11_Setup_Language_and_Regional_Settings.png)
  *Selecting language and regional settings in the Windows 11 Setup wizard*

  ![Win11 Install Selection and Data Consent](./Phase0.Virtual_Machines_Setup/023.%20Win11_Install_Selection_and_Data_Consent.png)
  *Accepting the installation terms*

  ![Win11 Installation Error TPM Requirement](./Phase0.Virtual_Machines_Setup/024.%20Win11_Installation_Error_TPM_Requirement.png)
  *TPM requirement error encountered — resolved by enabling TPM in settings*

  ![Win11 VM Settings Menu](./Phase0.Virtual_Machines_Setup/025.%20Win11_VM_Settings_Menu.png)
  *Accessing VM settings to apply the TPM/Secure Boot fix*

  ![Win11 EULA Agreement](./Phase0.Virtual_Machines_Setup/027.%20Windows_11_EULA_Agreemen.png)
  *Accepting the Windows 11 End User License Agreement*

  ![Win11 Install Location Unallocated Space](./Phase0.Virtual_Machines_Setup/028.%20Win11_Install_Location_Unallocated_Space.png)
  *Selecting the unallocated 127 GB disk for installation*

  ![Win11 Setup Installing OS](./Phase0.Virtual_Machines_Setup/029.%20Windows_11_Setup_Installing_OS.png)
  *Windows 11 installation in progress*

  ![Win11 Installing Files Progress](./Phase0.Virtual_Machines_Setup/030.%20Windows_11_Installing_Files_Progress.png)
  *File installation progress screen*

  #### OOBE — Out of Box Experience
  ![Win11 OOBE Region Selection](./Phase0.Virtual_Machines_Setup/031.%20Win11_OOBE_Region_Selection.png)
  *Selecting region during the Windows 11 Out of Box Experience*

  ![Win11 OOBE Create Local User Workstation.1](./Phase0.Virtual_Machines_Setup/032.%20Win11_OOBE_Create_Local_User_Workstation.1.png)
  *Creating the local administrator account named Workstation.1*

  ![Win11 OOBE Set Local User Password](./Phase0.Virtual_Machines_Setup/033.%20Win11_OOBE_Set_Local_User_Password.png)
  *Setting the password for the Workstation.1 local account*

  ![Win11 OOBE Set Security Questions](./Phase0.Virtual_Machines_Setup/034.%20Win11_OOBE_Set_Security_Questions.png)
  *Configuring security questions for the local account*

  ![Win11 OOBE Privacy Settings Selection](./Phase0.Virtual_Machines_Setup/035.%20Win11_OOBE_Privacy_Settings_Selection.png)
  *Customizing privacy settings to minimize background telemetry*

  ![Win11 OOBE Final Configuration Screen](./Phase0.Virtual_Machines_Setup/036.%20Win11_OOBE_Final_Configuration_Screen.png)
  *Final configuration screen before reaching the desktop*

  ![Win11 Workstation Initial Login Screen](./Phase0.Virtual_Machines_Setup/037.%20Win11_Workstation_Initial_Login_Screen.png)
  *First login screen for the Workstation.1 account*

  ![Win11 Successful OS Installation Complete](./Phase0.Virtual_Machines_Setup/038.%20Win11_Successful_OS_Installation_Complete.png)
  *Windows 11 Enterprise desktop confirming successful installation*

  ---

  ### 4. Windows Server 2025 Deployment
  *Setting up the core server that will eventually serve as the Active Directory Domain Controller.*

  | Setting | Value |
  |---|---|
  | Version | Windows Server 2025 Standard (Desktop Experience) |
  | Initial Account | Built-in Administrator |

  ![Windows Server 2025 Setup Language Selection](./Phase0.Virtual_Machines_Setup/039.%20Windows_Server_2025_Setup_Language_Selection.png)
  *Language selection screen in the Windows Server 2025 installer*

  ![Windows Server 2025 OS Edition Selection](./Phase0.Virtual_Machines_Setup/040.%20Windows_Server_2025_OS_Edition_Selection.png)
  *Selecting Windows Server 2025 Datacenter (Desktop Experience)*

  ![Windows Server 2025 OS Installation Start](./Phase0.Virtual_Machines_Setup/041.%20Server_2025_OS_Installation_Start.png)
  *Server 2025 installation begins*

  ![Server 2025 Set Administrator Password](./Phase0.Virtual_Machines_Setup/042.%20Server_2025_Set_Administrator_Password.png)
  *Setting the built-in Administrator password*

  ![Server 2025 Admin Password Confirmation](./Phase0.Virtual_Machines_Setup/043.%20Server_2025_Admin_Password_Confirmation.png)
  *Confirming the Administrator password*

  ![Server 2025 Initial Administrator Login](./Phase0.Virtual_Machines_Setup/044.%20Server_2025_Initial_Administrator_Login.png)
  *First login as the built-in Administrator account*

  ![Server 2025 Initial Server Manager Launch](./Phase0.Virtual_Machines_Setup/045.%20Server_2025_Initial_Server_Manager_Launch.png)
  *Server Manager Dashboard confirming readiness for role deployment*

</details>

---

## Phase 1 — Promote to Domain Controller

**Goal:** Transform the base Windows Server 2025 installation into a fully functioning Active Directory Domain Controller.

- [x] Configure Static IP Address (`192.168.1.10`, DNS pointed to `127.0.0.1`)
- [x] Rename server to `Technocrat-DC01`
- [x] Install Active Directory Domain Services (AD DS) role via Server Manager
- [x] Promote to Domain Controller — New Forest: `technocrat.local`

#### Key Decisions

- **Static IP** — A domain controller must have a fixed IP so that domain-joined clients can always locate it for authentication and DNS resolution.
- **DNS pointed to loopback** — After promotion, the DC runs its own DNS server. Pointing it to `127.0.0.1` ensures it resolves internal records correctly.
- **New Forest** — `technocrat.local` was created as a fresh AD forest, establishing the root domain for the entire lab environment.

That is a great catch. Keeping the count in the header makes the documentation feel much more consistent and lets anyone reading it know exactly how deep the "deep dive" goes.

Here is the updated Markdown for Phase 1 with the (31 Screenshots) tag included in the summary header. I also ensured the folder path Phase1.Windows_Server_Config_PromoteDC and the exact filenames from your upload (like 20A. Server_Manager_Promote_to_Domain_Controller_Link.png) are perfectly preserved.

Markdown
<details>
  <summary><b>Click to expand: Phase 1 - AD DS Installation & DC Promotion (31 Screenshots)</b></summary>

  ### Server Manager & initial configuration
  *Preparing the baseline server environment by configuring the hostname and static IP stack required for a Domain Controller.*

  ![Server Manager Local Server Properties Default](./Phase1.Windows_Server_Config_PromoteDC/01A.%20Server_Manager_Local_Server_Properties_Default.png)
  *Server Manager opening view before any configuration — baseline starting point*

  ![Rename Server to DC01](./Phase1.Windows_Server_Config_PromoteDC/02A.%20Rename_Server_to_DC01.png)
  *Renaming the server to Technocrat-DC01 via Server Manager Local Server properties*

  ![Network Settings Initial IP Assignment](./Phase1.Windows_Server_Config_PromoteDC/03A.%20Network_Settings_Initial_IP_Assignment.png)
  *Opening network adapter settings to begin static IP configuration*

  ![Network Settings Switch to Manual IP](./Phase1.Windows_Server_Config_PromoteDC/04A.%20Network_Settings_Switch_to_Manual_IP.png)
  *Switching from DHCP to manual IP assignment*

  ![IPv4 Static IP and DNS Configuration](./Phase1.Windows_Server_Config_PromoteDC/05A.%20IPv4_Static_IP_and_DNS_Configuration.png)
  *Setting static IP 192.168.1.10 with DNS pointed to 127.0.0.1 — the DC will serve as its own DNS resolver after promotion*

  ![Local Server Properties Static IPv4 Confirmed](./Phase1.Windows_Server_Config_PromoteDC/06A.%20Local_Server_Properties_Static_IPv4_Confirmed.png)
  *Server Manager confirming the static IP is now applied*

  ![Disable IE Enhanced Security Configuration](./Phase1.Windows_Server_Config_PromoteDC/07A.%20Disable_IE_Enhanced_Security_Configuration.png)
  *Disabling IE Enhanced Security Configuration to ease lab administration*

  ![Server Restart Apply Name and IP Changes](./Phase1.Windows_Server_Config_PromoteDC/08A.%20Server_Restart_Apply_Name_and_IP_Changes.png)
  *Restarting the server to apply the hostname rename and static IP changes*

  ![Local Server Properties Final Pre-Promotion Check](./Phase1.Windows_Server_Config_PromoteDC/09A.%20Local_Server_Properties_Final_Pre-Promotion_Check.png)
  *Verifying hostname and IP are correct before beginning AD DS role installation*

  ---

  ### Installing the AD DS role
  *Adding the Active Directory Domain Services binaries to the server through the Add Roles and Features wizard.*

  ![Launching Add Roles and Features Wizard](./Phase1.Windows_Server_Config_PromoteDC/10A.%20Launching_Add_Roles_and_Features_Wizard.png)
  *Launching the Add Roles and Features Wizard from Server Manager*

  ![ADDS Wizard Before You Begin](./Phase1.Windows_Server_Config_PromoteDC/11A.%20ADDS_Wizard_Before_You_Begin.png)
  *Before You Begin screen of the Add Roles and Features Wizard*

  ![ADDS Wizard Server Selection DC01](./Phase1.Windows_Server_Config_PromoteDC/12A.%20ADDS_Wizard_Server_Selection_DC01.png)
  *Selecting Technocrat-DC01 as the target server*

  ![ADDS Wizard Select Server Role ADDS](./Phase1.Windows_Server_Config_PromoteDC/13A.%20ADDS_Wizard_Select_Server_Role_ADDS.png)
  *Checking Active Directory Domain Services from the server roles list*

  ![ADDS Wizard Add Required Management Features](./Phase1.Windows_Server_Config_PromoteDC/14A.%20ADDS_Wizard_Add_Required_Management_Features.png)
  *Confirming additional required management features to be installed alongside AD DS*

  ![ADDS Wizard Select Features](./Phase1.Windows_Server_Config_PromoteDC/15A.%20ADDS_Wizard_Select_Features.png)
  *Features screen — leaving defaults and proceeding*

  ![ADDS Installation Overview Notes](./Phase1.Windows_Server_Config_PromoteDC/16A.%20ADDS_Installation_Overview_Notes.png)
  *AD DS overview screen explaining what the role does*

  ![ADDS Final Review and Install Confirmation](./Phase1.Windows_Server_Config_PromoteDC/17A.%20ADDS_Final_Review_and_Install_Confirmation.png)
  *Confirmation screen before beginning the AD DS role installation*

  ![ADDS Feature Installation Running](./Phase1.Windows_Server_Config_PromoteDC/18A.%20ADDS_Feature_Installation_Running.png)
  *AD DS role installation in progress*

  ![Server Manager ADDS Role Visible](./Phase1.Windows_Server_Config_PromoteDC/19A.%20Server_Manager_ADDS_Role_Visible.png)
  *Server Manager showing AD DS role installed — yellow flag prompting DC promotion*

  ---

  ### Promoting to Domain Controller
  *Promoting the server to a Domain Controller and creating the 'technocrat.local' forest.*

  ![Server Manager Promote to Domain Controller](./Phase1.Windows_Server_Config_PromoteDC/20A.%20Server_Manager_Promote_to_Domain_Controller_Link.png)
  *Clicking the yellow flag notification to launch the DC Promotion wizard*

  ![ADDS Deployment Add New Forest](./Phase1.Windows_Server_Config_PromoteDC/21A.%20ADDS_Deployment_Add_New_Forest.png)
  *Selecting "Add a new forest" and setting the root domain name to technocrat.local*

  ![ADDS New Forest Technocrat Local](./Phase1.Windows_Server_Config_PromoteDC/22A.%20ADDS_New_Forest_Technocrat_Local.png)
  *Confirming the new forest name technocrat.local*

  ![ADDS DC Options Functional Levels and DSRM](./Phase1.Windows_Server_Config_PromoteDC/23A.%20ADDS_DC_Options_Functional_Levels_and_DSRM_Password.png)
  *Setting forest/domain functional levels and configuring the DSRM recovery password*

  ![ADDS DNS Options Ignore Delegation Warning](./Phase1.Windows_Server_Config_PromoteDC/24A.%20ADDS_DNS_Options_Ignore_Delegation_Warning.png)
  *DNS delegation warning — expected for a new private forest, safely ignored*

  ![ADDS Verify NetBIOS Domain Name TECHNOCRAT](./Phase1.Windows_Server_Config_PromoteDC/25A.%20ADDS_Verify_NetBIOS_Domain_Name_TECHNOCRAT.png)
  *NetBIOS name auto-populated as TECHNOCRAT — confirmed and accepted*

  ![ADDS Paths Database Logs and SYSVOL Default](./Phase1.Windows_Server_Config_PromoteDC/26A.%20ADDS_Paths_Database_Logs_and_SYSVOL_Defaults.png)
  *Leaving default paths for the NTDS database, log files, and SYSVOL*

  ![ADDS Final Validation Before DC Promotion](./Phase1.Windows_Server_Config_PromoteDC/27A.%20ADDS_Final_Validation_Before_DC_Promotion.png)
  *Prerequisites check passing — green light to proceed with promotion*

  ![ADDS DC Promotion Installation Progress](./Phase1.Windows_Server_Config_PromoteDC/28A.%20ADDS_DC_Promotion_Installation_Progress.png)
  *DC promotion running — server will automatically reboot on completion*

  ---

  ### Post-promotion verification
  *Confirming successful domain creation and administrative tool accessibility.*

  ![DC01 Initial Domain Administrator Login](./Phase1.Windows_Server_Config_PromoteDC/29A.%20DC01_Initial_Domain_Administrator_Login.png)
  *First login after reboot as TECHNOCRAT\Administrator — confirming domain prefix on login screen*

  ![Server Manager Verify DC01 on Technocrat Local](./Phase1.Windows_Server_Config_PromoteDC/30A.%20Server_Manager_Verify_DC01_on_Technocrat_Local.png)
  *Server Manager confirming DC01 is now a domain controller for technocrat.local*

  ![Verify Active Directory Administrative Tools Installed](./Phase1.Windows_Server_Config_PromoteDC/31A.%20Verify_Active_Directory_Administrative_Tools_Installed.png)
  *Confirming Active Directory administrative tools (ADUC, DNS Manager, GPMC) are installed and accessible*

</details>

---

## Phase 2 — OUs, Users & Groups

**Goal:** Build an organizational structure inside Active Directory that mirrors a real company, then populate it with user accounts and security groups.

- [x] Design and create Organizational Unit (OU) structure under a top-level `Technocrat` OU
- [x] Create department OUs: `IT`, `HR`, `Sales`, `Disabled Accounts`
- [x] Bulk-import 50 users via PowerShell script and CSV file into `Lab-Users` OU
- [x] Create Security Groups and assign users to appropriate groups

#### OU Structure

```
technocrat.local
└── Technocrat
    ├── IT
    ├── HR
    ├── Sales
    ├── Disabled Accounts
    └── Lab-Users
```

### Opening ADUC & creating the OU structure

![ADUC Initial Launch Technocrat Local](./phase-2-users-ous-groups/screenshots/1B_ADUC_Initial_Launch_Technocrat_Local.png)
*Active Directory Users and Computers opening view showing the technocrat.local domain*

![ADUC Domain New Organizational Unit Menu](./phase-2-users-ous-groups/screenshots/2B_ADUC_Domain_New_Organizational_Unit_Menu.png)
*Right-clicking the domain to create a new top-level Organizational Unit*

![ADUC New Object Organizational Unit Dialog](./phase-2-users-ous-groups/screenshots/3B_ADUC_New_Object_Organizational_Unit_Dialog.png)
*New Object dialog for creating an Organizational Unit*

![ADUC Create Top Level Technocrat OU](./phase-2-users-ous-groups/screenshots/4B_ADUC_Create_Top_Level_Technocrat_OU.png)
*Creating the top-level Technocrat OU that will contain all department OUs*

![Nesting Department OUs Inside Technocrat OU](./phase-2-users-ous-groups/screenshots/5B_Nesting_Department_OUs_Inside_Technocrat_OU.png)
*Creating department OUs nested inside the Technocrat OU*

![ADUC Technocrat Nested OU Structure Complete](./phase-2-users-ous-groups/screenshots/6B_ADUC_Technocrat_Nested_OU_Structure_Complete.png)
*Completed OU structure showing IT, HR, Sales, Disabled Accounts, and Lab-Users nested under Technocrat*

### Bulk user import

![ADUC Organizing Bulk Users to Department OUs](./phase-2-users-ous-groups/screenshots/7B_ADUC_Organizing_Bulk_Users_to_Department_OUs.png)
*Moving bulk-imported users from Lab-Users into their respective department OUs*

![ADUC Bulk Move Users to IT OU](./phase-2-users-ous-groups/screenshots/8B_ADUC_Bulk_Move_Users_to_IT_OU.png)
*Bulk-selecting and moving users into the IT OU*

![ADUC Move Objects Warning Confirmation](./phase-2-users-ous-groups/screenshots/9B_ADUC_Move_Objects_Warning_Confirmation.png)
*Confirming the move operation warning dialog*

![ADUC Final Technocrat OU Structure Expanded](./phase-2-users-ous-groups/screenshots/10B_ADUC_Final_Technocrat_OU_Structure_Expanded.png)
*Final expanded OU tree showing users distributed across all department OUs*

![ADUC Disabled Accounts OU Populated](./phase-2-users-ous-groups/screenshots/11B_ADUC_Disabled_Accounts_OU_Populated.png)
*Disabled Accounts OU populated — ready to receive deactivated user accounts*

![ADUC HR OU Users List](./phase-2-users-ous-groups/screenshots/12B_ADUC_HR_OU_Users_List.png)
*HR OU showing its assigned user accounts*

![ADUC IT OU Users List](./phase-2-users-ous-groups/screenshots/13B_ADUC_IT_OU_Users_List.png)
*IT OU showing its assigned user accounts*

![ADUC Sales OU Users List](./phase-2-users-ous-groups/screenshots/14B_ADUC_Sales_OU_Users_List.png)
*Sales OU showing its assigned user accounts*

![ADUC Accounting OU Users List](./phase-2-users-ous-groups/screenshots/15B_ADUC_Accounting_OU_Users_List.png)
*Accounting OU showing its assigned user accounts*

![ADUC Marketing OU Users List](./phase-2-users-ous-groups/screenshots/16B_ADUC_Marketing_OU_Users_List.png)
*Marketing OU showing its assigned user accounts*

### Creating a new user manually

![ADUC IT OU New Object Name Menu](./phase-2-users-ous-groups/screenshots/17B_ADUC_IT_OU_New_Object_Name_Menu.png)
*Right-clicking the IT OU to manually create a new user object*

![ADUC New User Initial Password Logon Details](./phase-2-users-ous-groups/screenshots/18B_ADUC_New_User_Initial_Password_Logon_Details.png)
*Setting the initial password and logon options for the new user*

![ADUC New User Object Name and Security Setup](./phase-2-users-ous-groups/screenshots/19B_ADUC_New_User_Object_Name_and_Security_Setup.png)
*Entering the new user's name and security settings*

![ADUC New User Summary Confirmation Abel](./phase-2-users-ous-groups/screenshots/20B_ADUC_New_User_Summary_Confirmation_Abel.png)
*Summary confirmation screen before finalizing the new user account*

![ADUC User Properties Abel Scheheelein](./phase-2-users-ous-groups/screenshots/21B_ADUC_User_Properties_Abel_Scheheelein.png)
*User properties for Abel Scheheelein showing account details*

![ADUC User Properties Default Group Members](./phase-2-users-ous-groups/screenshots/22B_ADUC_User_Properties_Default_Group_Members.png)
*Member Of tab showing Abel's default group membership (Domain Users)*

### Creating security groups

![Creating First Security Group Members](./phase-2-users-ous-groups/screenshots/23B_Creating_First_Security_Group_Members.png)
*Creating the first security group and adding initial members*

![ADUC Create Security Group IT Admins](./phase-2-users-ous-groups/screenshots/24B_ADUC_Create_Security_Group_IT_Admins.png)
*Creating the IT-Admins security group*

![ADUC IT OU Security Group Created Verification](./phase-2-users-ous-groups/screenshots/25B_ADUC_IT_OU_Security_Group_Created_Verification.png)
*Verifying the IT-Admins group appears in the IT OU*

![ADUC Create Groups OU](./phase-2-users-ous-groups/screenshots/26B_ADUC_Create_Groups_OU.png)
*Creating a dedicated Groups OU to organize all security groups*

![Centralizing Domain Security Groups Creation](./phase-2-users-ous-groups/screenshots/27B_Centralizing_Domain_Security_Groups_Creation.png)
*Centralizing all security groups into the Groups OU for clean AD organization*

![ADUC Create Security Group VPN User](./phase-2-users-ous-groups/screenshots/28B_ADUC_Create_Security_Group_VPN_User.png)
*Creating the VPN-Users security group*

![ADUC Groups OU VPN Users](./phase-2-users-ous-groups/screenshots/29B_ADUC_Groups_OU_VPN_Users.png)
*Groups OU showing VPN-Users group created*

### Assigning users to groups

![ADUC User Properties Member Tab Abel](./phase-2-users-ous-groups/screenshots/30B_ADUC_User_Properties_Member_Tab_Abel.png)
*Opening Abel's Member Of tab to assign group memberships*

![ADUC Add User to IT Admins](./phase-2-users-ous-groups/screenshots/31B_ADUC_Add_User_to_IT-Admins.png)
*Adding Abel to the IT-Admins security group*

![ADUC Abel Assigned to IT Admins Security Group](./phase-2-users-ous-groups/screenshots/32B_ADUC_Abel_Assigned_to_IT-Admins_Security_Group.png)
*Member Of tab confirming Abel is now a member of IT-Admins*

![ADUC Assigning Abel and Jason to IT Admins](./phase-2-users-ous-groups/screenshots/33B_ADUC_Assigning_Abel_and_Jason_to_IT-Admins.png)
*Adding both Abel and Jason to the IT-Admins group*

![ADUC Groups OU Complete Security Group List](./phase-2-users-ous-groups/screenshots/34B_ADUC_Groups_OU_Complete_Security_Group_List.png)
*Groups OU showing all security groups created for the domain*

![ADUC Create Domain Local Group DL VPN Access](./phase-2-users-ous-groups/screenshots/35B_ADUC_Create_Domain_Local_Group_DL-VPN-Access.png)
*Creating a Domain Local group DL-VPN-Access for role-based access control*

![ADUC Nesting VPN Users into DL VPN Access](./phase-2-users-ous-groups/screenshots/36B_ADUC_Nesting_VPN-Users_into_DL-VPN-Access.png)
*Nesting the VPN-Users global group inside the DL-VPN-Access domain local group*

![ADUC Verify Group Nesting VPN Users in DL VPN](./phase-2-users-ous-groups/screenshots/37B_ADUC_Verify_Group_Nesting_VPN-Users_in_DL-VPN.png)
*Verifying the group nesting is correctly configured*

![ADUC Assigning IT Department Users to VPN Users](./phase-2-users-ous-groups/screenshots/38B_ADUC_Assigning_IT_Department_Users_to_VPN-Users.png)
*Assigning IT department users to the VPN-Users group*

![ADUC Deactivating User Account Steven Allen](./phase-2-users-ous-groups/screenshots/39B_ADUC_Deactivating_User_Account_Steven_Allen.png)
*Deactivating Steven Allen's account and moving it to the Disabled Accounts OU*

### Group properties & delegation

![ADUC Group Properties Assign Managed By](./phase-2-users-ous-groups/screenshots/40B_ADUC_Group_Properties_Assign_Managed_By.png)
*Setting the Managed By attribute on a security group*

![ADUC Delegate Group Membership Update](./phase-2-users-ous-groups/screenshots/41B_ADUC_Delegate_Group_Membership_Update.png)
*Delegating group membership management to a specific user*

![ADUC Rename Group Prefix GS IT Admins](./phase-2-users-ous-groups/screenshots/42B_ADUC_Rename_Group_Prefix_GS_IT-Admins.png)
*Renaming group with GS prefix to follow naming conventions*

![ADUC Rename Group Prefix GS VPN Users](./phase-2-users-ous-groups/screenshots/43B_ADUC_Rename_Group_Prefix_GS_VPN-Users.png)
*Renaming VPN-Users group with GS prefix*

![ADUC Rename Group Prefix DL VPN Access](./phase-2-users-ous-groups/screenshots/44B_ADUC_Rename_Group_Prefix_DL-VPN-Access.png)
*Renaming domain local group with DL prefix following naming conventions*

![ADUC Sales OU Delegate Control Wizard Launch](./phase-2-users-ous-groups/screenshots/45B_ADUC_Sales_OU_Delegate_Control_Wizard_Launch.png)
*Launching the Delegation of Control Wizard on the Sales OU*

![Delegation Wizard Select GS IT Admins Group](./phase-2-users-ous-groups/screenshots/46B_Delegation_Wizard_Select_GS_IT-Admins_Group.png)
*Selecting the GS-IT-Admins group to delegate control to*

![Delegation Wizard Selecting Common Administrative Tasks](./phase-2-users-ous-groups/screenshots/47B_Delegation_Wizard_Selecting_Common_Administrative_Tasks.png)
*Selecting which administrative tasks to delegate to IT-Admins*

![Delegation Wizard Summary and Completion](./phase-2-users-ous-groups/screenshots/48B_Delegation_Wizard_Summary_and_Completion.png)
*Delegation of Control Wizard summary confirming delegation is complete*

---

## Phase 3 — Group Policy Objects

**Goal:** Implement security hardening and desktop controls across the domain using Group Policy.

- [x] Implement security hardening via Group Policy (password complexity, account lockout)
- [x] Configure desktop and browser restrictions for specific OUs

#### GPOs Created

| GPO Name | Linked To | Purpose |
|---|---|---|
| Password Policy | `technocrat.local` (domain) | Minimum length, complexity, max age |
| Account Lockout Policy | `technocrat.local` (domain) | Lock after failed attempts |
| Desktop Restrictions | HR OU | Screensaver lockout, browser controls |
| Drive Mapping | Sales OU | Map shared network drive on login |

### Opening GPMC & reviewing existing policy

![GPMC Console Navigation from Tools Menu](./phase-3-group-policy/screenshots/1C_GPMC_Console_Navigation_from_Tools_Menu.png)
*Opening the Group Policy Management Console from Server Manager Tools menu*

![GPMC Technocrat Local Initial View](./phase-3-group-policy/screenshots/2C_GPMC_Technocrat_Local_Initial_View.png)
*GPMC initial view showing the technocrat.local domain and existing Default Domain Policy*

![GPMC Edit Default Domain Policy Menu](./phase-3-group-policy/screenshots/3C_GPMC_Edit_Default_Domain_Policy_Menu.png)
*Right-clicking the Default Domain Policy to open it for editing*

![GPM Editor Default Domain Policy Overview](./phase-3-group-policy/screenshots/4C_GPM_Editor_Default_Domain_Policy_Overview.png)
*Group Policy Management Editor showing the full Default Domain Policy structure*

### Configuring the Password Policy

![GPM Editor Selecting Computer Configuration](./phase-3-group-policy/screenshots/5C_GPM_Editor_Selecting_Computer_Configuration.png)
*Navigating to Computer Configuration to locate security settings*

![GPM Editor Expanding Computer Policies Node](./phase-3-group-policy/screenshots/6C_GPM_Editor_Expanding_Computer_Policies_Node.png)
*Expanding the Policies node under Computer Configuration*

![GPM Editor Selecting Windows Settings Node](./phase-3-group-policy/screenshots/7C_GPM_Editor_Selecting_Windows_Settings_Node.png)
*Navigating into Windows Settings*

![GPM Editor Selecting Security Settings Node](./phase-3-group-policy/screenshots/8C_GPM_Editor_Selecting_Security_Settings_Node.png)
*Selecting Security Settings to access Account Policies*

![GPM Editor Selecting Account Policies Node](./phase-3-group-policy/screenshots/9C_GPM_Editor_Selecting_Account_Policies_Node.png)
*Opening Account Policies to reach Password Policy settings*

![GPM Editor Selecting Password Policy Node](./phase-3-group-policy/screenshots/10C_GPM_Editor_Selecting_Password_Policy_Node.png)
*Selecting the Password Policy node to view and edit all password settings*

![GPM Editor Selecting Minimum Password Length](./phase-3-group-policy/screenshots/11C_GPM_Editor_Selecting_Minimum_Password_Length.png)
*Opening the Minimum Password Length policy setting*

![GPM Editor Minimum Password Length Property](./phase-3-group-policy/screenshots/12C_GPM_Editor_Minimum_Password_Length_Property.png)
*Setting the minimum password length value*

![Password Policy Set 10 Character Minimum](./phase-3-group-policy/screenshots/13C_Password_Policy_Set_10_Character_Minimum.png)
*Minimum password length set to 10 characters*

![Password Policy Enable Complexity Requirements](./phase-3-group-policy/screenshots/14C_Password_Policy_Enable_Complexity_Requirements.png)
*Enabling password complexity requirements — uppercase, lowercase, numbers, and symbols*

![Password Policy Complexity Requirement Properties](./phase-3-group-policy/screenshots/15C_Password_Policy_Complexity_Requirement_Properties.png)
*Complexity requirement properties confirming the setting is enabled*

![GPM Editor Selecting Maximum Password Age](./phase-3-group-policy/screenshots/16C_GPM_Editor_Selecting_Maximum_Password_Age.png)
*Opening the Maximum Password Age setting*

![Password Policy Set 90 Day Maximum Password Age](./phase-3-group-policy/screenshots/17C_Password_Policy_Set_90_Day_Maximum_Password_Age.png)
*Setting maximum password age to 90 days — users must change password every 90 days*

### Applying & verifying the policy

![Applying Group Policy Changes Immediately](./phase-3-group-policy/screenshots/18C_Applying_Group_Policy_Changes_Immediately.png)
*Running gpupdate /force to immediately apply the updated password policy*

### Creating & linking a GPO to the HR OU

![GPMC HR OU Create and Link GPO Menu](./phase-3-group-policy/screenshots/19C_GPMC_HR_OU_Create_and_Link_GPO_Menu.png)
*Right-clicking the HR OU in GPMC to create and link a new GPO*

![GPMC Creating Targeted GPO for HR OU](./phase-3-group-policy/screenshots/20C_GPMC_Creating_Targeted_GPO_for_HR_OU.png)
*Naming and creating the new GPO targeted specifically at the HR OU*

![GPMC Opening HR Specific Policy Editor](./phase-3-group-policy/screenshots/21C_GPMC_Opening_HR_Specific_Policy_Editor.png)
*Opening the HR GPO in the Group Policy Management Editor*

![GPM Editor Selecting Computer Configuration HR](./phase-3-group-policy/screenshots/22C_GPM_Editor_Selecting_Computer_Configuration_HR.png)
*Navigating to Computer Configuration within the HR GPO*

![GPM Editor Expanding HR Computer Policies](./phase-3-group-policy/screenshots/23C_GPM_Editor_Expanding_HR_Computer_Policies.png)
*Expanding the Computer Policies node in the HR GPO*

![GPM Editor Selecting Administrative Templates](./phase-3-group-policy/screenshots/24C_GPM_Editor_Selecting_Administrative_Templates.png)
*Selecting Administrative Templates to access desktop and system restriction settings*

![GPM Editor Selecting Control Panel Policy Node](./phase-3-group-policy/screenshots/25C_GPM_Editor_Selecting_Control_Panel_Policy_Node.png)
*Navigating to the Control Panel policy node*

![GPM Editor Selecting Personalization Policy Node](./phase-3-group-policy/screenshots/26C_GPM_Editor_Selecting_Personalization_Policy_Node.png)
*Selecting the Personalization policy node for screensaver and desktop settings*

![GPM Editor Reviewing Machine Personalization](./phase-3-group-policy/screenshots/27C_GPM_Editor_Reviewing_Machine_Personalization.png)
*Reviewing available personalization policy settings*

![GPM Editor User Configuration Personalization](./phase-3-group-policy/screenshots/28C_GPM_Editor_User_Configuration_Personalization.png)
*Switching to User Configuration for personalization policy settings*

![GPM Editor Enabling Desktop Screen Saver Policy](./phase-3-group-policy/screenshots/29C_GPM_Editor_Enabling_Desktop_Screen_Saver_Policy.png)
*Enabling the screen saver policy for HR OU workstations*

![GPM Editor Selecting Password Protect Screen](./phase-3-group-policy/screenshots/30C_GPM_Editor_Selecting_Password_Protect_Screen.png)
*Enabling password protection on the screen saver — workstation locks after timeout*

![GPO HR Desktop Lockout Enable Password Protection](./phase-3-group-policy/screenshots/31C_GPO_HR_Desktop_Lockout_Enable_Password_Protection.png)
*Confirming password-protected screen saver lockout is enabled for HR users*

![GPM Editor Selecting Screen Saver Timeout Policy](./phase-3-group-policy/screenshots/32C_GPM_Editor_Selecting_Screen_Saver_Timeout_Policy.png)
*Opening the screen saver timeout policy setting*

![GPO HR Desktop Lockout Set 300 Second Timeout](./phase-3-group-policy/screenshots/33C_GPO_HR_Desktop_Lockout_Set_300_Second_Timeout.png)
*Setting screen saver timeout to 300 seconds (5 minutes) for HR workstations*

### Final verification

![GPMC HR OU Linked GPOs Verification](./phase-3-group-policy/screenshots/34C_GPMC_HR_OU_Linked_GPOs_Verification.png)
*GPMC showing both GPOs correctly linked — Password Policy at domain level, Desktop Lockout at HR OU level*

---

## Phase 4 — Domain Join & Initial User Logon

**Goal:** Join the Windows 11 workstation to the `technocrat.local` domain and verify that domain policies and profiles apply correctly.

- [x] Set client DNS to point to `Technocrat-DC01` (`192.168.1.10`)
- [x] Join `Workstation.1` to the `technocrat.local` domain
- [x] Test domain user authentication and profile creation
- [x] Verify GPO application with `gpresult /r` and `gpupdate /force`

### Preparing the client for domain join

![Initial Local Logon Prompt](./phase-4-client-domain-join/screenshots/1D_Initial_Local_Logon_Prompt.png)
*Logging into the Windows 11 workstation locally as Workstation.1 before domain join*

![Navigate Settings DNS Config](./phase-4-client-domain-join/screenshots/2D_Navigate_Settings_DNS_Config.png)
*Opening network settings on the client to configure DNS*

![Network Internet Settings](./phase-4-client-domain-join/screenshots/3D_Network_Internet_Settings.png)
*Network and Internet settings panel on Windows 11*

![Edit DNS Settings Manual Selection](./phase-4-client-domain-join/screenshots/4D_Edit_DNS_Settings_Manual_Selection.png)
*Switching DNS configuration to manual entry*

![Enable IPv4 DNS Manual Config](./phase-4-client-domain-join/screenshots/5D_Enable_IPv4_DNS_Manual_Config.png)
*Enabling manual IPv4 DNS configuration*

![Set Preferred DNS to DC IP](./phase-4-client-domain-join/screenshots/6D_Set_Preferred_DNS_to_DC_IP.png)
*Setting the preferred DNS server to 192.168.1.10 — pointing the client at Technocrat-DC01*

![Verify Client DNS Assignment](./phase-4-client-domain-join/screenshots/7D_Verify_Client_DNS_Assignment.png)
*Confirming the DNS setting is saved correctly on the client*

### Troubleshooting connectivity

![Quick Link Menu Select System](./phase-4-client-domain-join/screenshots/8D_Quick_Link_Menu_Select_System.png)
*Opening System properties via the Quick Link menu*

![System About Select Advanced Settings](./phase-4-client-domain-join/screenshots/9D_System_About_Select_Advanced_Settings.png)
*Navigating to Advanced System Settings*

![System Properties Computer Name Tab](./phase-4-client-domain-join/screenshots/10D_System_Properties_Computer_Name_Tab.png)
*System Properties Computer Name tab — starting point for domain join*

![Computer Name Changes Dialog](./phase-4-client-domain-join/screenshots/11D_Computer_Name_Changes_Dialog.png)
*Computer Name/Domain Changes dialog box*

![Rename Client Computer Name Field](./phase-4-client-domain-join/screenshots/13D_Rename_Client_Computer_Name_Field.png)
*Entering the computer name for the workstation*

![Set Computer Name and Domain Field](./phase-4-client-domain-join/screenshots/14D_Set_Computer_Name_And_Domain_Field.png)
*Entering technocrat.local in the domain field to initiate the domain join*

![Domain Controller Unreachable DNS Error](./phase-4-client-domain-join/screenshots/15D_Domain_Controller_Unreachable_DNS_Error.png)
*Domain controller unreachable error — DNS not resolving correctly, troubleshooting begins*

![Manual IPv4 Static IP Assignment](./phase-4-client-domain-join/screenshots/16D_Manual_IPv4_Static_IP_Assignment.png)
*Assigning a static IP to the client to ensure it is on the same subnet as the DC*

![Ping DC IP Connectivity Success](./phase-4-client-domain-join/screenshots/17D_Ping_DC_IP_Connectivity_Success.png)
*Pinging the DC at 192.168.1.10 — successful reply confirms network connectivity*

![NSLookup Verify Domain DNS Resolution](./phase-4-client-domain-join/screenshots/18D_NSLookup_Verify_Domain_DNS_Resolution.png)
*Running NSLookup to verify the client can resolve technocrat.local via the DC's DNS*

![Retry Domain Join After DNS Fix](./phase-4-client-domain-join/screenshots/19D_Retry_Domain_Join_After_DNS_Fix.png)
*Retrying the domain join after resolving the DNS connectivity issue*

### Joining the domain

![Domain Admin Credentials Join Prompt](./phase-4-client-domain-join/screenshots/20D_Domain_Admin_Credentials_Join_Prompt.png)
*Entering domain administrator credentials to authorize the domain join*

![Confirm Domain Admin Credentials](./phase-4-client-domain-join/screenshots/21D_Confirm_Domain_Admin_Credentials.png)
*Confirming the administrator credentials for technocrat.local*

![Welcome to Domain Success Message](./phase-4-client-domain-join/screenshots/22D_Welcome_To_Domain_Success_Message.png)
*"Welcome to the technocrat.local domain" — domain join successful*

![Restart Now Post Domain Join](./phase-4-client-domain-join/screenshots/23D_Restart_Now_Post_Domain_Join.png)
*Restarting the workstation to complete the domain join process*

### First domain user logon

![Select Other User For Domain Logon](./phase-4-client-domain-join/screenshots/24D_Select_Other_User_For_Domain_Logon.png)
*Selecting "Other user" on the login screen to log in as a domain account*

![Domain User Abel Schu Initial Logon](./phase-4-client-domain-join/screenshots/25D_Domain_User_Abel_Schu_Initial_Logon.png)
*Logging in as domain user Abel Scheheelein for the first time on the workstation*

![First Logon Password Change Requirement](./phase-4-client-domain-join/screenshots/26D_First_Logon_Password_Change_Requirement.png)
*Password change prompt on first logon — confirming the "must change at next logon" policy is enforced*

![Password Change Confirmation Success](./phase-4-client-domain-join/screenshots/27D_Password_Change_Confirmation_Success.png)
*Password successfully changed — user proceeds to the desktop*

![Hyper-V Enhanced Session RDP Restriction Error](./phase-4-client-domain-join/screenshots/28D_Hyper-V_Enhanced_Session_RDP_Restriction_Error.png)
*Enhanced Session RDP restriction error encountered — documented as a real troubleshooting scenario*

![Enter Domain User Credentials TECHNOCRAT](./phase-4-client-domain-join/screenshots/29D_Enter_Domain_User_Credentials_TECHNOCRAT.png)
*Entering TECHNOCRAT domain credentials at the login prompt*

![Verify Domain User Profile Start Menu](./phase-4-client-domain-join/screenshots/30D_Verify_Domain_User_Profile_Start_Menu.png)
*Domain user profile successfully created — Start Menu confirming Abel is logged in*

### Post-logon verification

![Whoami All Verify Domain User Context](./phase-4-client-domain-join/screenshots/31D_Whoami_All_Verify_Domain_User_Context.png)
*Running whoami /all in Command Prompt — confirming domain user context, group memberships, and SID*

---

## Phase 5 — DNS, DHCP & Shared Folders

**Goal:** Configure core network services and establish a centralized file share with proper permission management.

- [x] Configure Reverse Lookup Zones in DNS Manager
- [x] Set up DHCP scope for automatic IP assignment to domain clients
- [x] Create a central file share with layered NTFS and Share permissions

#### Key Concepts Practiced

- **Reverse Lookup Zones** — Allow DNS to resolve IP addresses back to hostnames, essential for logging and troubleshooting.
- **DHCP Scope** — Defined an IP range for the lab subnet, with the DC set as the DNS server option so joined clients automatically use the correct resolver.
- **NTFS vs Share Permissions** — Both layers were configured intentionally. Share permissions control network access; NTFS permissions control what authenticated users can do with the files themselves.

### DNS configuration

![Server Manager Tools Select DNS](./phase-5-dns-dhcp-shared-folders/screenshots/1E_Server_Manager_Tools_Select_DNS.png)
*Opening DNS Manager from the Server Manager Tools menu*

![DNS Manager Server Node Overview](./phase-5-dns-dhcp-shared-folders/screenshots/2E_DNS_Manager_Server_Node_Overview.png)
*DNS Manager opening view showing Technocrat-DC01 as the DNS server*

![Expand DC01 DNS Server Nodes](./phase-5-dns-dhcp-shared-folders/screenshots/3E_Expand_DC01_DNS_Server_Nodes.png)
*Expanding the DC01 node to view Forward and Reverse Lookup Zones*

![Forward Lookup Zones Directory Structure](./phase-5-dns-dhcp-shared-folders/screenshots/4E_Forward_Lookup_Zones_Directory_Structure.png)
*Forward Lookup Zones showing the technocrat.local zone already created during DC promotion*

![Right Click Domain Select New Host](./phase-5-dns-dhcp-shared-folders/screenshots/5E_Right_Click_Domain_Select_New_Host.png)
*Right-clicking the technocrat.local zone to add a new A record*

![Create New Host Record Fileserver](./phase-5-dns-dhcp-shared-folders/screenshots/6E_Create_New_Host_Record_Fileserver.png)
*Creating a new A record for fileserver.technocrat.local*

![Fileserver Host A Record Success Confirmation](./phase-5-dns-dhcp-shared-folders/screenshots/7E_Fileserver_Host_A_Record_Success_Confirmation.png)
*Confirmation that the fileserver A record was created successfully*

![Verify Fileserver A Record in DNS Manager](./phase-5-dns-dhcp-shared-folders/screenshots/8E_Verify_Fileserver_A_Record_In_DNS_Manager.png)
*DNS Manager showing the new fileserver.technocrat.local A record in the zone*

### Installing DHCP

![Server Manager Add Roles Features](./phase-5-dns-dhcp-shared-folders/screenshots/9E_Server_Manager_Select_Add_Roles_Features.png)
*Launching Add Roles and Features to install the DHCP Server role*

![Select Role Based Or Feature Based Installation](./phase-5-dns-dhcp-shared-folders/screenshots/10E_Select_Role_Based_Or_Feature_Based_Installation.png)
*Selecting Role-based installation in the Add Roles wizard*

![Select DC01 From Destination Server Pool](./phase-5-dns-dhcp-shared-folders/screenshots/11E_Select_DC01_From_Destination_Server_Pool.png)
*Selecting Technocrat-DC01 as the destination server*

![Select DHCP Server Role Installation](./phase-5-dns-dhcp-shared-folders/screenshots/12E_Select_DHCP_Server_Role_Installation.png)
*Checking the DHCP Server role from the server roles list*

![Add Required DHCP Management Tools](./phase-5-dns-dhcp-shared-folders/screenshots/13E_Add_Required_DHCP_Management_Tools.png)
*Confirming required DHCP management tools to be installed alongside the role*

![Select Features RSAT DHCP Tools](./phase-5-dns-dhcp-shared-folders/screenshots/14E_Select_Features_RSAT_DHCP_Tools.png)
*Selecting RSAT DHCP Tools for remote management capability*

![Confirm DHCP Server Installation Selections](./phase-5-dns-dhcp-shared-folders/screenshots/15E_Confirm_DHCP_Server_Installation_Selections.png)
*Confirmation screen before installing the DHCP Server role*

![DHCP Server Installation Progress DC01](./phase-5-dns-dhcp-shared-folders/screenshots/16E_DHCP_Server_Installation_Progress_DC01.png)
*DHCP Server role installation in progress*

![Complete DHCP Post Deployment Configuration Wizard](./phase-5-dns-dhcp-shared-folders/screenshots/17E_Complete_DHCP_Post_Deployment_Configuration_Wizard.png)
*Launching the DHCP post-install configuration wizard from the yellow flag notification*

![DHCP Server Post Install Authorization Credentials](./phase-5-dns-dhcp-shared-folders/screenshots/18E_DHCP_Server_Post_Install_Authorization_Credentials.png)
*Authorizing the DHCP server in Active Directory using domain admin credentials*

![DHCP AD DS Authorization Confirmation Success](./phase-5-dns-dhcp-shared-folders/screenshots/19E_DHCP_AD_DS_Authorization_Confirmation_Success.png)
*DHCP server successfully authorized in Active Directory*

![DHCP Post Install Configuration Success](./phase-5-dns-dhcp-shared-folders/screenshots/20E_DHCP_Post_Install_Configuration_Success.png)
*Post-install configuration complete — DHCP server is active and authorized*

### Configuring DHCP scope

![Search And Launch DHCP Management Console](./phase-5-dns-dhcp-shared-folders/screenshots/21E_Search_And_Launch_DHCP_Management_Console.png)
*Opening the DHCP Management Console*

![DHCP Management Console DC01 Node](./phase-5-dns-dhcp-shared-folders/screenshots/22E_DHCP_Management_Console_DC01_Node.png)
*DHCP console showing DC01 as the authorized DHCP server*

![Expand IPv4 Scope Management](./phase-5-dns-dhcp-shared-folders/screenshots/23E_Expand_IPv4_Scope_Management.png)
*Expanding the IPv4 node to begin scope creation*

![Right Click New Scope Wizard](./phase-5-dns-dhcp-shared-folders/screenshots/24E_Right_Click_New_Scope_Wizard.png)
*Right-clicking IPv4 to launch the New Scope Wizard*

![Welcome To New Scope Wizard](./phase-5-dns-dhcp-shared-folders/screenshots/25E_Welcome_To_New_Scope_Wizard.png)
*New Scope Wizard welcome screen*

![Set DHCP Scope Name Technocrat Clients](./phase-5-dns-dhcp-shared-folders/screenshots/26E_Set_DHCP_Scope_Name_Technocrat_Clients.png)
*Naming the DHCP scope "Technocrat_Clients"*

![Configure DHCP Scope IP Address Range](./phase-5-dns-dhcp-shared-folders/screenshots/27E_Configure_DHCP_Scope_IP_Address_Range.png)
*Setting the IP address range for the DHCP scope*

![Skip DHCP Exclusions And Delays](./phase-5-dns-dhcp-shared-folders/screenshots/28E_Skip_DHCP_Exclusions_And_Delays.png)
*Skipping exclusions — no IPs need to be reserved from the scope at this stage*

![Set DHCP Scope Lease Duration 8 Days](./phase-5-dns-dhcp-shared-folders/screenshots/29E_Set_DHCP_Scope_Lease_Duration_8_Days.png)
*Setting the DHCP lease duration to 8 days*

![Select Configure DHCP Options Now](./phase-5-dns-dhcp-shared-folders/screenshots/30E_Select_Configure_DHCP_Options_Now.png)
*Choosing to configure DHCP options immediately after scope creation*

![Skip Default Gateway Router Address Config](./phase-5-dns-dhcp-shared-folders/screenshots/31E_Skip_Default_Gateway_Router_Address_Config.png)
*Skipping default gateway — not required for this lab scope*

![Domain Name And DNS Servers Config](./phase-5-dns-dhcp-shared-folders/screenshots/32E_Domain_Name_And_DNS_Servers_Config.png)
*Setting technocrat.local as the domain name and 192.168.1.10 as the DNS server for DHCP clients*

![Activate New DHCP Scope Now](./phase-5-dns-dhcp-shared-folders/screenshots/33E_Activate_New_DHCP_Scope_Now.png)
*Activating the new DHCP scope immediately*

![Complete New Scope Wizard Finish](./phase-5-dns-dhcp-shared-folders/screenshots/34E_Complete_New_Scope_Wizard_Finish.png)
*New Scope Wizard complete — Technocrat_Clients scope is now active*

### Creating shared folders

![Create New Folder Shares On C Drive](./phase-5-dns-dhcp-shared-folders/screenshots/35E_Create_New_Folder_Shares_On_C_Drive.png)
*Creating a new Shares folder on the C: drive to host shared resources*

![Create New Folder Shares On C Drive Detail](./phase-5-dns-dhcp-shared-folders/screenshots/36E_Create_New_Folder_Shares_On_C_Drive.png)
*Shares folder created at C:\Shares on Technocrat-DC01*

![Create Sales Subfolder Within Shares](./phase-5-dns-dhcp-shared-folders/screenshots/37E_Create_Sales_Subfolder_Within_Shares.png)
*Creating a Sales subfolder inside C:\Shares for the Sales department*

![Sales Folder Properties Menu Select](./phase-5-dns-dhcp-shared-folders/screenshots/38E_Sales_Folder_Properties_Menu_Select.png)
*Opening the Sales folder properties to configure sharing*

![Select Advanced Sharing Options](./phase-5-dns-dhcp-shared-folders/screenshots/39E_Select_Advanced_Sharing_Options.png)
*Selecting Advanced Sharing to configure share permissions manually*

![Enable Advanced Sharing Shares Folder](./phase-5-dns-dhcp-shared-folders/screenshots/40E_Enable_Advanced_Sharing_Shares_Folder.png)
*Enabling Advanced Sharing on the Sales folder*

### Configuring share & NTFS permissions

![Add Users Or Groups To Share Permissions](./phase-5-dns-dhcp-shared-folders/screenshots/41E_Add_Users_Or_Groups_To_Share_Permissions.png)
*Adding users and groups to the Share permissions of the Sales folder*

![Add Users Or Groups Share Permissions Detail](./phase-5-dns-dhcp-shared-folders/screenshots/42E_Add_Users_Or_Groups_Share_Permissions.png)
*Selecting specific security groups to grant share access*

![Assign Sales Staff Group To Share Permissions](./phase-5-dns-dhcp-shared-folders/screenshots/43E_Assign_Sales-Staff_Group_To_Share_Permissions.png)
*Assigning the Sales-Staff security group to the share permissions*

![Grant Change And Read Share Permissions To Sales Staff](./phase-5-dns-dhcp-shared-folders/screenshots/44E_Grant_Change_And_Read_Share_Permissions_To_Sales-Staff.png)
*Granting Change and Read share permissions to Sales-Staff*

![Shares Security Tab Edit NTFS Permissions](./phase-5-dns-dhcp-shared-folders/screenshots/45E_Shares_Security_Tab_Edit_NTFS_Permissions.png)
*Switching to the Security tab to configure NTFS permissions separately from share permissions*

![Advanced Security Settings Read NTFS Permissions](./phase-5-dns-dhcp-shared-folders/screenshots/46E_Advanced_Security_Settings_Read_NTFS_Permissions.png)
*Opening Advanced Security Settings to review current NTFS permission entries*

![Convert Inherited Permissions Remove Users NTFS](./phase-5-dns-dhcp-shared-folders/screenshots/47E_Convert_Inherited_Permissions_Remove_Users_NTFS.png)
*Converting inherited permissions to explicit entries before making changes*

![Remove Standard Users From NTFS Access List](./phase-5-dns-dhcp-shared-folders/screenshots/48E_Remove_Standard_Users_From_NTFS_Access_List.png)
*Removing default Users group from NTFS access list to enforce least privilege*

![Advanced Security Settings Verify Clean NTFS Access List](./phase-5-dns-dhcp-shared-folders/screenshots/49E_Advanced_Security_Settings_Verify_Clean_NTFS_Access_List.png)
*Verifying the cleaned NTFS access list before adding the correct groups*

![Select Principal For NTFS Permission Entry](./phase-5-dns-dhcp-shared-folders/screenshots/50E_Select_Principal_For_NTFS_Permission_Entry.png)
*Selecting the security principal to assign NTFS permissions to*

![Assign Sales Staff As NTFS Principal](./phase-5-dns-dhcp-shared-folders/screenshots/51E_Assign_Sales-Staff_As_NTFS_Principal.png)
*Assigning the Sales-Staff group as the NTFS permission principal*

![Configure NTFS Read And Execute Permissions For Sales Staff](./phase-5-dns-dhcp-shared-folders/screenshots/52E_Configure_NTFS_Read_And_Execute_Permissions_For_Sales-Staff.png)
*Configuring Read and Execute NTFS permissions for the Sales-Staff group*

### Drive mapping via GPO

![Create And Link GPO To Sales OU](./phase-5-dns-dhcp-shared-folders/screenshots/53E_Create_And_Link_GPO_To_Sales_OU.png)
*Creating and linking a new GPO to the Sales OU for drive mapping*

![New GPO Create Mapped Drive Sales S](./phase-5-dns-dhcp-shared-folders/screenshots/54E_New_GPO_Create_Mapped_Drive_Sales-S.png)
*Naming the new GPO for the Sales drive mapping*

![Group Policy Management Edit Mapped Drive Sales S](./phase-5-dns-dhcp-shared-folders/screenshots/55E_Group_Policy_Management_Edit_MappedDrive-Sales-S.png)
*Opening the Sales drive mapping GPO in the Group Policy Management Editor*

![GPMC Editor User Configuration Node](./phase-5-dns-dhcp-shared-folders/screenshots/56E_GPMC_Editor_User_Configuration_Node.png)
*Navigating to User Configuration in the GPO editor*

![GPMC Editor User Preferences Selection](./phase-5-dns-dhcp-shared-folders/screenshots/57E_GPMC_Editor_User_Preferences_Selection.png)
*Expanding Preferences under User Configuration*

![GPMC Editor User Preferences Windows Settings](./phase-5-dns-dhcp-shared-folders/screenshots/58E_GPMC_Editor_User_Preferences_Windows_Settings.png)
*Navigating into Windows Settings under User Preferences*

![GPMC Editor Drive Maps Selection](./phase-5-dns-dhcp-shared-folders/screenshots/59E_GPMC_Editor_Drive_Maps_Selection.png)
*Selecting Drive Maps to create a new mapped drive entry*

![Right Click GPO Drive Map Select New Mapped Drive](./phase-5-dns-dhcp-shared-folders/screenshots/60E_Right_Click_GPO_Drive_Map_Select_New_Mapped_Drive.png)
*Right-clicking to create a new mapped drive in the GPO*

![Configure GPO Drive Map S Path And Label](./phase-5-dns-dhcp-shared-folders/screenshots/61E_Configure_GPO_Drive_Map_S_Path_And_Label.png)
*Configuring the mapped drive — UNC path to \\Technocrat-DC01\Sales and drive letter S:*

![GPO Common Tab Run In Logged On Security Context](./phase-5-dns-dhcp-shared-folders/screenshots/62E_GPO_Common_Tab_Run_In_Logged-On_Security_Context.png)
*Setting the GPO Common tab to run in the logged-on user's security context*

### Verification

![Logon To Sales M Lewis](./phase-5-dns-dhcp-shared-folders/screenshots/63E_Logon_To_Sales-Staff_Account_M-Lewis.png)
*Logging onto the workstation as Sales user M. Lewis to test the drive mapping*

![Verify S Drive Mapped Via Group Policy](./phase-5-dns-dhcp-shared-folders/screenshots/64E_Verify_S_Drive_Mapped_Via_Group_Policy.png)
*S: drive successfully mapped via Group Policy on the Sales user's desktop*

![Verify Sales Folder Access Within S Drive](./phase-5-dns-dhcp-shared-folders/screenshots/65E_Verify_Sales_Folder_Access_Within_S_Drive.png)
*Confirming Sales-Staff can access and browse the S: drive contents as expected*

---

## Phase 6 — Help Desk Simulation Scenarios

**Goal:** Simulate the most common Active Directory tasks a help desk technician handles on a daily basis.

- [x] Unlock a locked-out user account
- [x] Perform a password reset with "must change at next logon" enforced
- [x] Disable a user account and move it to the `Disabled Accounts` OU
- [x] Add a user to a security group
- [x] Resolve a DNS resolution issue
- [x] Use PowerShell to query last logon date: `Get-ADUser -Identity <user> -Properties LastLogonDate`

### Finding and unlocking a user account

![ADUC Search Icon Select](./phase-6-helpdesk-simulations/screenshots/1F_ADUC_Search_Icon_Select.png)
*Using the ADUC search function to locate a user account quickly*

![ADUC Find Users Contacts Groups Dialog](./phase-6-helpdesk-simulations/screenshots/2F_ADUC_Find_Users_Contacts_Groups_Dialog.png)
*Find Users, Contacts and Groups dialog — searching across the entire domain*

![ADUC Search Result Emily Scott](./phase-6-helpdesk-simulations/screenshots/3F_ADUC_Search_Result_Emily_Scott.png)
*Search results returning Emily Scott's account*

![Unlock Emily Scott Account Properties](./phase-6-helpdesk-simulations/screenshots/4F_Unlock_Emily_Scott_Account_Properties.png)
*Opening Emily Scott's account properties — Account tab showing the account is locked out*

### Password reset

![Right Click User Select Reset Password](./phase-6-helpdesk-simulations/screenshots/5F_Right_Click_User_Select_Reset_Password.png)
*Right-clicking a user account to select Reset Password*

![Enter New Password Enforce Change At Logon](./phase-6-helpdesk-simulations/screenshots/6F_Enter_New_Password_Enforce_Change_At_Logon.png)
*Setting a temporary password with "User must change password at next logon" enforced*

### Disabling an account

![Right Click Karen Disable Account](./phase-6-helpdesk-simulations/screenshots/7F_Right_Click_Karen_Disable_Account.png)
*Right-clicking Karen's account and selecting Disable Account*

![Move Disabled Account To Disabled Accounts OU](./phase-6-helpdesk-simulations/screenshots/8F_Move_Disabled_Account_To_Disabled_Accounts_OU.png)
*Moving the disabled account into the Disabled Accounts OU for clean AD organization*

### Adding a user to a group

![VPN Users Group Properties Members Tab](./phase-6-helpdesk-simulations/screenshots/9F_VPN_Users_Group_Properties_Members_Tab.png)
*Opening the GS-VPN-Users group properties to view and manage members*

![Add Brian Hall To VPN Users Group](./phase-6-helpdesk-simulations/screenshots/10F_Add_Brian_Hall_To_VPN_Users_Group.png)
*Adding Brian Hall to the GS-VPN-Users security group*

![Confirm Brian Hall Membership VPN Users](./phase-6-helpdesk-simulations/screenshots/11F_Confirm_Brian_Hall_Membership_VPN_Users.png)
*Members tab confirming Brian Hall is now a member of GS-VPN-Users*

### Advanced features & PowerShell query

![ADUC View Menu Enable Advanced Features](./phase-6-helpdesk-simulations/screenshots/12F_ADUC_View_Menu_Enable_Advanced_Features.png)
*Enabling Advanced Features in the ADUC View menu to expose additional account attributes*

![Right Click Abel Schu Select Properties](./phase-6-helpdesk-simulations/screenshots/13F_Right_Click_Abel_Schu_Select_Properties.png)
*Opening Abel Scheheelein's full properties with Advanced Features enabled*

![Abel Schu Properties Attribute Editor Last Logon](./phase-6-helpdesk-simulations/screenshots/14F_Abel_Schu_Properties_Attribute_Editor_LastLogon.png)
*Attribute Editor tab showing Abel's LastLogon timestamp — useful for identifying inactive accounts*

![PowerShell Get-ADUser Abel Schu Last Logon Date](./phase-6-helpdesk-simulations/screenshots/15F_PowerShell_Get-ADUser_Abel_Schu_LastLogonDate.png)
*Running Get-ADUser -Identity Abel_Schu -Properties LastLogonDate in PowerShell — confirming the same data via command line*

---

## Phase 50 (Z) — PowerShell Automation Script

**Goal:** The Grand Finale — a PowerShell script that automates bulk user creation from a CSV file, demonstrating real-world scripting and AD automation skills.

- [x] Prepare a CSV file containing 50 randomly generated user first and last names
- [x] Store the CSV in a dedicated lab folder for clean organization
- [x] Launch PowerShell ISE as Administrator
- [x] Write and execute a script to loop through the CSV and create all 50 users in ADUC under the Lab-Users OU
- [x] Verify all 50 accounts appear correctly in Active Directory Users and Computers

### Building and running the bulk user import script

![User Data Source CSV File Preparation](./phase-50-powershell-script/screenshots/1Z_User_Data_Source_CSV_File_Preparation.png)
*The CSV file containing 50 randomly generated user first and last names — the data source for the bulk import script*

![Initializing Users50 CSV Import File](./phase-50-powershell-script/screenshots/2Z_Initializing_Users50_CSV_Import_File.png)
*Initializing the CSV file in preparation for the PowerShell import script*

![Storing Users50 CSV in Lab Root Folder](./phase-50-powershell-script/screenshots/3Z_Storing_Users50_CSV_in_Lab_Root_Folder.png)
*Storing the CSV file in a dedicated lab root folder for clean file organization*

![Launching PowerShell ISE as Administrator](./phase-50-powershell-script/screenshots/4Z_Launching_PowerShell_ISE_as_Administrator.png)
*Launching PowerShell ISE as Administrator — required to execute AD cmdlets*

![PowerShell Script Bulk Importing Users from CSV](./phase-50-powershell-script/screenshots/5Z_PowerShell_Script_Bulk_Importing_Users_from_CSV.png)
*The PowerShell script running — looping through the CSV and creating each user account in ADUC under the Lab-Users OU*

![ADUC Bulk User Import Verification Lab Users](./phase-50-powershell-script/screenshots/6Z_ADUC_Bulk_User_Import_Verification_Lab-Users.png)
*ADUC confirming all 50 users successfully imported into the Lab-Users OU — script executed without errors*

---

## Skills Demonstrated

| Category | Skills |
|---|---|
| Virtualization | Hyper-V, Generation 2 VMs, VHDX management, TPM/Secure Boot configuration |
| Windows Server | Windows Server 2025 deployment, Server Manager, role installation |
| Active Directory | AD DS, OU design, user & group management, domain promotion |
| Group Policy | GPMC, GPO creation, linking, inheritance, enforcement, gpresult |
| Network Services | DNS (forward/reverse zones), DHCP (scopes, leases, options) |
| File Services | Shared folders, NTFS permissions, Share permissions |
| Help Desk | Account unlocks, password resets, user disables, group membership management |
| PowerShell | Bulk user import via CSV, AD queries, automation scripting |
| Documentation | Annotated screenshots, step-by-step walkthroughs, GitHub portfolio |

---
This has been an Abel Schuehlein x Technocrat Lab Production. Feel free to follow this lab along and reach out with any critiques!
