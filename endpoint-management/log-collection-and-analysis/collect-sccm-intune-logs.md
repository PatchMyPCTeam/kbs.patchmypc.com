---
title: "Collecting Log Files to Send to Support for ConfigMgr and Intune"
date: 2019-10-08
taxonomy:
    products:
        - 
    tech-stack:
        - 
    solutions:
        - endpoint-management
    post_tag:
        - 
    sub-solutions:
        - log-collection-and-analysis
        - troubleshooting
        - education
---

To help us diagnose specific issues, you may need to send us **log files** from **ConfigMgr** or **Intune**.

This article reviews the logs files required to allow our support team to **analyze** and **troubleshoot** different scenarios. Please review the different scenarios below.

**When sending the logs files, please attach the logs as a ZIP file using our [technical support form](/technical-support "Technical Support Contact Form").**

## Server-Side Logs

### Software Updates - Failing to Publish Updates Using Patch My PC's Publisher

When using the Patch My PC **Publisher to published third-party updates to WSUS**, we will need the following log files from the SUP/WSUS server where the service is installed.

- %PatchMyPCInstallDirectory%\\PatchMyPC.log

- %PatchMyPCInstallDirectory%\\PatchMyPC\*.lo\_

- %PatchMyPCInstallDirectory%\\Settings.xml

- %PatchMyPCInstallDirectory%\\PatchMyPC-DownloadHistory.csv

- %PatchMyPCInstallDirectory%\\PatchMyPC-PublishingHistory.csv

- %SiteServerLogsFolder%\\Wsyncmgr\*.log

- %SiteServerLogsFolder%\\WCM\*.log

The following log file may be needed **upon request only due to large file size**

- %ProgramFiles%\\Update Services\\LogFiles\\SoftwareDistribution.log

### Software Updates - Failing to Publish Updates Using ConfigMgr In-Console Publishing

When using the **ConfigMgr in-console publishing**, we will need the following log files from the SUP/WSUS server where the service is installed.

- %SiteSystemLogsFolder%\\SMS\_ISVUPDATES\_SYNCAGENT\*.log

- %SiteServerLogsFolder%\\Wsyncmgr\*.log

- %SiteServerLogsFolder%\\WCM\*.log

The following log file may be needed **upon request only due to large file size**

- %ProgramFiles%\\Update Services\\LogFiles\\SoftwareDistribution.log

### ConfigMgr Applications - Failing to Create/Update ConfigMgr Applications Using Patch My PC's Publisher

When using the Patch My PC **Publisher for ConfigMgr application creation**, we will need the following log files to troubleshoot applications failing to create.

- %PatchMyPCInstallDirectory%\\PatchMyPC.log

- %PatchMyPCInstallDirectory%\\PatchMyPC\*.lo\_

- %PatchMyPCInstallDirectory%\\Settings.xml

- %PatchMyPCInstallDirectory%\\PatchMyPC-DownloadHistory.csv

- %PatchMyPCInstallDirectory%\\PatchMyPC-PublishingHistory.csv

- %SCCMInstallFolder%\\Logs\\SMSProv\*.log

### Intune Applications - Failing to Create/Update Applications Using Patch My PC's Publisher

When using the Patch My PC **Publisher for Intune application creation**, we will need the following log files to troubleshoot applications failing to create.

- %PatchMyPCInstallDirectory%\\PatchMyPC.log

- %PatchMyPCInstallDirectory%\\PatchMyPC\*.lo\_

- %PatchMyPCInstallDirectory%\\Settings.xml

- %PatchMyPCInstallDirectory%\\PatchMyPC-DownloadHistory.csv

- %PatchMyPCInstallDirectory%\\PatchMyPC-PublishingHistory.csv

## Client-Side Logs

### Intune Applications/Updates Failing to Install on Client Devices

When troubleshooting **Intune application installation errors on a client**, we will need multiple client logs. Please include the following logs:

- %ProgramData%\\PatchMyPCIntuneLogs\\PatchMyPC-ScriptRunner.log
    - This may be found in the %ProgramData%\\PatchMyPC if the Install was initiated by the user from Company Portal.

- %ProgramData%\\PatchMyPCIntuneLogs\\PatchMyPC-SoftwareDetectionScript.log

