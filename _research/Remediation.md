---
title: "Remediation"
layout: single-portfolio
excerpt: <img width="1400" height="800" alt="image" src="https://github.com/user-attachments/assets/3706eba1-a06a-4673-93ad-8091d5632eb5" />
collection: research
order_number: 40
header: 
  og_image: "research/ternary.png"
---

In this thesis the researcher has given a proper "REMEDIATION" steps to vulnerabilities found during the scans in the form of code with respect to individual vulnereabilities.
The CIS-CAT on Ubuntu 20.04 and Windows_10_Ent encourage 'MANUAL' remediation while OpenSCAP give a combined method 'Automated and Manual'.
Remediation is divided into 2 parts, with the first 1st being Full Remediation and the second 2nd is Selective Remediation.
#### Option A: Full Generated Script using OpenSCAP (CAUTION) for Ubuntu 20.04
```bash
# BACKUP FIRST - This will make system-wide changes
# Create restoration point
sudo cp /etc/passwd /etc/passwd.pre-remediation
sudo cp /etc/shadow /etc/shadow.pre-remediation
sudo cp -r /etc/ssh /etc/ssh.pre-remediation

# Generate remediation script for CIS Level 1 Workstation
oscap xccdf generate fix \
    --profile xccdf_org.ssgproject.content_profile_cis_level1_workstation \
    --output ~/openscap-results/baseline/cis-l1-workstation-remediation.sh \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml
# Generate remediation script for CIS Level 2 Workstation
oscap xccdf generate fix \
    --profile xccdf_org.ssgproject.content_profile_cis_level2_workstation \
    --output ~/openscap-results/baseline/cis-l2-workstation-remediation.sh \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml

# Make remediation scripts executable
chmod +x ~/openscap-results/baseline/cis-l*-workstation-remediation.sh

# Run the full remediation
sudo ./cis-l1-workstation-remediation.sh 2>&1 | tee remediation-log.txt
```
#### Option B: Selective Remediation using recommendations from CIS-CAT (**RECOMMENDED**) for for Ubuntu 20.04
```bash
# Extract specific remediation commands and run them individually

# Example: Fix file permissions
echo "=== Fixing file permissions ==="
sudo chmod 644 /etc/passwd
sudo chmod 600 /etc/shadow
sudo chmod 600 /etc/gshadow
sudo chmod 644 /etc/group

# Example: Configure system auditing
echo "=== Installing and configuring audit daemon ==="
sudo apt install -y auditd audispd-plugins
sudo systemctl enable auditd
sudo systemctl start auditd

# Example: Configure login banner
echo "=== Setting up login banner ==="
sudo tee /etc/issue << 'EOF'
WARNING: Unauthorized access to this system is prohibited.
All connections are monitored and recorded.
EOF

sudo tee /etc/issue.net << 'EOF'
WARNING: Unauthorized access to this system is prohibited.
All connections are monitored and recorded.
EOF

                          OR

# WEEK 1: CRITICAL INFRASTRUCTURE
echo "=== WEEK 1: CRITICAL INFRASTRUCTURE ==="

# 1. Enable Firewall Protection
echo "Configuring UFW Firewall..."
systemctl unmask ufw.service
systemctl --now enable ufw.service
ufw allow ssh
ufw default deny incoming
ufw default deny outgoing
ufw allow out http
ufw allow out https
ufw allow out 53
ufw logging on
ufw --force enable

# 2. Configure Network Security Parameters
echo "Configuring Network Security..."
cat >> /etc/sysctl.conf << EOF
# Network Security Parameters
net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.log_martians = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.tcp_syncookies = 1
EOF
sysctl -p

# 3. Configure PAM Account Lockout
echo "Configuring PAM Account Lockout..."
apt update && apt install -y libpam-pwquality
echo "deny = 5" >> /etc/security/faillock.conf
echo "unlock_time = 900" >> /etc/security/faillock.conf

# 4. Secure Log File Permissions
echo "Securing Log Files..."
find /var/log -type f -exec chmod 640 {} \\;
find /var/log -type f -exec chown root:adm {} \\;

# WEEK 2: SECURITY HARDENING
echo "=== WEEK 2: SECURITY HARDENING ==="

# Install AppArmor
echo "Installing AppArmor..."
apt install -y apparmor apparmor-utils
sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX="apparmor=1 security=apparmor"/' /etc/default/grub
update-grub

# Configure Password Quality
echo "Configuring Password Policies..."
cat >> /etc/security/pwquality.conf << EOF
# Password Quality Requirements
minlen = 14
difok = 2
maxrepeat = 3
maxsequence = 3
enforce_for_root
EOF

# Secure Cron
echo "Securing Cron..."
chown root:root /etc/crontab
chmod og-rwx /etc/crontab
chown root:root /etc/cron.hourly /etc/cron.daily /etc/cron.weekly /etc/cron.monthly /etc/cron.d
chmod og-rwx /etc/cron.hourly /etc/cron.daily /etc/cron.weekly /etc/cron.monthly /etc/cron.d
```
Verify Remediation Results using Lynis, OpenSCAP and CIS-CAT for Ubuntu 20.04
```bash
Use LYNIS for deep scan
sudo lynis audit system # Test run system audit
                  OR
# Re-run the assessment to check improvements
mkdir -p ~/openscap-results/post-remediation
# Run post-remediation assessment
oscap xccdf eval \
    --profile xccdf_org.ssgproject.content_profile_cis_level1_workstation \
    --results ~/openscap-results/post-remediation/cis-l1-post-remediation-results.xml \
    --report ~/openscap-results/post-remediation/cis-l1-post-remediation-report.html \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml

# Compare before and after
```
Rollback Procedure (If Needed) for Ubuntu 20.04
```bash
# Create rollback script
cat > rollback-remediation.sh << 'EOF'
#!/bin/bash
echo "Rolling back remediation changes..."
# Restore from backup
BACKUP_DIR="/root/openscap-backup/$(ls -1 /root/openscap-backup/ | tail -1)"
if [ -d "$BACKUP_DIR" ]; then
    cp -r $BACKUP_DIR/ssh /etc/
    cp $BACKUP_DIR/login.defs /etc/
    systemctl restart ssh
    echo "Rollback completed from: $BACKUP_DIR"
else
    echo "No backup directory found!"
fi
EOF

chmod +x rollback-remediation.sh
```
Monitoring and Maintenance for Ubuntu 20.04
```bash
# Set up regular compliance checking
cat > /etc/cron.weekly/openscap-check << 'EOF'
#!/bin/bash
/usr/local/bin/oscap xccdf eval \
    --profile xccdf_org.ssgproject.content_profile_cis_level1_workstation \
    --results /var/log/openscap/weekly-$(date +%Y%m%d)-results.xml \
    --report /var/log/openscap/weekly-$(date +%Y%m%d)-report.html \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml
EOF

sudo mkdir -p /var/log/openscap
sudo chmod +x /etc/cron.weekly/openscap-check
```
#### Manual Remediation for Windows_10 using
```powershell
# GROUP 1: Initial Setup - Windows Update Configuration
Write-Host "=== Configuring Windows Update ===" -ForegroundColor Yellow
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -Force
Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -Name "AUOptions" -Value 4

# GROUP 2: Services - Disable Unnecessary Services
Write-Host "=== Disabling Tablet PC Input Service ===" -ForegroundColor Yellow
Stop-Service -Name "TabletInputService" -Force -ErrorAction SilentlyContinue
Set-Service -Name "TabletInputService" -StartupType Disabled

# GROUP 3: Network Security - Disable SSL 2.0
Write-Host "=== Disabling SSL 2.0 Client ===" -ForegroundColor Yellow
New-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client" -Force
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client" -Name "Enabled" -Value 0

# GROUP 4: Host-Based Firewall - Block SMB Port
Write-Host "=== Blocking SMB port 445 ===" -ForegroundColor Yellow
netsh advfirewall firewall add rule name="Block SMB" dir=in action=block protocol=TCP localport=445

# GROUP 5: Access Control - Enhanced UAC Settings
Write-Host "=== Configuring UAC settings ===" -ForegroundColor Yellow
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorAdmin" -Value 2

# GROUP 6: Logging and Auditing - Increase Security Log Size
Write-Host "=== Increasing Security log size ===" -ForegroundColor Yellow
wevtutil sl Security /ms:196608000

# GROUP 7: System Maintenance - Enable Safe DLL Search Mode
Write-Host "=== Enabling Safe DLL Search Mode ===" -ForegroundColor Yellow
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager" -Name "SafeDllSearchMode" -Value 1

Additional PowerShell 7 Compatible Examples
# Storage Sense Configuration
Write-Host "=== Configuring Storage Sense ===" -ForegroundColor Yellow
New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\StorageSense" -Force
Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\StorageSense" -Name "AllowStorageSenseGlobal" -Value 1

                                OR

# Windows Security Remediation - PowerShell 7 Manual Commands
# CIS Microsoft Windows 10 Enterprise Benchmark v4.0.0 Level 1
# Requires -Version 7.0 RunAsAdministrator
# ===============================================================================
# Function to create registry path if it doesn't exist (use before using codes in the GROUPS)
function New-RegistryPath {
    param([string]$Path)
    if (!(Test-Path $Path)) {
        try {
            New-Item -Path $Path -Force -ErrorAction Stop | Out-Null
            Write-Host "Created registry path: $Path" -ForegroundColor Green
        }
        catch {
            Write-Warning "Failed to create registry path: $Path - $($_.Exception.Message)"
        }
    }
}

# Function to safely set registry property
function Set-SafeRegistryProperty {
    param(
        [string]$Path,
        [string]$Name,
        [object]$Value,
        [string]$Type = "DWord"
    )
    
    New-RegistryPath -Path $Path
    
    try {
        Set-ItemProperty -Path $Path -Name $Name -Value $Value -Type $Type -Force -ErrorAction Stop
        Write-Host "Set $Path\$Name = $Value" -ForegroundColor Cyan
    }
    catch {
        Write-Warning "Failed to set $Path\$Name - $($_.Exception.Message)"
    }
}

Write-Host "Starting Windows Security Hardening..." -ForegroundColor Yellow
# ===============================================================================
# GROUP 1: INITIAL SETUP - Easy Implementation Controls
# ===============================================================================
Write-Host "`n=== GROUP 1: Initial Setup ===" -ForegroundColor Magenta
# Windows Update Automatic Policies (Critical Impact)
Write-Host "Configuring Windows Update policies..." -ForegroundColor Yellow
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -Name "NoAutoUpdate" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -Name "AUOptions" -Value 4
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -Name "ScheduledInstallDay" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -Name "ScheduledInstallTime" -Value 3
# Storage Sense Automated Cleanup (Medium Impact)
Write-Host "Configuring Storage Sense policies..." -ForegroundColor Yellow
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\StorageSense"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\StorageSense" -Name "AllowStorageSenseGlobal" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\StorageSense" -Name "AllowStorageSenseTemporaryFilesCleanup" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\StorageSense" -Name "StorageSenseCloudContentDehydrationThreshold" -Value 30
# System Restore Point Policies (Medium Impact)
Write-Host "Configuring System Restore policies..." -ForegroundColor Yellow
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\SystemRestore"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\SystemRestore" -Name "DisableConfig" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\SystemRestore" -Name "DisableSR" -Value 0
# Enable System Restore (using WMI instead of unavailable cmdlets)
Write-Host "Enabling System Restore..." -ForegroundColor Yellow
try {
    Enable-ComputerRestore -Drive "C:\" -ErrorAction Stop
    Checkpoint-Computer -Description "Security Remediation Baseline" -RestorePointType "MODIFY_SETTINGS" -ErrorAction Stop
    Write-Host "System Restore configured successfully" -ForegroundColor Green
}
catch {
    Write-Host "System Restore cmdlets not available in PowerShell 7. Using alternative method..." -ForegroundColor Yellow
    try {
        $wmi = Get-WmiObject -Class "Win32_SystemRestore" -ErrorAction Stop
        $wmi.Enable("C:\")
        Write-Host "System Restore enabled via WMI" -ForegroundColor Green
    }
    catch {
        Write-Warning "Could not enable System Restore: $($_.Exception.Message)"
    }
}

# Disk Quota Management (Medium Impact)
Write-Host "Configuring disk quota..." -ForegroundColor Yellow
try {
    fsutil quota enforce C:
    fsutil quota modify C: 10737418240 8589934592 Administrator
    Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "NtfsDisableLastAccessUpdate" -Value 1
}
catch {
    Write-Warning "Disk quota configuration failed: $($_.Exception.Message)"
}
# ===============================================================================
# GROUP 2: SERVICES - Easy Implementation Controls
# ===============================================================================
Write-Host "`n=== GROUP 2: Services ===" -ForegroundColor Magenta
# Task Scheduler Security Policies (Medium Impact)
Write-Host "Configuring Task Scheduler policies..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule" -Name "DisableRpcOverTcp" -Value 1
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Task Scheduler5.0"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Task Scheduler5.0" -Name "Disable New Task Creation" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Task Scheduler5.0" -Name "Allow Cross Forest Tasks" -Value 0

# Disable Unnecessary Services (Medium Impact)
Write-Host "Disabling unnecessary services..." -ForegroundColor Yellow
$servicesToDisable = @(
    "TabletInputService",        # Tablet PC Input Service
    "WSearch",                   # Windows Search (if not needed)
    "Spooler",                   # Print Spooler (if no printers)
    "Fax",                       # Fax Service
    "TapiSrv",                   # Telephony
    "SensorService",             # Sensor Service
    "SensrSvc",                  # Sensor Monitoring Service
    "RetailDemo",                # Retail Demo Service
    "dmwappushservice",          # Device Management Wireless Application Protocol
    "MapsBroker"                 # Downloaded Maps Manager
)

foreach ($service in $servicesToDisable) {
    try {
        if (Get-Service -Name $service -ErrorAction SilentlyContinue) {
            Stop-Service -Name $service -Force -ErrorAction SilentlyContinue
            Set-Service -Name $service -StartupType Disabled -ErrorAction Stop
            Write-Host "Disabled service: $service" -ForegroundColor Green
        }
    }
    catch {
        Write-Warning "Failed to disable service $service`: $($_.Exception.Message)"
    }
}

# Service Failure Recovery Actions (Medium Impact)
Write-Host "Configuring service recovery actions..." -ForegroundColor Yellow
sc.exe failure "Themes" reset= 86400 actions= restart/60000/restart/60000/restart/60000
sc.exe failure "AudioSrv" reset= 86400 actions= restart/60000/restart/60000/restart/60000
sc.exe failure "EventLog" reset= 86400 actions= restart/60000/restart/60000/restart/60000

# Service Account Restrictions (High Impact)
Write-Host "Configuring service account restrictions..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" -Name "RestrictAnonymous" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" -Name "RestrictAnonymousSAM" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\lanmanserver\parameters" -Name "RestrictNullSessAccess" -Value 1

# ===============================================================================
# GROUP 3: NETWORK - Easy Implementation Controls
# ===============================================================================
Write-Host "`n=== GROUP 3: Network Security ===" -ForegroundColor Magenta

# IPv6 Transition Security (High Impact)
Write-Host "Configuring IPv6 security..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters" -Name "DisabledComponents" -Value 0xFF
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\TCPIP\v6Transition"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\TCPIP\v6Transition" -Name "Force_Tunneling" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\TCPIP\v6Transition" -Name "6to4_State" -Value 1

# SSL Configuration Hardening (High Impact)
Write-Host "Hardening SSL configuration..." -ForegroundColor Yellow

# Disable SSL 2.0
New-RegistryPath -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client"
New-RegistryPath -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server"
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client" -Name "Enabled" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server" -Name "Enabled" -Value 0

# Disable SSL 3.0
New-RegistryPath -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client"
New-RegistryPath -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server"
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client" -Name "Enabled" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server" -Name "Enabled" -Value 0

# Network Provider Security Settings (Medium Impact)
Write-Host "Configuring network provider security..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\NetworkProvider\Order" -Name "ProviderOrder" -Value "RDPNP,LanmanWorkstation,webclient" -Type "String"
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Network Connections"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Network Connections" -Name "NC_ShowSharedAccessUI" -Value 0

# WLAN Media Cost Policies (Medium Impact)
Write-Host "Configuring WLAN policies..." -ForegroundColor Yellow
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WcmSvc\GroupPolicy"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WcmSvc\GroupPolicy" -Name "fMinimizeConnections" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WcmSvc\GroupPolicy" -Name "fBlockNonDomain" -Value 1

# Network Connection Security (High Impact)
Write-Host "Configuring network connection security..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Network Connections" -Name "NC_StdDomainUserSetLocation" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Network Connections" -Name "NC_AllowNetBridge_NLA" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Netbt\Parameters" -Name "NoNameReleaseOnDemand" -Value 1

# ===============================================================================
# GROUP 4: HOST-BASED FIREWALL - Easy Implementation Controls
# ===============================================================================

Write-Host "`n=== GROUP 4: Firewall Configuration ===" -ForegroundColor Magenta

# Advanced Firewall Logging (Medium Impact)
Write-Host "Configuring firewall logging..." -ForegroundColor Yellow
netsh advfirewall set allprofiles logging filename %systemroot%\system32\LogFiles\Firewall\pfirewall.log
netsh advfirewall set allprofiles logging maxfilesize 4096
netsh advfirewall set allprofiles logging droppedconnections enable
netsh advfirewall set allprofiles logging allowedconnections enable

# Custom Firewall Rules (Medium Impact)
Write-Host "Adding custom firewall rules..." -ForegroundColor Yellow
netsh advfirewall firewall add rule name="Block WinRM HTTP" dir=in action=block protocol=TCP localport=5985
netsh advfirewall firewall add rule name="Block WinRM HTTPS" dir=in action=block protocol=TCP localport=5986
netsh advfirewall firewall add rule name="Block SMB" dir=in action=block protocol=TCP localport=445
netsh advfirewall firewall add rule name="Block NetBIOS" dir=in action=block protocol=TCP localport=139
netsh advfirewall firewall add rule name="Block RPC Endpoint Mapper" dir=in action=block protocol=TCP localport=135

# Firewall Notification Settings (Medium Impact)
Write-Host "Configuring firewall notifications..." -ForegroundColor Yellow
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\DomainProfile"
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\PrivateProfile"
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\PublicProfile"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\DomainProfile" -Name "DisableNotifications" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\PrivateProfile" -Name "DisableNotifications" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\PublicProfile" -Name "DisableNotifications" -Value 0

# ===============================================================================
# GROUP 5: ACCESS CONTROL - Easy Implementation Controls
# ===============================================================================

Write-Host "`n=== GROUP 5: Access Control ===" -ForegroundColor Magenta

# Enhanced UAC Settings (High Impact)
Write-Host "Configuring UAC settings..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorAdmin" -Value 2
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorUser" -Value 3
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "EnableInstallerDetection" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ValidateAdminCodeSignatures" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "EnableSecureUIAPaths" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "EnableLUA" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "PromptOnSecureDesktop" -Value 1

# Interactive Logon Security (High Impact)
Write-Host "Configuring logon security..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "DontDisplayLastUserName" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "DontDisplayLockedUserId" -Value 3
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "InactivityTimeoutSecs" -Value 900
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "ScreenSaverGracePeriod" -Value 5

# Network Logon Restrictions (High Impact)
Write-Host "Configuring network logon restrictions..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" -Name "LimitBlankPasswordUse" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" -Name "NoLMHash" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" -Name "LmCompatibilityLevel" -Value 5
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" -Name "NTLMMinClientSec" -Value 537395200
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" -Name "NTLMMinServerSec" -Value 537395200

# Session Timeout Policies (Medium Impact)
Write-Host "Configuring session timeouts..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -Name "autodisconnect" -Value 15
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -Name "EnableSecuritySignature" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -Name "RequireSecuritySignature" -Value 1

# ===============================================================================
# GROUP 6: LOGGING AND AUDITING - Easy Implementation Controls
# ===============================================================================

Write-Host "`n=== GROUP 6: Logging and Auditing ===" -ForegroundColor Magenta

# Event Log Size Configuration (Medium Impact)
Write-Host "Configuring event log sizes..." -ForegroundColor Yellow
wevtutil sl Security /ms:196608000
wevtutil sl Application /ms:32768000
wevtutil sl System /ms:32768000
wevtutil sl "Windows PowerShell" /ms:32768000

# Log Retention Policies (Medium Impact)
Write-Host "Configuring log retention..." -ForegroundColor Yellow
wevtutil sl Security /rt:false
wevtutil sl Application /rt:false
wevtutil sl System /rt:false
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\EventLog\Security"
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\EventLog\Application"
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\EventLog\System"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\EventLog\Security" -Name "Retention" -Value "0" -Type "String"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\EventLog\Application" -Name "Retention" -Value "0" -Type "String"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\EventLog\System" -Name "Retention" -Value "0" -Type "String"

# Additional Audit Categories (Medium Impact)
Write-Host "Configuring audit policies..." -ForegroundColor Yellow
auditpol /set /subcategory:"Credential Validation" /success:enable /failure:enable
auditpol /set /subcategory:"Application Group Management" /success:enable /failure:enable
auditpol /set /subcategory:"Computer Account Management" /success:enable /failure:enable
auditpol /set /subcategory:"Distribution Group Management" /success:enable /failure:enable
auditpol /set /subcategory:"Other Account Management Events" /success:enable /failure:enable
auditpol /set /subcategory:"Security Group Management" /success:enable /failure:enable
auditpol /set /subcategory:"User Account Management" /success:enable /failure:enable
auditpol /set /subcategory:"Process Creation" /success:enable /failure:enable
auditpol /set /subcategory:"Directory Service Changes" /success:enable /failure:enable
auditpol /set /subcategory:"Directory Service Replication" /success:enable /failure:enable
auditpol /set /subcategory:"Detailed Directory Service Replication" /success:enable /failure:enable

# Event Log Security Permissions (Medium Impact)
Write-Host "Configuring event log permissions..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\EventLog\Security" -Name "RestrictGuestAccess" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\EventLog\Application" -Name "RestrictGuestAccess" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\EventLog\System" -Name "RestrictGuestAccess" -Value 1

# ===============================================================================
# GROUP 7: SYSTEM MAINTENANCE - Easy Implementation Controls
# ===============================================================================

Write-Host "`n=== GROUP 7: System Maintenance ===" -ForegroundColor Magenta

# System Settings Hardening (High Impact)
Write-Host "Hardening system settings..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager" -Name "SafeDllSearchMode" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager" -Name "ProtectionMode" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "DisableCAD" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Setup\RecoveryConsole" -Name "SecurityLevel" -Value 0
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Setup\RecoveryConsole" -Name "SetCommand" -Value 0

# File and Folder Permission Templates (Medium Impact)
Write-Host "Setting file and folder permissions..." -ForegroundColor Yellow
try {
    icacls "C:\Windows\System32\config" /inheritance:r /grant:r "Administrators:(OI)(CI)F" "SYSTEM:(OI)(CI)F"
    icacls "C:\Windows\System32\drivers" /inheritance:r /grant:r "Administrators:(OI)(CI)F" "SYSTEM:(OI)(CI)F"
    icacls "C:\Windows\System32\spool" /inheritance:r /grant:r "Administrators:(OI)(CI)F" "SYSTEM:(OI)(CI)F"
    Write-Host "File permissions configured successfully" -ForegroundColor Green
}
catch {
    Write-Warning "Some file permission changes failed: $($_.Exception.Message)"
}

# Registry Access Restrictions (Medium Impact)
Write-Host "Configuring registry access restrictions..." -ForegroundColor Yellow
Set-SafeRegistryProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\SecurePipeServers\winreg" -Name "Description" -Value "Registry Server" -Type "String"

# System Maintenance Scheduling (Medium Impact)
Write-Host "Creating maintenance tasks..." -ForegroundColor Yellow
try {
    # Create a simple security check script
    $scriptPath = "C:\Scripts"
    if (!(Test-Path $scriptPath)) {
        New-Item -Path $scriptPath -ItemType Directory -Force | Out-Null
    }
    
    $securityScript = @"
# Simple security check script
Get-Date | Out-File "C:\Scripts\LastSecurityCheck.txt"
Get-EventLog -LogName Security -Newest 10 | Out-File "C:\Scripts\RecentSecurityEvents.txt"
"@
    
    $securityScript | Out-File -FilePath "C:\Scripts\SecurityCheck.ps1" -Force
    
    schtasks /create /tn "Security Baseline Check" /tr "powershell.exe -ExecutionPolicy Bypass -File C:\Scripts\SecurityCheck.ps1" /sc weekly /d SUN /st 02:00 /ru SYSTEM /f
    schtasks /create /tn "Event Log Cleanup" /tr "wevtutil cl Application" /sc monthly /d 1 /st 03:00 /ru SYSTEM /f
    Write-Host "Maintenance tasks created successfully" -ForegroundColor Green
}
catch {
    Write-Warning "Failed to create maintenance tasks: $($_.Exception.Message)"
}

# Storage Sense Cleanup Policies (Medium Impact)
Write-Host "Configuring storage cleanup..." -ForegroundColor Yellow
New-RegistryPath -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Storage Sense"
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Storage Sense" -Name "AllowStorageSenseTemporaryFilesCleanup" -Value 1
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Storage Sense" -Name "ConfigStorageSenseRecycleBinCleanupThreshold" -Value 30
Set-SafeRegistryProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Storage Sense" -Name "ConfigStorageSenseDownloadsCleanupThreshold" -Value 30

# ===============================================================================
# VERIFICATION AND RESTART RECOMMENDATIONS
# ===============================================================================

Write-Host "`n=== Security Remediation Complete ===" -ForegroundColor Green
Write-Host "Manual verification recommended for:" -ForegroundColor Yellow
Write-Host "1. Windows Update settings in Settings > Update & Security" -ForegroundColor White
Write-Host "2. Firewall rules in Windows Defender Firewall with Advanced Security" -ForegroundColor White
Write-Host "3. UAC settings in Control Panel > User Accounts" -ForegroundColor White
Write-Host "4. Event log sizes in Event Viewer properties" -ForegroundColor White
Write-Host "5. Service status in services.msc" -ForegroundColor White
Write-Host ""
Write-Host "RESTART REQUIRED for some changes to take effect" -ForegroundColor Red
Write-Host "Run 'Restart-Computer -Force' when ready" -ForegroundColor Yellow
Write-Host ""
Write-Host "Script completed with error handling. Check any warnings above." -ForegroundColor Green

# Use the below codes to REMEDIATE important services 
The Service dependencies, Registry Security, Performance Counter Services and WMI (Windows Management Instrumentation) should not be changed but only if needed.

# Re-enable critical WMI services
Set-Service -Name "WmiApSrv" -StartupType Automatic
Start-Service -Name "WmiApSrv"
Set-Service -Name "PerfHost" -StartupType Manual

# Re-register performance counters
lodctr /R

# Restart WMI service
Restart-Service -Name "Winmgmt" -Force

# Optional: Re-enable Windows Search if you need those counters
Set-Service -Name "WSearch" -StartupType Automatic
Start-Service -Name "WSearch"
```
