---
title: "System Error 1612 - The installation source for this product is not available."
date: 2020-12-11
taxonomy:
    products:
        - 
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - 
---

When uninstalling, repair or attempting to patch a product you may receive an error with an **exit code of 1603, 1612, or 0x8024002D (-2145124307), or 0X80240022 (-2145124318).**

The message generated in log files or Event Viewer can indicate that the product failed to uninstall/repair/patch due to some arbitrary reason e.g. "**Failed to remove**" or "**cannot be removed**".

## Determine if You Are Affected

If you are lucky, you will get a more obvious clue like "**Installation source for this product is not available**".

Here's an example of error messages we typically see in the **[vendor's MSI log file](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#install-logging)**.

Google Chrome -- Error 1714. The older version of Google Chrome cannot be removed. Contact your technical support group. System Error 1612.

Windows Installer installed the product. Product Name: Google Chrome. Product Version: 68.46.66. Product Language: 1033. Manufacturer: Google LLC. Installation success or error status: 1603.

Error 1714.The older version of Adobe Flash Player 32 ActiveX cannot be removed. Contact your technical support group. System Error 1612.

Here is an example of what you might see in **WUAHandler.log**:

Update 1 (x) finished installing (0x8024002d), Reboot Required? No

Installation job encountered some failures. Job Result = 0x80240022.

The error will often be returned with **exit code 1603**, which can be vague or troublesome to diagnose, or sometimes it is **1612 or 0x8024002D.**

If you run the installer manually, you will likely see an error similar to **The feature you are trying to use is on a network resource that's unavailable**

![The feature you are trying to use is on a network resource that's unavailable](/_images/The-feature-you-are-trying-to-use-is-on-a-network-resource-thats-unavailable.png "The feature you are trying to use is on a network resource that's unavailable")

## Root Cause for System Error 1612 (The installation source for this product is not available)

The error code **1612** is the most helpful to determine the root cause. If you perform an error lookup in CMTrace.exe, it will return **_The installation source for this product is not available. Verify that the source exists and that you can access it._**

![MSI Error 1612](/_images/MSI-Error-1612.png "MSI Error 1612")

**0x8024002D (-2145124307)** resolves to _**A full-file update could not be installed because it required the source.**_

When an MSI-based product is uninstalled, repaired, or upgraded, the previous **installer (.msi file) must be available** to complete the operation.

Access to the original MSI generally isn't an issue because, during installation, the windows installer should **copy the MSI file** to **[C:\\Windows\\Installer](https://docs.microsoft.com/en-us/archive/blogs/joscon/can-you-safely-delete-files-in-the-windirinstaller-directory)**.

When a product is uninstalled, **[Windows Installer](https://docs.microsoft.com/en-us/windows/win32/msi/windows-installer-portal)** will first try to perform the uninstall using the MSI file cached to **C:\\Windows\\Installer**. If the MSI file **does not exist in C:\\Windows\\Installer**, it will fall back to the original path used to perform the install.

The path to the cached installer is read by Windows Installer from the registry value **LocalPackage** of key **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Installer\\UserData\\S-1-5-18\\Products\\Install\\Properties**.

The path to the original source folder is read from the registry value **InstallSource** of the same registry key.

![](/_images/2020-12-11_15-27-56-2.jpg)

You can see a Google Chrome installer (**C:\\Windows\\CCMcache\\3w\\GoogleChromeStandaloneEnterprise64.msi**) is installed and is in ccmcache. You can also see the file was cached to **C:\\Windows\\Installer67aa2f8a.msi**.

It is worth noting that for repairs operations (the **/f** switch of **msiexec**), we noticed that the path(s) listed in the value(s) of registry key **HKEY\_CLASSES\_ROOT\\Installer\\Products\\SourceList\\Net** are used to find the cached or original installer, instead of the previously mentioned registry values under HKLM.

If the MSI file doesn't exist in the **C:\\Windows\\Installer** or **original source folder**, you will receive this error.

For example, we deleted both files above and attempted to uninstall or upgrade **Google Chrome** **87.0.4280.88 to anything newer**. You can see it's unable to perform the upgrade in this scenario since the previous version fails to uninstall due to missing installer.

![](/_images/2020-12-11_15-42-53.jpg)

> **Note:** It's common for the MSI InstallSource to not be accessible if it were in the **[ccmcache folder](https://docs.microsoft.com/en-us/mem/configmgr/core/clients/manage/manage-clients#BKMK_ClientCache)** and it was purged. However the primary issue is when the installer is deleted from **[C:\\Windows\\Installer](https://docs.microsoft.com/en-us/archive/blogs/joscon/can-you-safely-delete-files-in-the-windirinstaller-directory)**.
> 
> You should avoid deleting any files within **C:\\Windows\\Installer**. The common need to do this is to alleviate storage pressures because over time, after installing many applications, its storage capacity can be relatively large on thinly provisioned storage. Instead of deleting the files or the whole folder itself to regain storage space, it is recommended to uninstall unneeded applications.
> 
> Another common cause for missing installers is with the vendors MSI installer not caching itself to **C:\\Windows\\Installer**.

## How to Resolve MSI Error 1612  / System Error 1612 During MSI Upgrade or Uninstall

When you receive this error, you will need **the same version of the MSI file,** and it also needs to be **the same name** as it appears in the **PackageName** registry value of key **HKEY\_CLASSES\_ROOT\\Installer\\Products\\SourceList** (or what the file name is being prompted for by the Windows Installer dialogue as shown above).

Once you have sourced the original file, you have a couple of options:

- If you have a small number of impacted devices, consider manually restoring and renaming the MSI file to the paths referenced in the registry (**InstallSource** and **LocalPackage** of key **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Installer\\UserData\\S-1-5-18\\Products\\Install\\Properties**)

- Otherwise, using something like Configuration Manager or Intune, you could create an application/package which downloads the file to a predictable path on disk and then update the aforementioned registry values. **This method carries a significant risk of causing irreversible damage** to the systems in scope. It is highly recommended you fully understand the issue and perform lots of testing before committing to this approach. A safer approach could be including a script within the application/package that tries to determine the needed paths to restore the MSI to and copies + renames it to them.

A tool such as **[FixMissingMSI](https://github.com/suyouquan/SQLSetupTools#fixmissingmsi-version-22 "FixMissingMSI")** can also be an excellent tool in this situation. It will scan the local registry for all installed MSI applications that Windows Installer knows about, and verifies the cached or original installer is still accessible on the system. If it isn't, it will clearly show you which ones are missing and offer an easy way to restore the file(s) if you have them.

Alternatively, you could use consider Microsoft's [Program Install and Uninstall troubleshooter](https://support.microsoft.com/en-us/topic/fix-problems-that-block-programs-from-being-installed-or-removed-cca7d1b6-65a9-3d98-426b-e9f927e1eb4d).

## Additional Resources

Below you will find a list of additional resources related to this scenario:

- [Windows Installer source list update fails - Configuration Manager | Microsoft Docs](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/windows-installer-source-list-update-fails)

- [Can you safely delete files in the %windir%Installer directory? | Microsoft Docs](https://docs.microsoft.com/en-us/archive/blogs/joscon/can-you-safely-delete-files-in-the-windirinstaller-directory)

- [Restore the missing Windows Installer cache files - SQL Server | Microsoft Docs](https://docs.microsoft.com/en-us/troubleshoot/sql/install/restore-missing-windows-installer-cache-files)

- [Software Deployment : MSI cache locations and deploying with SCCM (itninja.com)](https://www.itninja.com/question/msi-cache-locations-and-deploying-with-sccm#:~:text=MSI%20files%20are%20cached%20in%20the%20Windows%20Installer,CAB%20file%20steamed%20into%20the%20MSI%20are%20OK.)

- [Wise Packaging - Symantec Enterprise (broadcom.com)](https://community.broadcom.com/symantecenterprise/communities/community-home/librarydocuments/viewdocument?DocumentKey=358ba549-46f7-4185-a089-89d92ef87893&CommunityKey=41d8253b-a238-4563-8718-ed7623beafbc&tab=librarydocuments)

- [Windows Installer Error 1612 | Desktop App | Autodesk Knowledge Network](https://knowledge.autodesk.com/support/desktop-app/learn-explore/caas/CloudHelp/cloudhelp/ENU/DesktopApp-Content/files/GUID-1F985644-50D5-47DE-9F81-C6C3E182E762-htm.html)