- %ProgramData%\\PatchMyPCIntuneLogs\\PatchMyPC-SoftwareUpdateDetectionScript.log

- %ProgramData%\\Microsoft\\IntuneManagementExtension\\Logs\\AgentExecutor\*.log

- %ProgramData%\\Microsoft\\IntuneManagementExtension\\Logs\\IntuneManagementExtension\*.log

- %ProgramData%\\Microsoft\\IntuneManagementExtension\\Logs\\AppWorkload\*.log

- %ProgramData%\\PatchMyPC\\PatchMyPC-UserNotification.log

- %ProgramData%\\PatchMyPC\\UISettingsUINotificationSettings.xml

If there are any rolled over logs, please include those, too.

**Note:** Some Patch My PC log files listed above may be found in **%WinDir%\\CCM** folder if that folder exists.

### ConfigMgr Applications Failing to Install on Client Devices

When troubleshooting **ConfigMgr application installation errors on a client**, we will need multiple client logs. Please include the following logs:

- %WinDir%\\CCM\\Logs\\AppDiscovery\*.log

- %WinDir%\\CCM\\Logs\\AppEnforce\*.log

- %WinDir%\\CCM\\Logs\\AppIntentEval\*.log

- %WinDir%\\CCM\\Logs\\CAS\*.log

- %WinDir%\\CCM\\Logs\\CIAgent.\*log

- %WinDir%\\CCM\\Logs\\DataTransferService\*.log

- %WinDir%\\CCM\\Logs\\PatchMyPC-ScriptRunner.log
    - This may be found in the %ProgramData%\\PatchMyPC if the Install was initiated by the user from Software Center.

- %WinDir%\\CCM\\Logs\\PatchMyPC-SoftwareDetectionScript.log
    - This may be found in the %temp% of the user who clicked 'Install' in Software Center if it was an 'Available' deployment.

- %WinDir%\\CCM\\Logs\\StateMessage.log

- %ProgramData%\\PatchMyPC\\PatchMyPC-UserNotification.log

- %ProgramData%\\PatchMyPC\\UISettingsUINotificationSettings.xml

### Third-Party Software Updates Failing to Install on Client Devices

When troubleshooting **update installation errors on a client**, we will need the following client logs:

- %WinDir%\\CCM\\Logs\\CAS\*.log

- %WinDir%\\CCM\\Logs\\DeltaDownload\*.log

- %WinDir%\\CCM\\Logs\\DataTransferService\*.log

- %WinDir%\\CCM\\Logs\\PatchMyPC-ScriptRunner.log (If exist)
    - This may be found in the %ProgramData%\\PatchMyPC if the Install was initiated by the user from Software Center.

- %WinDir%\\CCM\\Logs\\ScanAgent\*.log

- %WinDir%\\CCM\\Logs\\StateMessage.log

- %WinDir%\\CCM\\Logs\\UpdatesDeployment\*.log

- %WinDir%\\CCM\\Logs\\UpdatesHandler\*.log

- %WinDir%\\CCM\\Logs\\UpdatesStore\*.log

- %WinDir%\\CCM\\Logs\\WUAHandler\*.log

