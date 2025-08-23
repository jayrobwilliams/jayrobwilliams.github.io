---
title: "Linux Audit Tool Installation"
layout: single-portfolio
excerpt: <img width="311" height="162" alt="image" src="https://github.com/user-attachments/assets/f8896a7f-7932-4f4d-9c96-91b74c3cb977" />
collection: research
order_number: 10
header: 
  og_image: "images/New folder/Linux (1).png"
---
The ever expanding threat and breach on organizations has made operating systems auditing and hardening of great importance; Hence, i have give a step-wise direction on installing industry standard open-source tool for auditing and hardening.
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

