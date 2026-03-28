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

<details>
  <summary><b>Click to expand: Phase 2 - OU Structure, User Import & Group Nesting (48 Screenshots)</b></summary>

  ### Opening ADUC & creating the OU structure
  *Establishing a logical hierarchy within Active Directory to organize users, computers, and groups by department.*

  ![ADUC Initial Launch Technocrat Local](./Phase2.OUs_Users_Groups/01B.%20ADUC_Initial_Launch_Technocrat_Loca.png)
  *Active Directory Users and Computers opening view showing the technocrat.local domain*

  ![ADUC Domain New Organizational Unit Menu](./Phase2.OUs_Users_Groups/02B.%20ADUC_Domain_New_Organizational_Unit_Menu.png)
  *Right-clicking the domain to create a new top-level Organizational Unit*

  ![ADUC New Object Organizational Unit Dialog](./Phase2.OUs_Users_Groups/03B.%20ADUC_New_Object_Organizational_Unit_Dialog.png)
  *New Object dialog for creating an Organizational Unit*

  ![ADUC Create Top Level Technocrat OU](./Phase2.OUs_Users_Groups/04B.%20ADUC_Create_Top_Level_Technocrat_OU.png)
  *Creating the top-level Technocrat OU that will contain all department OUs*

  ![Nesting Department OUs Inside Technocrat OU](./Phase2.OUs_Users_Groups/05B.%20Nesting_Departmental_OUs_Inside_Technocrat.png)
  *Creating department OUs nested inside the Technocrat OU*

  ![ADUC Technocrat Nested OU Structure Complete](./Phase2.OUs_Users_Groups/06B.%20ADUC_Technocrat_Nested_OU_Structure_Complete.png)
  *Completed OU structure showing IT, HR, Sales, Disabled Accounts, and Lab-Users nested under Technocrat*

  ---

  ### Bulk user import
  *Moving bulk-imported accounts into their respective department OUs to reflect the corporate structure.*

  ![ADUC Organizing Bulk Users to Department OUs](./Phase2.OUs_Users_Groups/07B.%20ADUC_Organizing_Bulk_Users_to_Department_OUs.png)
  *Moving bulk-imported users from Lab-Users into their respective department OUs*

  ![ADUC Bulk Move Users to IT OU](./Phase2.OUs_Users_Groups/08B.%20ADUC_Bulk_Move_Users_to_IT_OU.png)
  *Bulk-selecting and moving users into the IT OU*

  ![ADUC Move Objects Warning Confirmation](./Phase2.OUs_Users_Groups/09B.%20ADUC_Move_Objects_Warning_Confirmatio.png)
  *Confirming the move operation warning dialog*

  ![ADUC Final Technocrat OU Structure Expanded](./Phase2.OUs_Users_Groups/10B.%20ADUC_Final_Technocrat_OU_Structure_Expanded.png)
  *Final expanded OU tree showing users distributed across all department OUs*

  ![ADUC Disabled Accounts OU Populated](./Phase2.OUs_Users_Groups/11B.%20ADUC_Disabled_Accounts_OU_Populated.png)
  *Disabled Accounts OU populated — ready to receive deactivated user accounts*

  ![ADUC HR OU Users List](./Phase2.OUs_Users_Groups/12B.%20ADUC_HR_OU_Users_List.png)
  *HR OU showing its assigned user accounts*

  ![ADUC IT OU Users List](./Phase2.OUs_Users_Groups/13B.%20ADUC_IT_OU_Users_List.png)
  *IT OU showing its assigned user accounts*

  ![ADUC Sales OU Users List](./Phase2.OUs_Users_Groups/14B.%20ADUC_Sales_OU_Users_List.png)
  *Sales OU showing its assigned user accounts*

  ![ADUC Accounting OU Users List](./Phase2.OUs_Users_Groups/15B.%20ADUC_Accounting_OU_Users_List.png)
  *Accounting OU showing its assigned user accounts*

  ![ADUC Marketing OU Users List](./Phase2.OUs_Users_Groups/16B.%20ADUC_Marketing_OU_Users_List.png)
  *Marketing OU showing its assigned user accounts*

  ---

  ### Creating a new user manually
  *Demonstrating the manual creation of a user object, including credential setup and property verification.*

  ![ADUC IT OU New Object Name Menu](./Phase2.OUs_Users_Groups/17B.%20ADUC_IT_OU_New_User_Creation_Menu.png)
  *Right-clicking the IT OU to manually create a new user object*

  ![ADUC New User Initial Password Logon Details](./Phase2.OUs_Users_Groups/18B.%20ADUC_New_User_Object_Name_and_Logon_Details.png)
  *Setting the initial password and logon options for the new user*

  ![ADUC New User Object Name and Security Setup](./Phase2.OUs_Users_Groups/19B.%20ADUC_New_User_Initial_Password_Security_Setup.png)
  *Entering the new user's name and security settings*

  ![ADUC New User Summary Confirmation Abel](./Phase2.OUs_Users_Groups/20B.%20ADUC_New_User_Summary_Confirmation_Abel_Schuehlein.png)
  *Summary confirmation screen before finalizing the new user account*

  ![ADUC User Properties Abel Scheheelein](./Phase2.OUs_Users_Groups/21B.%20ADUC_User_Properties_Abel_Schuehlein.png)
  *User properties for Abel Schuehlein showing account details*

  ![ADUC User Properties Default Group Members](./Phase2.OUs_Users_Groups/22B.%20ADUC_User_Properties_Default_Group_Membership.png)
  *Member Of tab showing Abel's default group membership (Domain Users)*

  ---

  ### Creating security groups
  *Implementing a centralized group management strategy to streamline access control.*

  ![Creating First Security Group Members](./Phase2.OUs_Users_Groups/23B.%20Creating_First_Group.png)
  *Creating the first security group and adding initial members*

  ![ADUC Create Security Group IT Admins](./Phase2.OUs_Users_Groups/24B.%20ADUC_Create_Security_Group_IT-Admins.png)
  *Creating the IT-Admins security group*

  ![ADUC IT OU Security Group Created Verification](./Phase2.OUs_Users_Groups/25B.%20ADUC_IT_OU_Security_Group_Created_Verification.png)
  *Verifying the IT-Admins group appears in the IT OU*

  ![ADUC Create Groups OU](./Phase2.OUs_Users_Groups/26B.%20ADUC_Create_Groups_OU.png)
  *Creating a dedicated Groups OU to organize all security groups*

  ![Centralizing Domain Security Groups Creation](./Phase2.OUs_Users_Groups/27B.%20Centralizing_Domain_Security_Groups_Creation.png)
  *Centralizing all security groups into the Groups OU for clean AD organization*

  ![ADUC Create Security Group VPN User](./Phase2.OUs_Users_Groups/28B.%20ADUC_Create_Security_Group_VPN-User.png)
  *Creating the VPN-Users security group*

  ![ADUC Groups OU VPN Users](./Phase2.OUs_Users_Groups/29B.%20ADUC_Groups_OU_VPN-Users.png)
  *Groups OU showing VPN-Users group created*

  ---

  ### Assigning users to groups
  *Applying the principle of AGDLP (Accounts, Global, Domain Local, Permissions) through group nesting.*

  ![ADUC User Properties Member Tab Abel](./Phase2.OUs_Users_Groups/30B.%20ADUC_User_Properties_Member_Of_Tab_Abel_Schuehlein.png)
  *Opening Abel's Member Of tab to assign group memberships*

  ![ADUC Add User to IT Admins](./Phase2.OUs_Users_Groups/31B.%20ADUC_Add_User_to_IT-Admins_Security_Group.png)
  *Adding Abel to the IT-Admins security group*

  ![ADUC Abel Assigned to IT Admins Security Group](./Phase2.OUs_Users_Groups/32B.%20ADUC_Abel_Schuehlein_Assigned_to_IT-Admins.png)
  *Member Of tab confirming Abel is now a member of IT-Admins*

  ![ADUC Assigning Abel and Jason to IT Admins](./Phase2.OUs_Users_Groups/33B.%20ADUC_Assigning_Abel_and_Jason_to_IT-Admins.png)
  *Adding both Abel and Jason to the IT-Admins group*

  ![ADUC Groups OU Complete Security Group List](./Phase2.OUs_Users_Groups/34B.%20ADUC_Groups_OU_Complete_Security_Group_List.png)
  *Groups OU showing all security groups created for the domain*

  ![ADUC Create Domain Local Group DL VPN Access](./Phase2.OUs_Users_Groups/35B.%20ADUC_Create_Domain_Local_Group_DL-VPN-Access.png)
  *Creating a Domain Local group DL-VPN-Access for role-based access control*

  ![ADUC Nesting VPN Users into DL VPN Access](./Phase2.OUs_Users_Groups/36B.%20ADUC_Nesting_VPN-Users_into_DL-VPN-Access.png)
  *Nesting the VPN-Users global group inside the DL-VPN-Access domain local group*

  ![ADUC Verify Group Nesting VPN Users in DL VPN](./Phase2.OUs_Users_Groups/37B.%20ADUC_Verify_Group_Nesting_VPN-Users_in_DL-VPN-Access.png)
  *Verifying the group nesting is correctly configured*

  ![ADUC Assigning IT Department Users to VPN Users](./Phase2.OUs_Users_Groups/38B.%20ADUC_Assigning_IT_Department_Users_to_VPN-Users.png)
  *Assigning IT department users to the VPN-Users group*

  ![ADUC Deactivating User Account Steven Allen](./Phase2.OUs_Users_Groups/39B.%20ADUC_Deactivating_User_Account_Steven_Allen.png)
  *Deactivating Steven Allen's account and moving it to the Disabled Accounts OU*

  ---

  ### Group properties & delegation
  *Configuring administrative ownership and delegating specific OU controls to departmental admin groups.*

  ![ADUC Group Properties Assign Managed By](./Phase2.OUs_Users_Groups/40B.%20ADUC_Group_Properties_Assign_Managed_By_Abel_Schuehlein.png)
  *Setting the Managed By attribute on a security group*

  ![ADUC Delegate Group Membership Update](./Phase2.OUs_Users_Groups/41B.%20ADUC_Delegate_Group_Membership_Update_to_Manager.png)
  *Delegating group membership management to a specific user*

  ![ADUC Rename Group Prefix GS IT Admins](./Phase2.OUs_Users_Groups/42B.%20ADUC_Rename_Group_Prefix_GS_IT_Admins.png)
  *Renaming group with GS prefix to follow naming conventions*

  ![ADUC Rename Group Prefix GS VPN Users](./Phase2.OUs_Users_Groups/43B.%20ADUC_Rename_Group_Prefix_GS_VPN_Users.png)
  *Renaming VPN-Users group with GS prefix*

  ![ADUC Rename Group Prefix DL VPN Access](./Phase2.OUs_Users_Groups/44B.%20ADUC_Rename_Group_Prefix_DL_VPN_Access.png)
  *Renaming domain local group with DL prefix following naming conventions*

  ![ADUC Sales OU Delegate Control Wizard Launch](./Phase2.OUs_Users_Groups/45B.%20ADUC_Sales_OU_Delegate_Control_Wizard_Launch.png)
  *Launching the Delegation of Control Wizard on the Sales OU*

  ![Delegation Wizard Select GS IT Admins Group](./Phase2.OUs_Users_Groups/46B.%20Delegation_Wizard_Select_GS_IT_Admins_Group.png)
  *Selecting the GS-IT-Admins group to delegate control to*

  ![Delegation Wizard Selecting Common Administrative Tasks](./Phase2.OUs_Users_Groups/47B.%20Delegation_Wizard_Selecting_Common_Administrative_Tasks.png)
  *Selecting which administrative tasks to delegate to IT-Admins*

  ![Delegation Wizard Summary and Completion](./Phase2.OUs_Users_Groups/48B.%20Delegation_Wizard_Summary_and_Completion.png)
  *Delegation of Control Wizard summary confirming delegation is complete*

