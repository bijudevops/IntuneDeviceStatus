# Intune Management Status Checker

A PowerShell script to determine if a Windows device is **managed by Microsoft Intune**.  
It gathers signals from Azure AD join status, MDM enrollment keys, and the Intune Management Extension service, then produces a clear verdict.

---

## 🔍 Features
- Runs `dsregcmd /status` to check **Azure AD Join** and **MDM enrollment URL**.
- Reads registry keys under `HKLM:\SOFTWARE\Microsoft\Enrollments` for **MS DM Server** enrollment.
- Detects the **Intune Management Extension** service (Win32 app management).
- Outputs a structured summary and a human-readable **Conclusion**.
- Returns an **exit code** for scripting/automation:
  - `0` = Intune-managed / likely managed
  - `1` = Not Intune-managed

---

## 📋 Requirements
- Windows 10 or 11
- PowerShell 5.1 or later
- Run as Administrator (recommended for registry access)

---

## 🚀 Usage
1. Download the script:

   ```powershell
   git clone https://github.com/<your-repo>/Intune-Management-Status.git
   cd Intune-Management-Status

ComputerName         : DESKTOP-1234
AzureAdJoined        : YES
WorkplaceJoined      : NO
MDMUrl               : https://enrollment.manage.microsoft.com/EnrollmentServer/Discovery.svc
AADDeviceId          : 12345678-abcd-efgh-ijkl-9876543210
EnrolledViaMSDMServer: True
EnrollmentCount      : 1
IMEPresent           : True
IMERunning           : True
Conclusion           : INTUNE-MANAGED (MDM enrolled via MS DM Server; MDMUrl present)

powershell.exe -ExecutionPolicy Bypass -File .\Get-IntuneManagementStatus.ps1
if ($LASTEXITCODE -eq 0) {
    Write-Output "✅ Device is Intune-managed"
} else {
    Write-Output "❌ Device is NOT Intune-managed"
}