- %WinDir%\\WindowsUpdate.log
    - You need to run **[Get-WindowsUpdateLog on Windows 8.1 and newer](https://docs.microsoft.com/en-us/powershell/module/windowsupdate/get-windowsupdatelog?view=win10-ps)** in PowerShell.

- %ProgramData%\\PatchMyPC\\PatchMyPC-UserNotification.log

- %ProgramData%\\PatchMyPC\\UISettingsUINotificationSettings.xml

### Download Stuck at 0% / Waiting to install / Preparing to download

When troubleshooting **update installation errors on a client**, we will need the following client logs:

- %WinDir%\\CCM\\Logs\\CAS\*.log

- %WinDir%\\CCM\\Logs\\CIAgent.\*log

- %WinDir%\\CCM\\Logs\\ClientLocation\*.log

- %WinDir%\\CCM\\Logs\\CMBITSManager\*.log

- %WinDir%\\CCM\\Logs\\ContentTransferManager\*.log

- %WinDir%\\CCM\\Logs\\DataTransferService\*.log

- %WinDir%\\CCM\\Logs\\StateMessage.log

- %WinDir%\\CCM\\Logs\\LocationServices.log\*.log

- %WinDir%\\CCM\\Logs\\UpdatesDeployment\*.log

- %WinDir%\\CCM\\Logs\\UpdatesHandler\*.log

- %WinDir%\\CCM\\Logs\\UpdatesStore\*.log

- %WinDir%\\CCM\\Logs\\PatchMyPC-ScriptRunner.log (If exist)
    - This may be found in the %ProgramData%\\PatchMyPC if the Install was initiated by the user from Software Center.

## WSUS Specific Logs

### Server Side

- %ProgramFiles%\\Update Services\\LogFiles\\SoftwareDistribution.log

### Client Side

- %WinDir%\\WindowsUpdate.log or

- %USERPROFILE%\\Desktop\\WindowsUpdate.log **\***
    - **\*** You need to run **[Get-WindowsUpdateLog on Windows 8.1 and newer](https://docs.microsoft.com/en-us/powershell/module/windowsupdate/get-windowsupdatelog?view=win10-ps)** in PowerShell to generate this log file

## Configuration Manager Specific  Logs

### Automatic Deployment Rule Failing for Third-Party Updates

When troubleshooting **automatic deployment rules failing for third-party updates**, we will need the following server-side logs. In this example, we will assume the main SCCM installation directory is: **C:\\Program Files\\Microsoft Configuration Manager**

- C:\\Program Files\\Microsoft Configuration Manager\\Logs\\ruleengine\*.log

- The [PatchDownloader.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SU_NAPLog) (Location may vary)
    - C:\\Program Files\\SMS\_CCM\\Logs\\PatchDownloader\*.log (Most common location)
    
    - C:\\Program Files\\Microsoft Configuration Manager\\Logs\\PatchDownloader\*.log (Possible location)
    
    - %WinDir%\\CCM\\Logs (Possible location)
    
    - If **unable to locate** a current [PatchDownloader.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SU_NAPLog), check HKLM\\SOFTWARE\\MicrosoftCCMLogging@Global:LogDirectory on the [site server](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/plan-for-site-system-servers-and-site-system-roles#configuration-manager-site-server)
        - ![how to locate the patchdownloader.log file](/_images/locate-the-patchdownloader-log-file.png "how to locate the patchdownloader.log file")
            

### Updates Failing to Download to Deployment Package using ConfigMgr Console

- When troubleshooting **updates failing to download into a deployment package from the ConfigMgr console**, we will need the following log from the machine running the ConfigMgr console:

- %temp%\\PatchDownloader\*.log
    - Note: if you are using an **RDP session the patchdownloader.log may be in a numbered sub-folder** in your users **%temp%** folder.

### Patch My PC Publisher - How to Enable Debug Logging

Enabling Debug logging is often helpful for troubleshooting unique issues with publishing. Follow the steps below to enable Debug logging:

1. Open the Publisher

3. Click on the **General** tab

5. In the dropdown under **Logging Options** select **Debug  
    ![Enable Debug Logging in Publisher](/_images/PatchMyPC-Settings_VB3O5uDPhE.png "Enable Debug Logging in Publisher")
    **

7. Close the Publisher

9. Open Services.msc and locate the **PatchMyPCService**

11. Right-click and Restart the **PatchMyPCService**

13. Debug Logging is now enabled

## Advanced Insights Specific Logs

### Advanced Insights Log Collector

In the installation directory, server-side, of Advanced Insights, is a standalone .exe which can collect all the necessary server-side log files useful for troubleshooting - bundled into a single .zip.

You can read more about **AdvancedInsightsLogDiag.exe** here: 

- [Advanced Insights Log Collector | Getting Started (patchmypc.com)](https://docs.patchmypc.com/installation-guides/advanced-insights-and-patch-insights/advanced-insights-log-collector)

### Installation

When troubleshooting the installation for Advanced Insights, the installation creates a log file at:

- %temp%\\AdvInsights.log

### Services

The following log files can be found installed server-side where you have installed Advanced Insights:

- %ProgramData%\\AdvancedInsightsLogs\\API\*.log

- Application Windows Event Log
    - Can be filtered on ".NET Runtime" source

### Clients

The following log files can be found client-side:

- %WinDir%\\CCM\\Logs\\PMPCInventory.log