</details>

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

<details>
  <summary><b>Click to expand: Phase 3 - Group Policy Objects & Security Baselines (34 Screenshots)</b></summary>

  ### Opening GPMC & reviewing existing policy
  *Accessing the Group Policy Management Console to establish a baseline for the technocrat.local domain.*

  ![GPMC Console Navigation from Tools Menu](./Phase3.Create_Apply_GPOs/01C.%20GPMC_Console_Navigation_from_Tools_Menu.png)
  *Opening the Group Policy Management Console from Server Manager Tools menu*

  ![GPMC Technocrat Local Initial View](./Phase3.Create_Apply_GPOs/02C.%20GPMC_Technocrat_Local_Initial_View.png)
  *GPMC initial view showing the technocrat.local domain and existing Default Domain Policy*

  ![GPMC Edit Default Domain Policy Menu](./Phase3.Create_Apply_GPOs/03C.%20GPMC_Edit_Default_Domain_Policy_Menu.png)
  *Right-clicking the Default Domain Policy to open it for editing*

  ![GPM Editor Default Domain Policy Overview](./Phase3.Create_Apply_GPOs/04C.%20GPM_Editor_Default_Domain_Policy_Overview.png)
  *Group Policy Management Editor showing the full Default Domain Policy structure*

  ---

  ### Configuring the Password Policy
  *Hardening the domain by enforcing length, complexity, and age requirements for all user accounts.*

  ![GPM Editor Selecting Computer Configuration](./Phase3.Create_Apply_GPOs/05C.%20GPM_Editor_Selecting_Computer_Configuration_Node.png)
  *Navigating to Computer Configuration to locate security settings*

  ![GPM Editor Expanding Computer Policies Node](./Phase3.Create_Apply_GPOs/06C.%20GPM_Editor_Expanding_Computer_Policies_Node.png)
  *Expanding the Policies node under Computer Configuration*

  ![GPM Editor Selecting Windows Settings Node](./Phase3.Create_Apply_GPOs/07C.%20GPM_Editor_Selecting_Windows_Settings_Node.png)
  *Navigating into Windows Settings*

  ![GPM Editor Selecting Security Settings Node](./Phase3.Create_Apply_GPOs/08C.%20GPM_Editor_Selecting_Security_Settings_Node.png)
  *Selecting Security Settings to access Account Policies*

  ![GPM Editor Selecting Account Policies Node](./Phase3.Create_Apply_GPOs/09C.%20GPM_Editor_Selecting_Account_Policies_Node.png)
  *Opening Account Policies to reach Password Policy settings*

  ![GPM Editor Selecting Password Policy Node](./Phase3.Create_Apply_GPOs/10C.%20GPM_Editor_Selecting_Password_Policy_Node.png)
  *Selecting the Password Policy node to view and edit all password settings*

  ![GPM Editor Selecting Minimum Password Length](./Phase3.Create_Apply_GPOs/11C.%20GPM_Editor_Selecting_Minimum_Password_Length_Policy.png)
  *Opening the Minimum Password Length policy setting*

  ![GPM Editor Minimum Password Length Property](./Phase3.Create_Apply_GPOs/12C.%20GPM_Editor_Minimum_Password_Length_Properties_Default.png)
  *Setting the minimum password length value*

  ![Password Policy Set 10 Character Minimum](./Phase3.Create_Apply_GPOs/13C.%20Password_Policy_Set_10_Character_Minimum.png)
  *Minimum password length set to 10 characters*

  ![Password Policy Enable Complexity Requirements](./Phase3.Create_Apply_GPOs/14C.%20Password_Policy_Enable_Complexity_Requirements.png)
  *Enabling password complexity requirements — uppercase, lowercase, numbers, and symbols*

  ![Password Policy Complexity Requirement Properties](./Phase3.Create_Apply_GPOs/15C.%20Password_Policy_Complexity_Requirement_Properties_Enabled.png)
  *Complexity requirement properties confirming the setting is enabled*

  ![GPM Editor Selecting Maximum Password Age](./Phase3.Create_Apply_GPOs/16C.%20GPM_Editor_Selecting_Maximum_Password_Age_Policy.png)
  *Opening the Maximum Password Age setting*

  ![Password Policy Set 90 Day Maximum Password Age](./Phase3.Create_Apply_GPOs/17C.%20Password_Policy_Set_90_Day_Maximum_Password_Age.png)
  *Setting maximum password age to 90 days — users must change password every 90 days*

  ---

  ### Applying & verifying the policy
  *Forcing an immediate policy refresh to ensure the new security baseline is active.*

  ![Applying Group Policy Changes Immediately](./Phase3.Create_Apply_GPOs/18C.%20Applying_Group_Policy_Changes_Immediately_via_CMD.png)
  *Running gpupdate /force to immediately apply the updated password policy*

  ---

  ### Creating & linking a GPO to the HR OU
  *Implementing targeted restrictions for specific departments to comply with local security standards.*

  ![GPMC HR OU Create and Link GPO Menu](./Phase3.Create_Apply_GPOs/19C.%20GPMC_HR_OU_Create_and_Link_GPO_Menu.png)
  *Right-clicking the HR OU in GPMC to create and link a new GPO*

  ![GPMC Creating Targeted GPO for HR OU](./Phase3.Create_Apply_GPOs/20C.%20GPMC_Creating_Targeted_GPO_for_HR_OU.png)
  *Naming and creating the new GPO targeted specifically at the HR OU*

  ![GPMC Opening HR Specific Policy Editor](./Phase3.Create_Apply_GPOs/21C.%20GPMC_Opening_HR_Specific_Policy_Editor.png)
  *Opening the HR GPO in the Group Policy Management Editor*

  ![GPM Editor Selecting Computer Configuration HR](./Phase3.Create_Apply_GPOs/22C.%20GPM_Editor_Selecting_Computer_Configuration_Node_for_HR_GPO.png)
  *Navigating to Computer Configuration within the HR GPO*

  ![GPM Editor Expanding HR Computer Policies](./Phase3.Create_Apply_GPOs/23C.%20GPM_Editor_Expanding_HR_Computer_Policies.png)
  *Expanding the Computer Policies node in the HR GPO*

  ![GPM Editor Selecting Administrative Templates](./Phase3.Create_Apply_GPOs/24C.%20GPM_Editor_Selecting_Administrative_Templates_Node.png)
  *Selecting Administrative Templates to access desktop and system restriction settings*

  ![GPM Editor Selecting Control Panel Policy Node](./Phase3.Create_Apply_GPOs/25C.%20GPM_Editor_Selecting_Control_Panel_Policy_Node.png)
  *Navigating to the Control Panel policy node*

  ![GPM Editor Selecting Personalization Policy Node](./Phase3.Create_Apply_GPOs/26C.%20GPM_Editor_Selecting_Personalization_Policy_Node.png)
  *Selecting the Personalization policy node for screensaver and desktop settings*

  ![GPM Editor Reviewing Machine Personalization](./Phase3.Create_Apply_GPOs/27C.%20GPM_Editor_Reviewing_Machine_Personalization_Options.png)
  *Reviewing available personalization policy settings*

  ![GPM Editor User Configuration Personalization](./Phase3.Create_Apply_GPOs/28C.%20GPM_Editor_User_Configuration_Personalization_Policies.png)
  *Switching to User Configuration for personalization policy settings*

  ![GPM Editor Enabling Desktop Screen Saver Policy](./Phase3.Create_Apply_GPOs/29C.%20GPM_Editor_Enabling_Desktop_Screen_Saver_Policy.png)
  *Enabling the screen saver policy for HR OU workstations*

  ![GPM Editor Selecting Password Protect Screen](./Phase3.Create_Apply_GPOs/30C.%20GPM_Editor_Selecting_Password_Protect_Screen_Saver_Policy.png)
  *Enabling password protection on the screen saver — workstation locks after timeout*

  ![GPO HR Desktop Lockout Enable Password Protection](./Phase3.Create_Apply_GPOs/31C.%20GPO_HR_Desktop_Lockout_Enable_Password_Protection.png)
  *Confirming password-protected screen saver lockout is enabled for HR users*

  ![GPM Editor Selecting Screen Saver Timeout Policy](./Phase3.Create_Apply_GPOs/32C.%20GPM_Editor_Selecting_Screen_Saver_Timeout_Policy.png)
  *Opening the screen saver timeout policy setting*

  ![GPO HR Desktop Lockout Set 300 Second Timeout](./Phase3.Create_Apply_GPOs/33C.%20GPO_HR_Desktop_Lockout_Set_300_Second_Timeout.png)
  *Setting screen saver timeout to 300 seconds (5 minutes) for HR workstations*

  ---

  ### Final verification
  *Verifying the GPO hierarchy and inheritance within the management console.*

  ![GPMC HR OU Linked GPOs Verification](./Phase3.Create_Apply_GPOs/34C.%20GPMC_HR_OU_Linked_GPOs_Verification.png)
  *GPMC showing both GPOs correctly linked — Password Policy at domain level, Desktop Lockout at HR OU level*

