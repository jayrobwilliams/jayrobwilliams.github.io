---
title: "Microsoft SCT and CIS-CAT Installation"
layout: single-portfolio
excerpt: <img width="940" height="400" alt="image" src="https://github.com/user-attachments/assets/99440c02-c4ae-49d5-a8ad-e685efd48e66" />
collection: research
order_number: 20
header: 
  og_image: "research/map.png"
---
Since the researcher is using a virtual machine, there is a need to install a compatible scripting environment, hence installing the latest PowerShell 7 Installation and **Run as ADMINISTRATION**
```powershell
# Navigate to the root of C: drive
Set-Location C:\
# Check Winget version (ensure it is installed)
winget --version
# Install the latest PowerShell 7 using Winget (requires Admin rights)
winget install --id Microsoft.Powershell --source winget
```
#### Microsoft Security Compliance Toolkit installation
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
#### Java 11 installation
```powershell
# Install OpenJDK 11 (required for running CIS-CAT Lite Assessor)
winget install --id Microsoft.OpenJDK.11 --source winget
# Refresh the environment variable for the current session and java version
$env:Path = [System.Environment]::GetEnvironmentVariable("Path", "Machine") + ";" +
            [System.Environment]::GetEnvironmentVariable("Path", "User") Java -version
```
#### CIS-CAT Lite Audit Installation for Windows_10
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
Phase 3: Scanning and Baseline Assessment of both OSs
```
