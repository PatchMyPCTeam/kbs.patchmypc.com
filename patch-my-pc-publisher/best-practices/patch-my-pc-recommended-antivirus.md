---
title: "Patch My PC - Recommended antivirus exclusions"
date: 2023-12-14
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - best-practices
        - security
        - education
---

This article outlines Microsoft's recommended antivirus exclusions for **Configuration Manager**, **WSUS**, and **Intune**. It documents folders pertinent to content distribution, particularly those within Patch My PC's scope of third-party updates. We've included links to relevant Microsoft documentation for a deeper dive into the topic.

**Note:** If you are looking for Attack Surface Reduction (ASR) Rule advice, please visit [A guide to using Attack Surface Reduction Rules (ASR) with Patch My PC - Patch My PC](https://patchmypc.com/kb/attack-surface-reduction-rules-patch-my-pc-guide/)

## Patch My PC Publisher

If you are using the **on-premises Patch My PC Publisher** for third-party patching, kindly ensure the following antivirus exclusion:

- **PostponedBinaries**
    - This folder is used to store package sources which are temporarily held due to the [Delay the in-place application upgrade by x days](/base-install-update-options-explained#delay-upgrade) feature

> **Note:** The  folder is specified in the registry key **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Patch My PC Publishing Service:Path**

## Configuration Manager / WSUS

### Server-side

Microsoft recommends quite a few antivirus exclusions when it comes to Configuration Manager (server-side). The **Patch My PC Publisher** interacts with the following folders:

- **SCCMContentLib\***
    - The location the content for the ConfigMgr apps will be published

- **WSUSContent\***
    - The location the content for the WSUS updates will be published

> **Note:** The  folder is specified in the registry key **HKEY\_LOCAL\_MACHINE\\Software\\Microsoft\\Update Services\\Server\\Setup:ContentDir**

More info on the server-side exclusions Microsoft recommends for **ConfigMgr** can be found [here](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/endpoint-protection/recommended-antivirus-exclusions) and WSUS can be found [here](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/configure-server-exclusions-microsoft-defender-antivirus?view=o365-worldwide#windows-server-update-services-exclusions).

### Client-Side

When it comes to clients, these folders should be excluded from antivirus scans:

- **C:\\Windows\\ccmcache\\\***
    - The SCCM cache folder stores temporary software packages for application execution

- **C:**\\**Windows**\\**Setup**\\**Scripts\***
    - The location for custom scripts during Windows installation process

 More info on the client-side exclusions Microsoft recommends for ConfigMgr can be found [here.](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/endpoint-protection/recommended-antivirus-exclusions#folder-exclusions-for-clients)

## Intune

For Win32 apps, Microsoft suggests excluding these folders from antivirus actions on the client side:

On x64 client machines:

- - **C:**\\**Program Files (x86)**\\**Microsoft IntuneManagement Extension**\\**Content**\\**\***
        - The location content is staged in and detection scripts are executed from.
    
    - **C:**\\**Windows**\\**IMECache**\\**\***
        - The location installers are executed from.

On x86 client machines:

- **C:**\\**Program Files**\\**Microsoft Intune Management**\\**Content**\\**\***
    - The location content is staged in and detection scripts are executed from.

- **C:**\\**Windows**\\**IMECache**\\**\***
    - The location installers are executed from.

For more information on the AV exclusions, see [this Microsoft Docs page](https://learn.microsoft.com/en-us/mem/intune/protect/endpoint-protection-windows-10#attack-surface-reduction-exceptions).

## Honorable mentions

This section highlights folders identified by Patch My PC engineers that contain files related to Patch My PC-published applications and updates. If these folders are blocked by antivirus or security software, that may cause issues.

Since there is no Microsoft documentation that explicitly mentions that these directories need to be whitelisted, please proceed with caution if you need to whitelist them.

**ConfigMgr / WSUS server-side**

- **_WSUS\_ContentDir_**\\**UpdateServicesPackages**\\**\***
    - The location 3rd party update content is staged before the cab is copied to the WSUSContent folder

- **%ProgramFiles%**\\**Update Services**\\**LogFiles**\\**WSUSTemp**\\**\***
    - The location of the staging area for signing cab files for 3rd party content

**ConfigMgr / WSUS client-side**

- **C:**\\**Windows**\\**CCM**\\**SystemTemp**\\**\***
    - This folder stores PowerShell detection scripts used in ConfigMgr Apps packaged by the Patch My PC Publisher.
    
    - The ConfigMgr agent runs these PowerShell scripts from this location.  
        Additionally, unrelated to Patch My PC, PowerShell scripts associated with Configuration Items or Baselines are also executed from this directory.

- **%windir%**\\**SoftwareDistribution**\\**Datastore**\\**\***
    - The location for metadata for Windows Server Update Services clients

- **%windir%**\\**SoftwareDistribution**\\**Download**\\**\***
    - The location for update content delivered via Windows Server Update Services