</details>

---

## Phase 4 — Domain Join & Initial User Logon

**Goal:** Join the Windows 11 workstation to the `technocrat.local` domain and verify that domain policies and profiles apply correctly.

- [x] Set client DNS to point to `Technocrat-DC01` (`192.168.1.10`)
- [x] Join `Workstation.1` to the `technocrat.local` domain
- [x] Test domain user authentication and profile creation
- [x] Verify GPO application with `gpresult /r` and `gpupdate /force`

<details>
  <summary><b>Click to expand: Phase 4 - Joining the Windows 11 Client to the Domain (30 Screenshots)</b></summary>

  ### Preparing the client for domain join
  *Configuring the workstation's network stack to communicate with the Technocrat Domain Controller.*

  ![Initial Local Logon Prompt](./Phase4.Join_Client_To_DC/01D.Initial_Local_Logon_Prompt.png)
  *Logging into the Windows 11 workstation locally as Workstation.1 before domain join*

  ![Navigate Settings DNS Config](./Phase4.Join_Client_To_DC/02D.Navigate_Settings_DNS_Config.png)
  *Opening network settings on the client to configure DNS*

  ![Network Internet Settings](./Phase4.Join_Client_To_DC/03D.Network_Internet_Settings.png)
  *Network and Internet settings panel on Windows 11*

  ![Edit DNS Settings Manual Selection](./Phase4.Join_Client_To_DC/04D.Edit_DNS_Settings_Manual_Selection.png)
  *Switching DNS configuration to manual entry*

  ![Enable IPv4 DNS Manual Config](./Phase4.Join_Client_To_DC/05D.Enable_IPv4_DNS_Manual_Config.png)
  *Enabling manual IPv4 DNS configuration*

  ![Set Preferred DNS to DC IP](./Phase4.Join_Client_To_DC/06D.Set_Preferred_DNS_to_DC_IP.png)
  *Setting the preferred DNS server to 192.168.1.10 — pointing the client at Technocrat-DC01*

  ![Verify Client DNS Assignment](./Phase4.Join_Client_To_DC/07D.Verify_Client_DNS_Assignment.png)
  *Confirming the DNS setting is saved correctly on the client*

  ---

  ### Troubleshooting connectivity
  *Identifying and resolving common DNS resolution and IP subnetting issues.*

  ![Quick Link Menu Select System](./Phase4.Join_Client_To_DC/08D.Quick_Link_Menu_Select_System.png)
  *Opening System properties via the Quick Link menu*

  ![System About Select Advanced Settings](./Phase4.Join_Client_To_DC/09D.System_About_Select_Advanced_Settings.png)
  *Navigating to Advanced System Settings*

  ![System Properties Computer Name Tab](./Phase4.Join_Client_To_DC/10D.System_Properties_Computer_Name_Tab.png)
  *System Properties Computer Name tab — starting point for domain join*

  ![Computer Name Changes Dialog](./Phase4.Join_Client_To_DC/11D.Computer_Name_Changes_Dialog.png)
  *Computer Name/Domain Changes dialog box*

  ![Rename Client Computer Field](./Phase4.Join_Client_To_DC/12D.Rename_Client_Computer_Field.png)
  *Entering the computer name for the workstation*

  ![Set Computer Name and Domain Field](./Phase4.Join_Client_To_DC/13D.Set_Computer_Name_And_Domain_Field.png)
  *Entering technocrat.local in the domain field to initiate the domain join*

  ![Domain Controller Unreachable DNS Error](./Phase4.Join_Client_To_DC/14D.Domain_Controller_Unreachable_DNS_Error.png)
  *Domain controller unreachable error — DNS resolution troubleshooting begins*

  ![Manual IPv4 Static IP Assignment](./Phase4.Join_Client_To_DC/15D.Manual_IPv4_Static_IP_Assignment.png)
  *Assigning a static IP to the client to ensure it is on the same subnet as the DC*

  ![Ping DC IP Connectivity Success](./Phase4.Join_Client_To_DC/16D.Ping_DC_IP_Connectivity_Success.png)
  *Pinging the DC at 192.168.1.10 — successful reply confirms network connectivity*

  ![NSLookup Verify Domain DNS Resolution](./Phase4.Join_Client_To_DC/17D.NSLookup_Verify_Domain_DNS_Resolution.png)
  *Running NSLookup to verify the client can resolve technocrat.local via the DC's DNS*

  ![Retry Domain Join After DNS Fix](./Phase4.Join_Client_To_DC/18D.Retry_Domain_Join_After_DNS_Fix.png)
  *Retrying the domain join after resolving the DNS connectivity issue*

  ---

  ### Joining the domain
  *Authorizing the workstation's membership within the Technocrat Active Directory forest.*

  ![Domain Admin Credentials Join Prompt](./Phase4.Join_Client_To_DC/19D.Domain_Admin_Credentials_Join_Prompt.png)
  *Entering domain administrator credentials to authorize the domain join*

  ![Confirm Domain Admin Credentials](./Phase4.Join_Client_To_DC/20D.Confirm_Domain_Admin_Credentials.png)
  *Confirming the administrator credentials for technocrat.local*

  ![Welcome to Domain Success Message](./Phase4.Join_Client_To_DC/21D.Welcome_To_Domain_Success_Message.png)
  *"Welcome to the technocrat.local domain" — domain join successful*

  ![Restart Now Post Domain Join](./Phase4.Join_Client_To_DC/22D.Restart_Now_Post_Domain_Join.png)
  *Restarting the workstation to complete the domain join process*

  ---

  ### First domain user logon
  *Testing the integration by logging in with a managed user account.*

  ![Select Other User For Domain Logon](./Phase4.Join_Client_To_DC/23D.Select_Other_User_For_Domain_Logon.png)
  *Selecting "Other user" on the login screen to log in as a domain account*

  ![Domain User Abel Schu Initial Logon](./Phase4.Join_Client_To_DC/24D.Domain_User_Abel_Schu_Initial_Logon.png)
  *Logging in as domain user Abel Schuehlein for the first time*

  ![First Logon Password Change Requirement](./Phase4.Join_Client_To_DC/25D.First_Logon_Password_Change_Requirement.png)
  *Password change prompt on first logon — confirming policy enforcement*

  ![Hyper-V Enhanced Session RDP Restriction Error](./Phase4.Join_Client_To_DC/26D.Hyper-V_Enhanced_Session_RDP_Restriction_Error.png)
  *Enhanced Session RDP restriction error encountered during setup*

  ![Enter Domain User Credentials TECHNOCRAT](./Phase4.Join_Client_To_DC/27D.Enter_Domain_User_Credentials_TECHNOCRAT.png)
  *Entering TECHNOCRAT domain credentials at the login prompt*

  ![Password Change Confirmation Success](./Phase4.Join_Client_To_DC/28D.Password_Change_Confirmation_Success.png)
  *Password successfully changed — user proceeds to the desktop*

  ![Verify Domain User Profile Start Menu](./Phase4.Join_Client_To_DC/29D.Verify_Domain_User_Profile_Start_Menu.png)
  *Domain user profile successfully created — Start Menu confirming Abel is logged in*

  ---

  ### Post-logon verification
  *Confirming the security context and identity of the active session.*

  ![Whoami All Verify Domain User Context](./Phase4.Join_Client_To_DC/30D.Whoami_All_Verify_Domain_User_Context.png)
  *Running whoami /all in Command Prompt — confirming domain user context and SIDs*

