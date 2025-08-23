---
permalink: /software/
title: "Softwares"
gallery:
---
Software is a set of instructions, data, or programs used to operate computers and execute specific tasks. In the world of technology research, documentation of tools, applications, and software employed is fundamental to the completeness and reproducibility of any study. 
### OS scan, audit and hardening
This study entails installing, configuring, scanning, auditing and hardening for two (2) popular 'Operating Systems' which are in four (4) parts. The 1st is the installation of OpenSCAP, CIS-CAT Lite Assessor, and Lynis on Ubuntu 20.04 workstation; the 2nd is the installation of Microsoft Security Compliance Toolkit and CIS-CAT Lite Assessor on Windows 10 Enterprise; the 3rd is the scanning and auditing of both OS's; while the 4th is remediation/hardening of discovered vulnerabilities after the scan while simultaneously gathering system performance metrics (memory usage, disk I/O, and CPU load) for (pre-scan, during scan, and post-scan) as security operation is conducted.
### Phase 1: OS Audit and Hardening tools installation on Ubuntu 20.04
#### [OpenSCAP](https://www.open-scap.org/) Installation 
The OpenSCAP will be installed using the 'Build and install from SOURCE method' as the traditional way of installation lacks important dependencies, workbench profiles, compliance security guide and dev environment. Below is the following steps for the installation procedure.
```bash
## Update system and install basic tools
sudo apt update && sudo apt install curl wget git vim -y
## Install OpenSCAP dependencies
sudo apt install cmake build-essential pkg-config \
    libxml2-dev libxslt1-dev libpcre3-dev libcurl4-openssl-dev \
    librpm-dev libbz2-dev libxmlsec1-dev libglib2.0-dev \
    libacl1-dev libselinux1-dev libdbus-1-dev libpopt-dev \
    python3-dev python3-pytest doxygen swig -y
```
```bash
# Clone and build OpenSCAP from the repository
git clone https://github.com/OpenSCAP/openscap.git
cd openscap # Navigate to the project directory
mkdir build # Create and enter build directory
cd build
cmake ../ # Generate build files with CMake
make -j$(nproc) # Compile using all available CPU cores
sudo make install # Install Openscap
```
#### Locate and create symlink for oscap binary
```bash
# Find where OpenSCAP was installed (system-wide)
sudo find /usr/local -name "oscap" -type f 2>/dev/null
# Find oscap binary in build directory
find ~/openscap/build -name "oscap" -type f 2>/dev/null
# Create symlink to make oscap available system-wide
sudo ln -s /home/$USER/openscap/build/utils/oscap /usr/local/bin/oscap
oscap --version # Verify installation
```
#### [CIS Compliance](https://github.com/ComplianceAsCode/content) Benchmark and Security Guide installation for OpenSCAP
After installing OpenSCAP, also install its security profile from GitHub with:
```bash
# Download the latest SCAP Security Guide
wget https://github.com/ComplianceAsCode/content/releases/download/v0.1.76/scap-security-guide-0.1.76.zip
# Extract the archive
unzip scap-security-guide-0.1.76.zip
# Create directory for SCAP content
sudo mkdir -p /usr/share/xml/scap/ssg/content/
# Copy SCAP content files to system directory
cd scap-security-guide-0.1.76
sudo cp *.xml /usr/share/xml/scap/ssg/content/
# Verify content installation
sudo ls -la /usr/share/xml/scap/ssg/content/
# Find Ubuntu-specific SCAP content
sudo find /usr/share -name "*ubuntu*" | grep -i scap

# Test Ubuntu 20.04 content availability
sudo oscap info /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml 2>/dev/null || echo "Ubuntu 20.04 
# List all available DataStream files
ls -la /usr/share/xml/scap/ssg/content/ssg-*-ds.xml
```
```bash
# Run SCAP evaluation with standard profile for Ubuntu 20.04
sudo oscap xccdf eval \
    --profile xccdf_org.ssgproject.content_profile_standard \
    --results results.xml \
    --report report.html \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml
# Alternative: Run evaluation for Ubuntu 22.04 (or choose your version if available)
# sudo oscap xccdf eval \
#     --profile xccdf_org.ssgproject.content_profile_standard \
#     --results results-ubuntu2204.xml \
#     --report report-ubuntu2204.html \
#     /usr/share/xml/scap/ssg/content/ssg-ubuntu2204-ds.xml
# Show all available profiles for Ubuntu 20.04
sudo oscap info /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml | grep -A 50 "Profiles:"
```
#### Additional useful commands
```bash
# Generate remediation script
sudo oscap xccdf generate fix \
    --profile xccdf_org.ssgproject.content_profile_standard \
    --output remediation-script.sh \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml
# Check for specific compliance frameworks
sudo oscap info /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml | grep -i "cis\|nist\|pci"
# To view HTML report use: firefox report.html or your preferred browser
```
```bash
# Troubleshoot: If content files are missing, check what was actually extracted
ls -la scap-security-guide-0.1.76/
# Verify oscap can access the files
sudo oscap --version
# Check file permissions
ls -la /usr/share/xml/scap/ssg/content/
# If evaluation fails, run with verbose output
sudo oscap xccdf eval \
    --profile xccdf_org.ssgproject.content_profile_standard \
    --results results.xml \
    --report report.html \
    --verbose INFO \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2204-ds.xml
```
#### [CIS-CAT Lite](https://learn.cisecurity.org/cis-cat-lite) Audit Installation 
CIS-CAT Lite is an audit tool incorporated with CIS benchmark and is useful for manual hardening.
```bash
# CIS-CAT Lite Installation and Setup
sudo apt install -y openjdk-11-jdk # Install Java (CIS-CAT dependency) OpenJDK 11
java --version # Verify Java installation
echo $JAVA_HOME # Check Java environment
which java
```
#### Set up CIS-CAT directory structure
```bash
# Create CIS-CAT working directory
mkdir -p ~/ciscat
cd ~/ciscat
# Extract to openscap build folder if preferred
unzip ~/Downloads/'CIS-CAT Lite Assessor v4.55.0.zip' -d ~/openscap/build/

# Navigate to extracted CIS-CAT directory
cd ~/ciscat/Assessor-CLI/
chmod +x ./Assessor-CLI.sh # Make the assessor executable
./Assessor-CLI.sh # Run CIS-CAT with basic options
# Run with HTML report output
./Assessor-CLI.sh -b benchmarks/CIS_Ubuntu_Linux_20.04_LTS_Benchmark_v1.1.0-xccdf.xml -r ~/ciscat/reports/
```
#### Troubleshooting
```bash
# If Java version issues occur
java --version
# Check if CIS-CAT files are properly extracted
find ~/ciscat -name "*.sh" -type f
# If permission denied errors
chmod -R +x ~/ciscat/Assessor-CLI/
```
#### [Lynis](https://github.com/CISOfy/Lynis) Audit Installation
This is a deep scanning and audit tool that will be used to verify `True/False Positive or Negative`.
```bash
sudo git clone https://github.com/CISOfy/lynis.git # Clone the Lynis repository
# Create a symlink to run Lynis from anywhere
sudo ln -s /opt/lynis/lynis /usr/local/bin/lynis
lynis --version # Verify installation
sudo lynis audit system # Test run system audit
```
### Phase 2: Microsoft SCT and CIS-CAT installation on Windows_10_Enterprise
#### [Microsoft Security Compliance Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=55319) 
Since the researcher is using a virtual machine, there isa  need to install a compatible scripting environment, hence installing the latest `PowerShell 7 Installation`
```powershell
# Navigate to the root of C: drive
Set-Location C:\
# Check Winget version (ensure it is installed)
winget --version
# Install the latest PowerShell 7 using Winget (requires Admin rights)
winget install --id Microsoft.Powershell --source winget
```
`Microsoft Security Compliance Toolkit installation`
```powershell
# Quick all-in-one Microsoft Security Compliance Toolkit (SCT) installer
function Install-CompleteSCT {
    $SCTPath = "C:\SecurityCompliance"
    New-Item -Path $SCTPath -ItemType Directory -Force

    Write-Host "📥 Downloading complete Microsoft SCT suite..." -ForegroundColor Cyan

    # All major SCT components
    $downloads = @{
        "Windows10-21H2"   = "https://download.microsoft.com/download/8/5/C/85C25433-A1B0-4FFA-9429-7E023E7DA8D8/Windows%2010%20Version%2021H2%20Security%20Baseline.zip"
        "Windows10-2004"   = "https://download.microsoft.com/download/8/5/C/85C25433-A1B0-4FFA-9429-7E023E7DA8D8/Windows%2010%20Version%202004%20and%20Windows%20Server%20Version%202004%20Security%20Baseline.zip"
        "PolicyAnalyzer"   = "https://download.microsoft.com/download/8/5/C/85C25433-A1B0-4FFA-9429-7E023E7DA8D8/PolicyAnalyzer.zip"
        "EdgeBaseline"     = "https://download.microsoft.com/download/8/5/C/85C25433-A1B0-4FFA-9429-7E023E7DA8D8/Microsoft%20Edge%20Security%20Baseline.zip"
    }

    foreach ($name in $downloads.Keys) {
        $url = $downloads[$name]
        $zipFile = "$SCTPath\$name.zip"
        $extractPath = "$SCTPath\$name"

        Write-Host "🔽 Downloading $name..." -ForegroundColor Yellow
        Invoke-WebRequest -Uri $url -OutFile $zipFile -UseBasicParsing

        Write-Host "📦 Extracting $name..." -ForegroundColor Yellow
        Expand-Archive -Path $zipFile -DestinationPath $extractPath -Force
        Remove-Item $zipFile -Force

        Write-Host "✅ $name installed" -ForegroundColor Green
    }

    Write-Host "`n🎉 Complete SCT installation finished!" -ForegroundColor Green
    Write-Host "📁 Location: $SCTPath" -ForegroundColor Cyan

    Start-Process explorer.exe -ArgumentList $SCTPath
}

