---
title: "Audit After Scan"
layout: single-portfolio
excerpt: <img width="1000" height="750" alt="image" src="https://github.com/user-attachments/assets/dcbf2247-10ba-48cc-a4d3-f0c846e6934c" />
collection: research
order_number: 30
header: 
  og_image: "research/Agreement-Strength.png"
---

The AUDIT AFTER SCAN phase of this project explains how the result was examined after scanning; I audited the scanned results by carefully looking into critical areas of the selected study's OSs, and while finding True Positives, False Positives, False Negatives and True Negatives. During this search, I discovered none of the predicted outcomes, as areas not scanned in the Level 1 security guide profile were extensively covered when the Level 2 security guide was used. The result was extensively broken into considerable parts and further broken down into seven (7) major groups for easy and collective analysis.

#### Security Domain Assessment Results for Ubuntu 20.04

| Security Domain | Before | After | Primary Improvements | Fail Rule |
|-----------------|--------|-------|---------------------|-----------|
| **Initial Setup** | Low | High | Bootload security, system hardening, EOL Service configuration, unnecessary services removal | **31-34** |
| **Services** | Very Low | Moderate | | **14-19** |
| **Network** | Low | Very High | Network parameters, kernel hardening | **10-25** |
| **Host-Based Firewall** | Moderate | Very High | Firewall rules, traffic filtering | **25-28** |
| **Access Control** | Very Low | High | PAM configuration, user management | **26-31** |
| **Logging & Auditing** | Very Low | Moderate | Log configuration, audit rules | **4-7** |
| **System Maintenance** | Very Low | Low | File permissions, maintenance tasks | **1-3** |


#### Security Domain Percentage Assessment for Windows

| Security Domain | Before % | After % | Primary Improvements |
|-----------------|----------|---------|---------------------|
| **Account Policies** | 40 | 52 | Password history, min age, length and complexity |
| **Local Policy** | 60 | 68 | Service configuration, unnecessary services removed |
| **Advance Audit Policy** | 33 | 48 | Acct logon, Mgt, detailed tracking, privilege use |
| **Windows Defender** | 0 | 22 | |
| **Firewall** | 0 | 22 | Domain, and public profile |


To see research data artfacts (results PDFs) click the buttons below where L1 = Level 1 and L2 = Level 2:

**Scan Summary for Ubuntu**

[Before Hardening Scan Summary](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/Enhanced%20Security%20Scan%20Summary%20Report.pdf){: .btn--research}
[After Hardening Scan Summary](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/Remediation%20Scans%20Summary.pdf){: .btn--research}

**Scan Summary for Windows_10_Ent**

[Before Hardening Scan Summary](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/Before%20Hardening%20Scan%20Report.pdf){: .btn--research} 
[After Hardening Scan Summary](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/After%20Hardening.pdf){: .btn--research}

**CIS-CAT Results for Ubuntu 20.04 Level 1 and 2 before hardening**

[Ubuntu L1 Scan Before Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/C1%20Benchmark%20Result%20xccdf_org.cisecurity.benchmarks_testresult_3.0.0_CIS_Ubuntu_Linux_20.04_LTS_Benchmark.pdf){: .btn--research}
[Ubuntu L2 Scan Before Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/C2%20Benchmark%20Result%20xccdf_org.cisecurity.benchmarks_testresult_3.0.0_CIS_Ubuntu_Linux_20.04_LTS_Benchmark.pdf){: .btn--research}

**Results for Ubuntu 20.04 Level 1 and 2 after hardening**

[Ubuntu L1 After Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/tree/master/files/pdf){: .btn--research}
[Ubuntu L2 After Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/cis-cat%20level%202%20after%20hardening.pdf){: .btn--research}

**Result for Lynis Deep Scan on Ubuntu 20.04**
[Lynis After Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/lynis_full_output_20250720_114954.txt.pdf){: .btn--research}

**OpenSCAP Results for Ubuntu 20.04 Level 1 and 2 Before Hardening**

[OpenSCAP L1 Before Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/xccdf_org.open-scap_testresult_xccdf_org.ssgproject.content_profile_cis_level1_workstation%20_%20OpenSCAP%20Evaluation%20Report.pdf){: .btn--research}
[OpenSCAP L2 Before Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/xccdf_org.open-scap_testresult_xccdf_org.ssgproject.content_profile_cis_level2_workstation%20_%20OpenSCAP%20Evaluation%20Report.pdf){: .btn--research}