</details>

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

<details>
  <summary><b>Click to expand: Phase 5 - DNS, DHCP & Shared Folders (65 Screenshots)</b></summary>

  ### DNS configuration
  *Configuring the domain's DNS infrastructure and adding host records to the forward lookup zone.*

  ![Server Manager Tools Select DNS](./Phase5.DNS_DHCP_Shared_Folders/01E.Server_Manager_Tools_Select_DNS.png)
  *Opening DNS Manager from the Server Manager Tools menu*

  ![DNS Manager Server Node Overview](./Phase5.DNS_DHCP_Shared_Folders/02E.DNS_Manager_Server_Node_Overview.png)
  *DNS Manager opening view showing Technocrat-DC01 as the DNS server*

  ![Expand DC01 DNS Server Nodes](./Phase5.DNS_DHCP_Shared_Folders/03E.Expand_DC01_DNS_Server_Nodes.png)
  *Expanding the DC01 node to view Forward and Reverse Lookup Zones*

  ![Forward Lookup Zones Directory Structure](./Phase5.DNS_DHCP_Shared_Folders/04E.Forward_Lookup_Zones_Directory_Structure.png)
  *Forward Lookup Zones showing the technocrat.local zone already created during DC promotion*

  ![Right Click Domain Select New Host](./Phase5.DNS_DHCP_Shared_Folders/05E.Right_Click_Domain_Select_New_Host.png)
  *Right-clicking the technocrat.local zone to add a new A record*

  ![Create New Host Record Fileserver A Record](./Phase5.DNS_DHCP_Shared_Folders/06E.Create_New_Host_Record_Fileserver_A_Record.png)
  *Creating a new A record for fileserver.technocrat.local*

  ![Fileserver Host A Record Success Confirmation](./Phase5.DNS_DHCP_Shared_Folders/07E.Fileserver_Host_A_Record_Success_Confirmation.png)
  *Confirmation that the fileserver A record was created successfully*

  ![Verify Fileserver A Record In DNS Manager](./Phase5.DNS_DHCP_Shared_Folders/08E.Verify_Fileserver_A_Record_In_DNS_Manager.png)
  *DNS Manager showing the new fileserver.technocrat.local A record in the zone*

  ---

  ### Installing DHCP
  *Adding the DHCP Server role and authorizing it within the Active Directory domain.*

  ![Server Manager Select Add Roles Features](./Phase5.DNS_DHCP_Shared_Folders/09E.Server_Manager_Select_Add_Roles_Features.png)
  *Launching Add Roles and Features to install the DHCP Server role*

  ![Select Role Based Or Feature Based Installation](./Phase5.DNS_DHCP_Shared_Folders/10E.Select_Role_Based_Or_Feature_Based_Installation.png)
  *Selecting Role-based installation in the Add Roles wizard*

  ![Select DC01 From Destination Server Pool](./Phase5.DNS_DHCP_Shared_Folders/11E.Select_DC01_From_Destination_Server_Pool.png)
  *Selecting Technocrat-DC01 as the destination server*

  ![Select DHCP Server Role Installation](./Phase5.DNS_DHCP_Shared_Folders/12E.Select_DHCP_Server_Role_Installation.png)
  *Checking the DHCP Server role from the server roles list*

  ![Add Required DHCP Management Tools](./Phase5.DNS_DHCP_Shared_Folders/13E.Add_Required_DHCP_Management_Tools.png)
  *Confirming required DHCP management tools to be installed alongside the role*

  ![Select Features RSAT DHCP Tools](./Phase5.DNS_DHCP_Shared_Folders/14E.Select_Features_RSAT_DHCP_Tools.png)
  *Selecting RSAT DHCP Tools for remote management capability*

  ![Confirm DHCP Server Installation Selections](./Phase5.DNS_DHCP_Shared_Folders/15E.Confirm_DHCP_Server_Installation_Selections.png)
  *Confirmation screen before installing the DHCP Server role*

  ![DHCP Server Installation Progress DC01](./Phase5.DNS_DHCP_Shared_Folders/16E.DHCP_Server_Installation_Progress_DC01.png)
  *DHCP Server role installation in progress*

  ![Complete DHCP Post Deployment Configuration](./Phase5.DNS_DHCP_Shared_Folders/17E.Complete_DHCP_Post_Deployment_Configuration.png)
  *Launching the DHCP post-install configuration wizard from the yellow flag notification*

  ![DHCP Server Post Install Authorization Wizard](./Phase5.DNS_DHCP_Shared_Folders/18E.DHCP_Server_Post_Install_Authorization_Wizard.png)
  *DHCP post-install authorization wizard — confirming the server will be authorized in AD*

  ![DHCP AD DS Authorization Credentials](./Phase5.DNS_DHCP_Shared_Folders/19E.DHCP_AD_DS_Authorization_Credentials.png)
  *Authorizing the DHCP server in Active Directory using domain admin credentials*

  ![DHCP Post Install Configuration Summary Success](./Phase5.DNS_DHCP_Shared_Folders/20E.DHCP_Post_Install_Configuration_Summary_Success.png)
  *Post-install configuration complete — DHCP server is active and authorized*

  ---

  ### Configuring DHCP scope
  *Creating and activating an IP address scope to automatically assign addresses to domain clients.*

  ![Search And Launch DHCP Management Console](./Phase5.DNS_DHCP_Shared_Folders/21E.Search_And_Launch_DHCP_Management_Console.png)
  *Opening the DHCP Management Console*

  ![DHCP Management Console DC01 Node](./Phase5.DNS_DHCP_Shared_Folders/22E.DHCP_Management_Console_DC01_Node.png)
  *DHCP console showing DC01 as the authorized DHCP server*

  ![Expand IPv4 Scope Management](./Phase5.DNS_DHCP_Shared_Folders/23E.Expand_IPv4_Scope_Management.png)
  *Expanding the IPv4 node to begin scope creation*

  ![Right Click IPv4 Select New Scope](./Phase5.DNS_DHCP_Shared_Folders/24E.Right_Click_IPv4_Select_New_Scope.png)
  *Right-clicking IPv4 to launch the New Scope Wizard*

  ![Welcome To New Scope Wizard](./Phase5.DNS_DHCP_Shared_Folders/25E.Welcome_To_New_Scope_Wizard.png)
  *New Scope Wizard welcome screen*

  ![Set DHCP Scope Name Technocrat Clients](./Phase5.DNS_DHCP_Shared_Folders/26E.Set_DHCP_Scope_Name_Technocrat_Clients.png)
  *Naming the DHCP scope "Technocrat_Clients"*

  ![Configure DHCP Scope IP Address Range](./Phase5.DNS_DHCP_Shared_Folders/27E.Configure_DHCP_Scope_IP_Address_Range.png)
  *Setting the IP address range for the DHCP scope*

  ![Skip DHCP Exclusions And Delay](./Phase5.DNS_DHCP_Shared_Folders/28E.Skip_DHCP_Exclusions_And_Delay.png)
  *Skipping exclusions — no IPs need to be reserved from the scope at this stage*

  ![Set DHCP Scope Lease Duration 8 Days](./Phase5.DNS_DHCP_Shared_Folders/29E.Set_DHCP_Scope_Lease_Duration_8_Days.png)
  *Setting the DHCP lease duration to 8 days*

  ![Select Configure DHCP Options Now](./Phase5.DNS_DHCP_Shared_Folders/30E.Select_Configure_DHCP_Options_Now.png)
  *Choosing to configure DHCP options immediately after scope creation*

  ![Domain Name And DNS Servers Config](./Phase5.DNS_DHCP_Shared_Folders/31E.Domain_Name_And_DNS_Servers_Config.png)
  *Setting technocrat.local as the domain name and 192.168.1.10 as the DNS server for DHCP clients*

  ![Skip Default Gateway Router Address Config](./Phase5.DNS_DHCP_Shared_Folders/32E.Skip_Default_Gateway_Router_Address_Config.png)
  *Skipping default gateway — not required for this lab scope*

  ![Activate New DHCP Scope Now](./Phase5.DNS_DHCP_Shared_Folders/33E.Activate_New_DHCP_Scope_Now.png)
  *Activating the new DHCP scope immediately*

  ![Complete New Scope Wizard Finish](./Phase5.DNS_DHCP_Shared_Folders/34E.Complete_New_Scope_Wizard_Finish.png)
  *New Scope Wizard complete — Technocrat_Clients scope is now active*

  ---

  ### Creating shared folders
  *Setting up a centralized file share on the domain controller for departmental access.*

  ![Create New Folder Shares On C Drive](./Phase5.DNS_DHCP_Shared_Folders/35E.Create_New_Folder_Shares_On_C_Drive.png)
  *Creating a new Shares folder on the C: drive to host shared resources*

  ![Create New Folder Shares On C Drive](./Phase5.DNS_DHCP_Shared_Folders/36E.Create_New_Folder_Shares_On_C_Drive.png)
  *Shares folder created at C:\Shares on Technocrat-DC01*

  ![Create Sales Subfolder Within Shares](./Phase5.DNS_DHCP_Shared_Folders/37E.Create_Sales_Subfolder_Within_Shares.png)
  *Creating a Sales subfolder inside C:\Shares for the Sales department*

  ![Sales Folder Properties Menu Select](./Phase5.DNS_DHCP_Shared_Folders/38E.Sales_Folder_Properties_Menu_Select.png)
  *Opening the Sales folder properties to configure sharing*

  ![Select Advanced Sharing Options](./Phase5.DNS_DHCP_Shared_Folders/39E.Select_Advanced_Sharing_Options.png)
  *Selecting Advanced Sharing to configure share permissions manually*

  ![Enable Advanced Sharing Shares Folder](./Phase5.DNS_DHCP_Shared_Folders/40E.Enable_Advanced_Sharing_Shares_Folder.png)
  *Enabling Advanced Sharing on the Sales folder*

  ---

  ### Configuring share & NTFS permissions
  *Applying layered Share and NTFS permissions to enforce least-privilege access control.*

  ![Add Users Or Groups To Share Permissions](./Phase5.DNS_DHCP_Shared_Folders/41E.Add_Users_Or_Groups_To_Share_Permissions.png)
  *Adding users and groups to the Share permissions of the Sales folder*

  ![Add Users Or Groups To Share Permissions](./Phase5.DNS_DHCP_Shared_Folders/42E.Add_Users_Or_Groups_To_Share_Permissions.png)
  *Selecting specific security groups to grant share access*

  ![Assign Sales-Staff Group To Share Permissions](./Phase5.DNS_DHCP_Shared_Folders/43E.Assign_Sales-Staff_Group_To_Share_Permissions.png)
  *Assigning the Sales-Staff security group to the share permissions*

  ![Grant Change And Read Share Permissions To Sales-Staff](./Phase5.DNS_DHCP_Shared_Folders/44E.Grant_Change_And_Read_Share_Permissions_To_Sales-Staff.png)
  *Granting Change and Read share permissions to Sales-Staff*

  ![Shares Security Tab Edit NTFS Permissions](./Phase5.DNS_DHCP_Shared_Folders/45E.Shares_Security_Tab_Edit_NTFS_Permissions.png)
  *Switching to the Security tab to configure NTFS permissions separately from share permissions*

  ![Advanced Security Settings Remove Users NTFS Permissions](./Phase5.DNS_DHCP_Shared_Folders/46E.Advanced_Security_Settings_Remove_Users_NTFS_Permissions.png)
  *Opening Advanced Security Settings — removing the default Users group to enforce least privilege*

  ![Convert Inherited Permissions To Explicit](./Phase5.DNS_DHCP_Shared_Folders/47E.Convert_Inherited_Permissions_To_Explicit.png)
  *Converting inherited permissions to explicit entries before making changes*

  ![Remove Standard Users From NTFS Access List](./Phase5.DNS_DHCP_Shared_Folders/48E.Remove_Standard_Users_From_NTFS_Access_List.png)
  *Removing default Users group from NTFS access list to enforce least privilege*

  ![Advanced Security Settings Verify Clean NTFS Access List](./Phase5.DNS_DHCP_Shared_Folders/49E.Advanced_Security_Settings_Verify_Clean_NTFS_Access_List.png)
  *Verifying the cleaned NTFS access list before adding the correct groups*

  ![Select Principal For NTFS Permission Entry](./Phase5.DNS_DHCP_Shared_Folders/50E.Select_Principal_For_NTFS_Permission_Entry.png)
  *Selecting the security principal to assign NTFS permissions to*

  ![Assign Sales-Staff As NTFS Principal](./Phase5.DNS_DHCP_Shared_Folders/51E.Assign_Sales-Staff_As_NTFS_Principal.png)
  *Assigning the Sales-Staff group as the NTFS permission principal*

  ![Configure NTFS Read And Execute Permissions For Sales-Staff](./Phase5.DNS_DHCP_Shared_Folders/52E.Configure_NTFS_Read_And_Execute_Permissions_For_Sales-Staff.png)
  *Configuring Read and Execute NTFS permissions for the Sales-Staff group*

  ---

  ### Drive mapping via GPO
  *Automating network drive assignment for the Sales department using Group Policy Preferences.*

  ![Create And Link GPO To Sales OU](./Phase5.DNS_DHCP_Shared_Folders/53E.Create_And_Link_GPO_To_Sales_OU.png)
  *Creating and linking a new GPO to the Sales OU for drive mapping*

  ![New GPO Creation MappedDrive-Sales-S](./Phase5.DNS_DHCP_Shared_Folders/54E.New_GPO_Creation_MappedDrive-Sales-S.png)
  *Naming the new GPO for the Sales drive mapping*

  ![Group Policy Management Edit MappedDrive-Sales-S](./Phase5.DNS_DHCP_Shared_Folders/55E.Group_Policy_Management_Edit_MappedDrive-Sales-S.png)
  *Opening the Sales drive mapping GPO in the Group Policy Management Editor*

  ![GPMC Editor User Configuration Node](./Phase5.DNS_DHCP_Shared_Folders/56E.GPMC_Editor_User_Configuration_Node.png)
  *Navigating to User Configuration in the GPO editor*

  ![GPMC Editor User Preferences Selection](./Phase5.DNS_DHCP_Shared_Folders/57E.GPMC_Editor_User_Preferences_Selection.png)
  *Expanding Preferences under User Configuration*

  ![GPMC Editor User Preferences Windows Settings](./Phase5.DNS_DHCP_Shared_Folders/58E.GPMC_Editor_User_Preferences_Windows_Settings.png)
  *Navigating into Windows Settings under User Preferences*

  ![GPMC Editor User Preferences Drive Maps Selection](./Phase5.DNS_DHCP_Shared_Folders/59E.GPMC_Editor_User_Preferences_Drive_Maps_Selection.png)
  *Selecting Drive Maps to create a new mapped drive entry*

  ![Right Click Drive Maps Select New Mapped Drive](./Phase5.DNS_DHCP_Shared_Folders/60E.Right_Click_Drive_Maps_Select_New_Mapped_Drive.png)
  *Right-clicking to create a new mapped drive in the GPO*

  ![Configure GPO Drive Map S Path And Label](./Phase5.DNS_DHCP_Shared_Folders/61E.Configure_GPO_Drive_Map_S_Path_And_Label.png)
  *Configuring the mapped drive — UNC path to \\Technocrat-DC01\Sales and drive letter S:*

  ![GPO Common Tab Run In Logged-On Security Context](./Phase5.DNS_DHCP_Shared_Folders/62E.GPO_Common_Tab_Run_In_Logged-On_Security_Context.png)
  *Setting the GPO Common tab to run in the logged-on user's security context*

  ---

  ### Verification
  *Confirming the drive mapping and folder access work correctly for a Sales department user.*

  ![Logon To Sales-Staff Account M-Lewis](./Phase5.DNS_DHCP_Shared_Folders/63E.Logon_To_Sales-Staff_Account_M-Lewis.png)
  *Logging onto the workstation as Sales user M. Lewis to test the drive mapping*

  ![Verify S Drive Mapped Via Group Policy](./Phase5.DNS_DHCP_Shared_Folders/64E.Verify_S_Drive_Mapped_Via_Group_Policy.png)
  *S: drive successfully mapped via Group Policy on the Sales user's desktop*

  ![Verify Sales Folder Access Within S Drive](./Phase5.DNS_DHCP_Shared_Folders/65E.Verify_Sales_Folder_Access_Within_S_Drive.png)
  *Confirming Sales-Staff can access and browse the S: drive contents as expected*