# Run complete installation
Install-CompleteSCT
```
`Java 11 installation`
```powershell
# Install OpenJDK 11 (required for running CIS-CAT Lite Assessor)
winget install --id Microsoft.OpenJDK.11 --source winget
# Refresh the environment variable for the current session
$env:Path = [System.Environment]::GetEnvironmentVariable("Path", "Machine") + ";" +
            [System.Environment]::GetEnvironmentVariable("Path", "User") Java -version
```
#### [CIS-CAT Lite](https://learn.cisecurity.org/cis-cat-lite) Audit Installation for Windows_10
```powershell
New-Item -ItemType Directory -Path "$env:USERPROFILE\Desktop\MSc" -Force # Create a folder MSc
# Extract the CIS-CAT Lite zip file to the MSc folder
Expand-Archive -Path "$env:USERPROFILE\Downloads\CIS-CAT Lite Assessor v4.54.1.zip" `
               -DestinationPath "$env:USERPROFILE\Desktop\MSc" -Force
Set-Location "$env:USERPROFILE\Desktop\MSc" # Navigate to CIS-CAT Assessor folder
Get-ChildItem # Step 4: View folder contents
Set-Location ".\CIS-CAT Lite Assessor v4.54.1\Assessor" #Enter the Assessor directory 
# Step 6: Confirm the contents (.jar files, config folders, etc.)
Get-ChildItem
# To scan with CIS benchmark
java -jar Assessor-CLI.jar -b "benchmarks\CIS_Microsoft_Windows_10_Enterprise_Benchmark_v4.0.0-xccdf.xml" -p "Level 1 (L1) - Corporate/Enterprise Environment (general use)" -html -txt
```
### Phase 3: Scanning and Baseline Assessment of both OSs 
Click  on [**THESIS SCRIPT**](https://godswill-ikot.github.io/research/Scripts/) to review, copy and use the script used in my actual thesis research titled "Operating System Hardening and Vulnerability Assessment Using CIS Benchmarking and System Security Tools on Windows and Linux".
Note: The script contains every tool on this page and every system info needed to verify system performance for pre, during and post scan.
#### Scanning and assessment Using OpenSCAP on Ubuntu 20.04
```bash
# Create timestamped directories for better organisation
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
mkdir -p ~/openscap-results/baseline/$TIMESTAMP
# CIS LEVEL 1 Workstation
oscap xccdf eval \
    --profile xccdf_org.ssgproject.content_profile_cis_level1_workstation \
    --results ~/openscap-results/baseline/$TIMESTAMP/cis-l1-workstation-results.xml \
    --report ~/openscap-results/baseline/$TIMESTAMP/cis-l1-workstation-report.html \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml
# CIS LEVEL 2 Workstation
oscap xccdf eval \
    --profile xccdf_org.ssgproject.content_profile_cis_level2_workstation \
    --results ~/openscap-results/baseline/$TIMESTAMP/cis-l2-workstation-results.xml \
    --report ~/openscap-results/baseline/$TIMESTAMP/cis-l2-workstation-report.html \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml
# Run with OVAL results and verbose output
oscap xccdf eval \
    --profile xccdf_org.ssgproject.content_profile_cis_level1_workstation \
    --results ~/openscap-results/baseline/cis-l1-workstation-detailed-results.xml \
    --report ~/openscap-results/baseline/cis-l1-workstation-detailed-report.html \
    --oval-results \
    --verbose INFO \
    /usr/share/xml/scap/ssg/content/ssg-ubuntu2004-ds.xml
```
#### Scanning and Assessment using CIS-CAT on Ubuntu 20.04
```bash
# List available profiles first
java -jar Assessor-CLI.jar \
    -b "benchmarks/CIS_Ubuntu_Linux_20.04_LTS_Benchmark_v3.0.0-xccdf.xml" \
    -D session.type=local \
    -p
chmod +x Assessor.sh # Give permission to script
sudo ./Assessor.sh # Run script
            # OR
# Run Level 1 Workstation assessment
sudo java -jar Assessor-CLI.jar \
    -D session.type=local \
    -b "benchmarks/CIS_Ubuntu_Linux_20.04_LTS_Benchmark_v3.0.0-xccdf.xml" \
    -p "Level 1 - Workstation" \
    -html
```
#### View, Assess and  Generate Remediation scripts for results for Ubuntu scan
After scans, it's important to view and assess the results to help understand the important areas of the OS to apply hardening security so as not to disrupt the OS functionality generally.
```bash
## Open HTML Reports for CIS-CAT (Graphical Interface)
# Option 1: Firefox (if available)
firefox *.html &

# Option 2: Default system browser
xdg-open *.html

# Option 3: Chromium/Chrome
chromium-browser *.html &
google-chrome *.html &

# Open HTML reports (if GUI available)
# firefox ~/openscap-results/baseline/ubuntu-cis-l1-workstation-report.html &
# firefox ~/openscap-results/baseline/ubuntu-cis-l2-workstation-report.html &

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
```
### Phase 4: Remediation of vulnerabilities
The [**REMEDIATION**](https://godswill-ikot.github.io/research/Remediation/) process is divided into parts which are "Full and Selective/Manual" remediation 
#### Option A: Run Full Generated Script (CAUTION) for Ubuntu 20.04
```bash
# BACKUP FIRST - This will make system-wide changes
echo "WARNING: This will make significant system changes!"
echo "Press Ctrl+C to cancel, or Enter to continue..."
read

# Create restoration point
sudo cp /etc/passwd /etc/passwd.pre-remediation
sudo cp /etc/shadow /etc/shadow.pre-remediation
sudo cp -r /etc/ssh /etc/ssh.pre-remediation

# Run the full remediation
echo "Running CIS Level 1 remediation..."
sudo ./cis-l1-workstation-remediation.sh 2>&1 | tee remediation-log.txt
```
#### Option B: Run Selective Remediation (RECOMMENDED) for Ubuntu 20.04
```bash
# Run the custom selective remediation
echo "Running selective remediation..."
sudo ./custom-remediation.sh 2>&1 | tee selective-remediation-log.txt
                        OR
# Extract specific remediation commands and run them individually

#  Fix file permissions
echo "=== Fixing file permissions ==="
sudo chmod 644 /etc/passwd
sudo chmod 600 /etc/shadow
sudo chmod 600 /etc/gshadow
sudo chmod 644 /etc/group

# Configure SSH (if needed)
sudo sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo systemctl restart ssh

# Set password policies
sudo apt install libpam-pwquality -y
sudo sed -i 's/# minlen = 8/minlen = 14/' /etc/security/pwquality.conf
sudo sed -i 's/# dcredit = 0/dcredit = -1/' /etc/security/pwquality.conf
sudo sed -i 's/# ucredit = 0/ucredit = -1/' /etc/security/pwquality.conf
sudo sed -i 's/# lcredit = 0/lcredit = -1/' /etc/security/pwquality.conf
sudo sed -i 's/# ocredit = 0/ocredit = -1/' /etc/security/pwquality.conf

# Configure system auditing
echo "=== Installing and configuring audit daemon ==="
sudo apt install -y auditd audispd-plugins
sudo systemctl enable auditd
sudo systemctl start auditd

# Configure login banner
echo "=== Setting up login banner ==="
sudo tee /etc/issue << 'EOF'
WARNING: Unauthorized access to this system is prohibited.
All connections are monitored and recorded.
EOF

sudo tee /etc/issue.net << 'EOF'
WARNING: Unauthorized access to this system is prohibited.
All connections are monitored and recorded.
EOF
```
Verify Remediation Results for Ubuntu 20.04
```bash
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