**OpenSCAP Results for Ubuntu 20.04 Level 1 and 2 After Hardening**

[OpenSCAP L1 After Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/OpenSCAP%20After%20remediation%20level%201.pdf){: .btn--research}
[OpenSCAP L2 After Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/OpenSCAP%20After%20remediation%20level%202.pdf){: .btn--research}

**CIS-CAT Results for WINDOWS_10_ENT Level 1 and 2 before hardening**

[WINDOWS_10 Scan Before Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/Benchmark%20Result%20xccdf_org.cisecurity.benchmarks_testresult_4.0.0_CIS_Microsoft_Windows_10_Enterprise_Benchmark%20(1).pdf){: .btn--research}
[WINDOWS_10 Scan After Hardening](https://github.com/godswill-ikot/godswill-ikot.github.io/blob/master/files/pdf/N_Windows%20CIS-CAT%20SCAN.pdf){: .btn--research}

### Security Assessment Results
**Core Security Controls for Ubuntu 20.04**

| Security Requirement Category | CIS Benchmark Control | Approximate Page Number |
|-------------------------------|----------------------|------------------------|
| **8. Audit Log Files** | 4.1.3 Ensure audit log files are owned by root and mode 0640 or less permissive | **-118-120** |
| **9. Auditing Enabled Boot Kernel Parameter** | 4.1.1.2 Ensure auditd is enabled at boot | **-114-115** |
| **10. Auditd Utility Present** | 4.1.x (Various specific audit rules, too many to list individually here: e.g., 4.1.14 Ensure successful file system mounts are collected, 4.1.6 Ensure events that modify the system administration scope (sudoers) are collected, 4.1.5 Ensure events that modify a user's privileges are collected, 4.1.6 Ensure auditd is configured to rotate logs) | **-124-159 (Section 4.1 Mandatory Access Controls)** |
| **11. Audit Record Backup** | 4.1.4 Ensure logs are shipped to a remote log host | **-121-122** |
| **12. Auto File System Mounting Tools** | 2.2.14 Disable Automounting | **-78-79** |
| **13. IP Forwarding** | 3.2.1 Ensure IP forwarding is disabled | **-97-98** |
| **14. Relay Files Ownership** | 5.1.1 Ensure cron daemon is enabled (General system file permissions) | **-185-191** |
| **15. Mail Relaying** | 2.2.12 Configure Postfix for Local-Only Mail (if Postfix is used) | **-75-76** |
| **16. Post Core Dumps** | 1.5.5 Ensure core dumps are restricted | **-38-49** |
| **17. Password, Shadow, and Group Files** | 6.1.2 Ensure /etc/passwd permissions are configured, 6.1.3 Ensure /etc/shadow permissions are configured, 6.1.4 Ensure /etc/group permissions are configured, 6.1.5 Ensure /etc/gshadow permissions are configured | **-189-195** |
| **18. Aid Service** | 2.2.16 Disable aid | **-90-81** |
| **19. Telnet Client and DHCP Client** | 2.3.1 Disable telnet client, 2.2.17 Disable dhclient (if not needed) | **-90-81, -81-82** |
| **20. Graphical Desktop Environment** | 1.7.3 Ensure GNOME/GDM is configured to lock the screen (if applicable for Desktop) | **-58-60** |
| **21. LDAP Server** | 2.3.2 Remove unnecessary packages: LDAP Server (Specific to stand on Ubuntu) | **-66-67** |
| **22. External Devices** | 1.1.2 Disable USB Storage | **-35-36** |
| **23. Firewalls** | 3.5.1 Ensure ufw is installed and enabled, 3.5.2 Ensure iptv4 is default deny policy | **-107-109** |

### Security Assessment Results 
**Core CIS Microsoft Windows 10 Enterprise Benchmark Control**

| Security Requirement Category | CIS Benchmark Control | Approximate Page Number |
|-------------------------------|----------------------|------------------------|
| **8. Audit Log Files** | 17.1.1 Ensure 'Audit Credential Validation' is set to 'Success and Failure', 17.5.1 Ensure 'Audit Account Lockout' is set to 'Failure' | **210-215** |
| **9. Auditing Enabled Boot** | 17.1.1 Ensure 'Audit Credential Validation' is set to 'Success and Failure', Advanced Audit Policy Configuration | **210-215** |
| **10. Audit Utility Present** | 17.1.1 Ensure 'Audit Credential Validation' is set to 'Success and Failure', 17.3.1 Ensure 'Audit PNP Activity' is set to include 'Success', 17.5.4 Ensure 'Audit User Account Management' is set to 'Success and Failure', 17.6.1 Ensure 'Audit Detailed Directory Service Replication' is set to include 'Failure' | **210-230** |
| **11. Audit Record Backup** | 18.8.21.2 Ensure 'Configure registry policy processing: Do not apply during periodic background processing' is set to 'Enabled: FALSE' | **310-315** |
| **12. Auto File System Mounting** | 18.9.8.1 Ensure 'Turn off Autoplay' is set to 'Enabled: All drives', 18.9.8.2 Ensure 'Set the default behavior for AutoRun' is set to 'Enabled: Do not execute any autorun commands' | **350-355** |
| **13. IP Forwarding** | 18.5.4.1 Ensure 'MSS: (EnableICMPRedirect) Allow ICMP redirects to override OSPF generated routes' is set to 'Disabled', 18.5.8.1 Ensure 'MSS: (NoNameReleaseOnDemand) Allow the computer to ignore NetBIOS name release requests except from WINS servers' is set to 'Enabled' | **280-290** |
| **14. System Files Ownership** | 2.2.21 Ensure 'Log on as a service' is set to 'No One' (DC only), File and Registry permissions via Security Templates | **120-125** |
| **15. Mail Relaying** | 2.2.33 Ensure 'Simple TCP/IP Services' is set to 'Disabled or Not Installed', 2.2.34 Ensure 'SNMP Service' is set to 'Disabled or Not Installed' | **140-145** |
| **16. Core Dumps** | 18.8.22.1.13 Ensure 'Turn off heap termination on corruption' is set to 'Disabled', Computer Configuration\Windows Settings\Security Settings\Local Policies\Security Options | **320-325** |
| **17. Password, SAM, and Registry Files** | 18.8.21.2 Ensure 'Configure registry policy processing', 2.3.1.1 Ensure 'Accounts: Administrator account status' is set to 'Disabled', 2.3.1.4 Ensure 'Accounts: Guest account status' is set to 'Disabled' | **75-85** |
| **18. Unnecessary Services** | 2.2.1 Ensure 'Access Credential Manager as a trusted caller' is set to 'No One', 2.2.5 Ensure 'Add workstations to domain' is set to 'Administrators' (DC only), Various service disable recommendations | **90-140** |
| **19. Telnet and DHCP Client** | 2.2.38 Ensure 'Telnet' is set to 'Disabled or Not Installed', 18.5.19.2.1 Ensure 'Prohibit installation and configuration of Network Bridge on your DNS domain network' is set to 'Enabled' | **145-150, 295-300** |
| **20. GUI Desktop Environment** | 18.9.15.1 Ensure 'Turn off the advertising ID' is set to 'Enabled', 18.9.16.1 Ensure 'Turn off location' is set to 'Enabled', Control Panel restrictions | **380-390** |
| **21. LDAP Server** | 2.2.17 Ensure 'DNS Server' is set to 'Disabled or Not Installed' (if not a DNS server), Remove unnecessary Windows Features | **130-135** |
| **22. External Devices** | 18.9.10.1.1 Ensure 'Prevent installation of devices that match any of these device IDs' is set to 'Enabled', 18.9.10.1.2 Ensure 'Prevent installation of devices using drivers that match these device setup classes' is set to 'Enabled' | **360-365** |
| **23. Windows Firewall** | 9.1.1 Ensure 'Windows Firewall: Domain: Firewall state' is set to 'On', 9.2.1 Ensure 'Windows Firewall: Private: Firewall state' is set to 'On', 9.3.1 Ensure 'Windows Firewall: Public: Firewall state' is set to 'On' | **190-210** |

## Additional Security Requirements for Ubuntu 20.04

| Security Requirement Category | CIS Benchmark Control | Approximate Page Number |
|-------------------------------|----------------------|------------------------|
| **1. File Integrity Monitoring** | 1.3.1 Ensure AIDE is installed, 1.3.2 Ensure filesystem integrity is regularly checked | **-37-40** |
| **2. Login Banner** | 1.7.2 Ensure local login warning banner is configured properly, 1.7.3 Ensure remote login warning banner is configured properly | **-56-60** |
| **3. Account Lockout & Password Policy** | 5.4.1 Ensure password expiration is set for password policies, 5.3.1 Ensure password creation requirements are configured, 5.3.4 Ensure password hashing algorithm is SHA-512 or stronger | **-166-168, -169-172, -173** |
| **4. Public Directories (Ownership)** | Implicitly covered by general file permissions, e.g., sticky bit on world-writable directories which are root owned by default | **-24 (Section 1.1 Initial Setup - Filesystem Configuration)** |
| **5. Sticky Bit on Public Directories** | 1.1.20 Ensure sticky bit is set on all world-writable directories | **-50-51** |
| **6. Syslog Log File Ownership** | 4.1.4 Ensure rsyslog default file permissions are configured | **-121-122** |
| **7. System Command Ownership** | 6.1.1 Ensure system file permissions are configured (General system file permissions) | **-189-191** |
| **24. Display Unsuccessful Login Attempts** | 5.1.8 Ensure pam_lastlog is enabled | **-164-165** |
| **25. Enable Postfix** | 2.2.12 Configure Postfix for Local Only Mail (if used) | **-75-76** |
| **26. Other Services** | 2.2.x (Specific services, e.g.) 2.2.2 Ensure X Window System is not installed, 2.2.3 Ensure Avahi Server is not enabled, 2.2.19 Disable Bluetooth, 2.2.20 Disable automatic mounting, 2.2.21 Disable outdated preference chrony/ntpd, 2.2.22 Disable outdated | **-70-85 (Section 2.2 Services)** |
| **27. NFS, SCTP, TPC and DCCP** | 3.1.2 Ensure wireless interfaces are disabled (DCCP), 3.1.3 Disable SCTP, 3.1.4 Disable RDS, 3.1.5 Disable TIPC | **-90-93** |
| **28. SSH Service** | 5.2.x (Numerous specific SSH configurations, e.g.) 5.2.3 Ensure permissions on /etc/ssh/sshd_config are configured, 5.2.8 Ensure SSH IgnoreRhosts is enabled, 5.2.10 Ensure SSH root login is disabled, 5.2.14 Ensure SSH MaxAuthTries is set to 4 or less, 5.2.16 Ensure SSH LoginGraceTime is set to one minute or less, 5.2.18 Ensure SSH ClientAliveCountMax is set to 3 or less | **-168-185, -169-171, -ssh Configuration** |
| **29. System Boot Configuration** | 1.5.1 Ensure bootloader password is set, 1.5.2 Ensure permissions on bootloader config are configured | **-44-47** |
| **30. Correct Umask** | 1.5.1 Ensure default umask for users is 027 or more restrictive | **-37-38** |
| **31. Screen Service** | 1.7.4 Ensure console screen lock is enabled (if applicable for Desktop) | **-60-61** |
| **32. Kernel Parameters** | 3.2.x (Various kernel parameter settings, e.g.) 3.2.3 Ensure net.ipv4.tcp_syncookies is enabled, 3.2.5 Ensure net.ipv4.conf.all.accept_redirects is disabled, 3.2.9 Ensure net.ipv6.conf.all.accept_ra_pinfo is disabled | **-97-106 (Section 3.2 Network Parameters)** |
| **33. System Logins** | 5.5.1 Set mesg.group for users | **-187-188** |
| **34. Interactive User** | 5.4.5 Ensure default user shell timeout is disabled | **-187** |
| **35. Virtual and Serial Consoles** | 5.1.5 Limit root login to system console | **-161-162** |
| **36. Attack on VPN Networks** | N/A (Network architecture, not OS hardening control) | **N/A** |
| **37. Linux Security Module** | 1.6.1.1 Ensure AppArmor is installed, 1.6.1.2 Ensure AppArmor is enabled in the bootloader configuration, 1.6.1.3 Ensure all AppArmor Profiles are in enforce or complain mode | **-30-54 (Section 1.6 Mandatory Access Controls)** |
| **38. Filesystem Partition** | 1.1.3 Ensure /tmp is a separate partition, 1.1.5 Ensure separate partition exists for /var, 1.1.6 Ensure separate partition exists for /var/log, 1.1.7 Ensure separate partition exists for /var/log/audit, 1.1.9 Ensure separate partition exists for /home | **-25-28** |
| **39. Samba Client and FTP Protocol** | N/A (generally no specific CIS control if not used; removal of unnecessary packages would be 2.2.x Remove unnecessary packages) | **N/A** |


### Additional Security Requirements

| Security Requirement Category | CIS Benchmark Control | Approximate Page Number |
|------------------------------|----------------------|------------------------|
| **1. File Integrity Monitoring** | Windows File Protection/System File Checker<br>18.8.22.1.1 Ensure 'Turn off Data Execution Prevention for Explorer' is set to 'Disabled' | **-315-320** |
| **2. Login Banner** | 2.3.7.1 Ensure 'Interactive logon: Message text for users attempting to log on' is configured<br>2.3.7.2 Ensure 'Interactive logon: Message title for users attempting to log on' is configured | **-85-90** |
| **3. Account Lockout & Password Policy** | 1.1.1 Ensure 'Enforce password history' is set to '24 or more password(s)'<br>1.1.5 Ensure 'Minimum password length' is set to '14 or more character(s)'<br>1.2.1 Ensure 'Account lockout duration' is set to '15 or more minute(s)'<br>1.2.2 Ensure 'Account lockout threshold' is set to '5 or fewer invalid logon attempt(s)' | **-50-65** |
| **4. Public Directories (Ownership)** | File and folder permissions via Security Templates<br>18.9.30.2 Ensure 'Prevent users from sharing files within their profile' is set to 'Enabled' | **-400-405** |
| **5. Sticky Bit Equivalent** | NTFS permissions and inheritance settings<br>Advanced sharing permissions configuration | **-File System Security** |
| **6. Event Log File Security** | 18.8.21.2 Ensure 'Configure registry policy processing'<br>Event Log policies under Computer Configuration\\Administrative Templates\\Windows Components\\Event Log Service | **-310-315** |
| **7. System Command Security** | Code Integrity policies<br>18.8.22.1.2 Ensure 'Turn off location' is set to 'Enabled' | **-320-325** |
| **24. Display Login Attempts** | 2.3.7.7 Ensure 'Interactive logon: Prompt user to change password before expiration' is set to 'between 5 and 14 days'<br>Security event logging configuration | **-90-95** |
| **25. Mail Service** | 2.2.33 Ensure 'Simple TCP/IP Services' is set to 'Disabled or Not Installed'<br>SMTP service configuration if needed | **-140-145** |
| **26. Other Services** | 2.2.2 Ensure 'Act as part of the operating system' is set to 'No One'<br>2.2.6 Ensure 'Back up files and directories' is set to 'Administrators'<br>2.2.14 Ensure 'Create global objects' is set to 'Administrators, LOCAL SERVICE, NETWORK SERVICE, SERVICE'<br>Multiple service management controls | **-90-140** |
| **27. Network Protocols** | 18.5.4.2 Ensure 'MSS: (NoNameReleaseOnDemand)' is set to 'Enabled'<br>18.5.11.1 Ensure 'MSS: (SafeDllSearchMode) Enable Safe DLL search mode' is set to 'Enabled'<br>Network protocol security settings | **-280-295** |
| **28. RDP/Remote Desktop** | 18.9.59.2.2 Ensure 'Do not allow passwords to be saved' is set to 'Enabled'<br>18.9.59.3.9.1 Ensure 'Set client connection encryption level' is set to 'Enabled: High Level'<br>18.9.59.3.11.1 Ensure 'Do not allow drive redirection' is set to 'Enabled' | **-450-470** |
| **29. Boot Configuration** | 18.4.1 Ensure 'MSS: (AutoAdminLogon) Enable Automatic Logon' is set to 'Disabled'<br>2.3.7.8 Ensure 'Interactive logon: Smart card removal behavior' is set to 'Lock Workstation' or higher | **-75-80**<br>**-95-100** |
| **30. User Rights Assignment** | 2.2.1 through 2.2.39 - Complete User Rights Assignment section<br>File and folder permissions | **-90-150** |
| **31. Screen Lock** | 18.1.1.1 Ensure 'Prevent enabling lock screen camera' is set to 'Enabled'<br>18.1.1.2 Ensure 'Prevent enabling lock screen slide show' is set to 'Enabled'<br>Screen saver policies | **-250-255** |
| **32. Registry Security** | Registry permissions and access control<br>18.8.21.2 Ensure 'Configure registry policy processing'<br>2.3.10.1 Ensure 'Network access: Allow anonymous SID/Name translation' is set to 'Disabled' | **-100-110** |
| **33. User Session Management** | 2.3.7.4 Ensure 'Interactive logon: Machine inactivity limit' is set to '900 or fewer second(s)'<br>2.3.11.8 Ensure 'Network security: LAN Manager authentication level' is set to 'Send NTLMv2 response only. Refuse LM & NTLM' | **-90-95**<br>**-110-115** |
| **34. Interactive User Controls** | 18.9.6.1 Ensure 'Allow users to enable online speech recognition services' is set to 'Disabled'<br>User rights and logon restrictions | **-340-345** |
| **35. Console Access** | 2.2.32 Ensure 'Log on as a batch job' is set to 'Administrators' (DC Only)<br>2.2.35 Ensure 'Log on locally' user right policy | **-140-145** |
| **36. VPN Security** | Windows built-in VPN client security settings<br>18.5.19.2.1 Ensure 'Prohibit installation and configuration of Network Bridge' is set to 'Enabled' | **-295-300** |
| **37. Windows Defender** | 18.9.39.2 Ensure 'Configure detection for potentially unwanted applications' is set to 'Enabled: Block'<br>18.9.39.15 Ensure 'Turn on real-time protection' is set to 'Enabled'<br>Windows Defender Antivirus policies | **-420-440** |
| **38. Disk Partitioning** | BitLocker Drive Encryption policies<br>18.9.10.2.1 Ensure 'Allow use of Camera' is set to 'Disabled'<br>Drive encryption and partition security | **-360-370** |
| **39. SMB/File Sharing** | 18.5.11.2 Ensure 'MSS: (ScreenSaverGracePeriod) The time in seconds before the screen saver grace period expires' is set to 'Enabled: 5 or fewer seconds'<br>2.3.8.1 Ensure 'Microsoft network client: Digitally sign communications (always)' is set to 'Enabled'<br>2.3.8.2 Ensure 'Microsoft network client: Digitally sign communications (if server agrees)' is set to 'Enabled' | **-105-110** |


#### Key Areas Covered for Ubuntu 20.04:
- Audit and Logging (Controls 8-11, 24)
- Network Security (Controls 13, 23, 27-28, 36)
- Access Control (Controls 2-3, 31, 33-35)
- File System Security (Controls 1, 4-7, 38)
- Service Management (Controls 12, 15, 18-22, 26, 29)
- System Configuration (Controls 14, 16-17, 30, 32, 37, 39)

#### Implementation Priority for Ubuntu 20.04:
- Critical: Audit logging, firewall coniguration, access controls
- High: File integrity monitoring, login banners, service management
- Medium: System hardening, kernel parameters, filesystem partitioning
## Windows 10 Specific Security Features

### Additional Windows 10 Controls

| **Windows Security** | Windows Defender, SmartScreen, and Windows Security Center configuration | **-420-450** |
| **BitLocker** | 18.9.10.1.1 through 18.9.10.1.17 - Complete BitLocker policies<br>Full disk encryption requirements | **-360-380** |
| **Windows Update** | 18.9.101.1.1 Ensure 'Configure Automatic Updates' is set to 'Enabled'<br>18.9.101.1.2 Ensure 'Configure Automatic Updates: Scheduled install day' is set to '0 - Every day' | **-490-500** |
| **Microsoft Edge** | 18.9.35.1 through 18.9.35.15 - Complete Microsoft Edge policies<br>Browser security configuration | **-410-420** |
| **Windows Store** | 18.9.80.1.1 Ensure 'Only display the private store within the Microsoft Store' is set to 'Enabled'<br>App installation restrictions | **-480-485** |
| **Credential Guard** | Windows Defender Credential Guard configuration<br>Virtualization-based security settings | **-Hardware Security** |

## Summary

This assessment covers **39 major security requirements** plus **6 Windows 10-specific controls** based on CIS Microsoft Windows 10 Enterprise Benchmark v1.12.0.

### Key Areas Covered:
- **Account Policies** (Controls 3, 24, 33-34)
- **Local Policies** (Controls 2, 29-32, 35)
- **Event Log** (Controls 8-11, 6)
- **Restricted Groups** (Service accounts and administrative access)
- **System Services** (Controls 12, 15, 18-22, 25-26)
- **Registry** (Controls 32, 4, 16-17)
- **File System** (Controls 1, 4-7, 38)
- **Windows Firewall** (Control 23)
- **Advanced Audit Policy** (Controls 8-11)
- **Administrative Templates** (Controls 13, 20, 22, 27-28, 36-39)

### Implementation Priority:
1. **Critical**: Password policies, account lockout, Windows Firewall, Windows Defender
2. **High**: Audit logging, user rights assignment, service management, BitLocker
3. **Medium**: Registry security, file permissions, application restrictions

### Windows 10 Enterprise Features:
- **Windows Defender Advanced Threat Protection**
- **Credential Guard and Device Guard**
- **BitLocker with TPM**
- **Windows Hello for Business**
- **Controlled Folder Access**