</details>

---

## Phase 6 — Help Desk Simulation Scenarios

**Goal:** Simulate the most common Active Directory tasks a help desk technician handles on a daily basis.

- [x] Unlock a locked-out user account
- [x] Perform a password reset with "must change at next logon" enforced
- [x] Disable a user account and move it to the `Disabled Accounts` OU
- [x] Add a user to a security group
- [x] Resolve a DNS resolution issue
- [x] Use PowerShell to query last logon date: `Get-ADUser -Identity <user> -Properties LastLogonDate`

<details>
  <summary><b>Click to expand: Phase 6 - Help Desk Simulation Scenarios (15 Screenshots)</b></summary>

  ### Finding and unlocking a user account
  *Simulating a real help desk ticket — locating and unlocking a locked-out domain user account.*

  ![ADUC Search Icon Select](./Phase6.Help_Desk_Scenarios/01F.ADUC_Search_Icon_Select.png)
  *Using the ADUC search function to locate a user account quickly*

  ![ADUC Find Users Contacts Groups Dialog](./Phase6.Help_Desk_Scenarios/02F.ADUC_Find_Users_Contacts_Groups_Dialog.png)
  *Find Users, Contacts and Groups dialog — searching across the entire domain*

  ![ADUC Search Result Emily Scott](./Phase6.Help_Desk_Scenarios/03F.ADUC_Search_Result_Emily_Scott.png)
  *Search results returning Emily Scott's account*

  ![Unlock Emily Scott Account Properties](./Phase6.Help_Desk_Scenarios/04F.Unlock_Emily_Scott_Account_Properties.png)
  *Opening Emily Scott's account properties — Account tab showing the account is locked out*

  ---

  ### Password reset
  *Resetting a user's password and enforcing a change on next logon — a daily help desk task.*

  ![Right Click User Select Reset Password](./Phase6.Help_Desk_Scenarios/05F.Right_Click_User_Select_Reset_Password.png)
  *Right-clicking a user account to select Reset Password*

  ![Enter New Password Enforce Change At Logon](./Phase6.Help_Desk_Scenarios/06F.Enter_New_Password_Enforce_Change_At_Logon.png)
  *Setting a temporary password with "User must change password at next logon" enforced*

  ---

  ### Disabling an account
  *Disabling a departed user's account and moving it to the Disabled Accounts OU for clean AD hygiene.*

  ![Right Click Karen Disable Account](./Phase6.Help_Desk_Scenarios/07F.Right_Click_Karen_Disable_Account.png)
  *Right-clicking Karen's account and selecting Disable Account*

  ![Move Disabled Account To Disabled Accounts OU](./Phase6.Help_Desk_Scenarios/08F.Move_Disabled_Account_To_Disabled_Accounts_OU.png)
  *Moving the disabled account into the Disabled Accounts OU for clean AD organization*

  ---

  ### Adding a user to a group
  *Granting a user access to a resource by adding them to the appropriate security group.*

  ![VPN Users Group Properties Members Tab](./Phase6.Help_Desk_Scenarios/09F.VPN_Users_Group_Properties_Members_Tab.png)
  *Opening the GS-VPN-Users group properties to view and manage members*

  ![Add Brian Hall To VPN Users Group](./Phase6.Help_Desk_Scenarios/10F.Add_Brian_Hall_To_VPN_Users_Group.png)
  *Adding Brian Hall to the GS-VPN-Users security group*

  ![Confirm Brian Hall Membership VPN Users](./Phase6.Help_Desk_Scenarios/11F.Confirm_Brian_Hall_Membership_VPN_Users.png)
  *Members tab confirming Brian Hall is now a member of GS-VPN-Users*

  ---

  ### Advanced features & PowerShell query
  *Using the Attribute Editor and PowerShell to audit user account activity and last logon data.*

  ![ADUC View Menu Enable Advanced Features](./Phase6.Help_Desk_Scenarios/12F.ADUC_View_Menu_Enable_Advanced_Features.png)
  *Enabling Advanced Features in the ADUC View menu to expose additional account attributes*

  ![Right Click Abel Schu Select Properties](./Phase6.Help_Desk_Scenarios/13F.Right_Click_Abel_Schu_Select_Properties.png)
  *Opening Abel Scheheelein's full properties with Advanced Features enabled*

  ![Abel Schu Properties Attribute Editor LastLogon](./Phase6.Help_Desk_Scenarios/14F.Abel_Schu_Properties_Attribute_Editor_LastLogon.png)
  *Attribute Editor tab showing Abel's LastLogon timestamp — useful for identifying inactive accounts*

  ![PowerShell Get-ADUser Abel Schu LastLogonDate](./Phase6.Help_Desk_Scenarios/15F.PowerShell_Get-ADUser_Abel_Schu_LastLogonDate.png)
  *Running Get-ADUser -Identity Abel_Schu -Properties LastLogonDate in PowerShell — confirming the same data via command line*

</details>

---

## Phase 50 (Z) — PowerShell Automation Script

**Goal:** The Grand Finale — a PowerShell script that automates bulk user creation from a CSV file, demonstrating real-world scripting and AD automation skills.

- [x] Prepare a CSV file containing 50 randomly generated user first and last names
- [x] Store the CSV in a dedicated lab folder for clean organization
- [x] Launch PowerShell ISE as Administrator
- [x] Write and execute a script to loop through the CSV and create all 50 users in ADUC under the Lab-Users OU
- [x] Verify all 50 accounts appear correctly in Active Directory Users and Computers

<details>
  <summary><b>Click to expand: Phase 50 (Z) - PowerShell Bulk User Import Automation (6 Screenshots)</b></summary>

  ### Building and running the bulk user import script
  *Automating the creation of 50 Active Directory user accounts using a PowerShell script and CSV data source.*

  ![User Data Source CSV File Preparation](./Z_50_Users_Script_CSV/01Z.User_Data_Source_CSV_File_Preparation.png)
  *The CSV file containing 50 randomly generated user first and last names — the data source for the bulk import script*

  ![Initializing Users50 CSV Import File](./Z_50_Users_Script_CSV/02Z.Initializing_Users50_CSV_Import_File.png)
  *Initializing the CSV file in preparation for the PowerShell import script*

  ![Storing Users50 CSV in Lab Root Folder](./Z_50_Users_Script_CSV/03Z.Storing_Users50_CSV_in_Lab_Root_Folder.png)
  *Storing the CSV file in a dedicated lab root folder for clean file organization*

  ![Launching PowerShell ISE as Administrator](./Z_50_Users_Script_CSV/04Z.Launching_PowerShell_ISE_as_Administrator.png)
  *Launching PowerShell ISE as Administrator — required to execute AD cmdlets*

  ![PowerShell Script Bulk Importing Users from CSV](./Z_50_Users_Script_CSV/05Z.PowerShell_Script_Bulk_Importing_Users_from_CSV.png)
  *The PowerShell script running — looping through the CSV and creating each user account in ADUC under the Lab-Users OU*

  ![ADUC Bulk User Import Verification Lab-Users OU](./Z_50_Users_Script_CSV/06Z.ADUC_Bulk_User_Import_Verification_Lab-Users_OU.png)
  *ADUC confirming all 50 users successfully imported into the Lab-Users OU — script executed without errors*

</details>
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
This has been an Abel Schuehlein x Technocrat Lab Production. Feel free to follow this lab along and/or reach out with any critiques!
