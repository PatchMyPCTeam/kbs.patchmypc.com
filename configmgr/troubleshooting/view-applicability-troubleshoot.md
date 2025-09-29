---
title: "View Applicability Rules and Troubleshoot Detection States for Third-Party Updates"
date: 2020-01-26
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

Viewing the **applicability rules** of a **third-party update** can help determine why an update does not appear in software center or install within SCCM.

This article looks at how the **update applicability** is determined for **third-party updates** within our catalog.

This guide will be helpful to determine why third-party software updates may not install on devices or report as required in Configuration Manager.

![SCCM Statistics Chart for a Third-Party Software Update](images/Third-Party-Software-Updates-SCCM-Detection-States.png)

### Step 1: Understand the Different Software Update States

Here's a review of the **states** available for software updates within Configuration Manager and WSUS.

**Compliant** = The software update has been detected as being installed.  
**Required** = The software update has been detected as being needed, but it hasn't been reported as installed.  
**Not Required** = The software update has been detected as not being applicable. This status is what we will be looking deeper into here.  
**Unknown** = The software update hasn't been evaluated yet on the client.

### Step 2: Determine the Detection State of a Software Update on All Systems

When you are troubleshooting why a third-party software update isn't appearing in software center or installing on a specific device or a large number of devices, the best first step is to click the update within the **All Software Updates node** and review what is the **overall detection** for all systems.

![SCCM Detection State Chart for a Third-Party Software Update](images/All-Device-Third-Party-Update-Detection-State.png)

The overall detection results can be helpful in understanding if the detection seems incorrect for all systems or if only a few devices seem not to be detecting the update state correctly.

For example, if a large number of devices have an **Unknown** detection state, it's likely an issue where the WSUS Administration IIS application pool is stopped, or the clients have a WSUS GPO conflict causing machines not to scan against the software update successfully.

### Step 3: Determine the Detection State of a Software Update on a Specific Computer

If you are troubleshooting a specific computer not receiving a third-party update, run the following built-in report within Configuration Managers console under **Monitoring** > **Reporting** > **Reports** > **Software Updates - A Compliance** > **Compliance 5 - Specific computer**.

![Compliance 5 Specific computer compliance state](images/Compliance-5-Specific-computer.png)

This report will allow you to review if the software update is detecting as **Installed** or **Required** on the specific computer in question.

### Step 4: Determine the Detection State of a Software Update on a Device Collection

Next, you can expand the search to review the detect states on a **specific collection of devices**. For example, you could choose a collection where the software update has been deployed. From the Configuration Managers consol navigate to **Monitoring** > **Reporting** > **Reports** > **Software Updates - A Compliance** > **Compliance 2 - Specific software update**.

![Compliance 2 - Specific software update](images/Compliance-2-Specific-software-update.png)

Review the different **detection states** for the **specific software update** in question.

### Step 5: Check the Software Update Group's Deployment Completion Statistics

If the detect state looks correct and the update(s) in question appear as required,  the next step is to review the deployment completion statistics from the Deployments node of the console.

From the Configuration Managers console navigate to **Monitoring** > **Deployments** \> **Select the Software Update Group's Deployment**, and review the **Completion Statistics** chart.

![Deployment Completion Statistics SCCM Third-Party Update](images/Deployment-Complention-Statistics-SCCM-Updates.png)

Review the different **states** that devices are in for the deployment. If you have a large number of devices in an **In Progress** or **Failed** state, this could explain why machines aren't installing the software update. Click the **View Status** link below the chart to review a more detailed view of devices in specific states.

![View Status Software Update Deployment SCCM Console](images/View-Status-Software-Update-Deployment-SCCM.png)

For example, if a large number of devices are in an **In Progress** state, it could be because the software update deployment package isn't fully distributed or boundaries and boundary groups aren't configured correctly for all clients.

### Determining Software Update Applicability Rules when the State is Reporting Inaccurately

If the software update is detecting as **Not Applicable** on a device where it should (An **older version of the product is installed**), you can review the detection rules of the software update using one of the processes below.

### Modify Updates Wizard

One way to get the applicability rules for the software update **Notepad++ 8.1.3 (x64)** is to launch the **Modify Published Updates** wizard in the **Options** menu of the **Updates** tab.

![Run Modified Update Wizard in Publisher](images/Modify-Published-Updates-Wizard2.png)

Select the update that isn't being detected as Required and click **Show Applicability Rules**.

![Show WSUS Applicability Rules Third-Party Update](images/Show-Applicability-Rules-Tab2.png)

### Show Package Info

Another way to view applicability rules for an update is by using the '[Show package info...](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#PackageInfo)' right-click option. Once you have selected **show package info from the right-click menu** you can **double-click** on any given product listed and view the applicability rules.

![Double Click on product in 'Show Package Info' to see applicability rules](images/Show-Applicability.gif)

> **Note:** When you double click a product from the 'Show Package Info' tool you are querying the **latest catalog** for applicability rules. When you select an update in the Modify Updates wizard you are viewing the **applicability rules for the selected update**. 

In this wizard, you can review the rules determined to show if an update will be detected as required on a client machine in the **IsInstallable Rules** section.

![Show IsInstallable WSUS Applicability Rules Third-Party Update](images/NotepadIsInstallableRules.png)

For the update **Notepad++ 8.1.3 (x64)** to show as required, the following registry conditions need to exist:

1. HKEY\_LOCAL\_MACHINE Subkey="SOFTWARE\\Microsoft\\WindowsCurrentVersion\\Uninstall\\Notepad++:DisplayVersion" exis

3. HKEY\_LOCAL\_MACHINE Subkey="SOFTWARE\\Microsoft\\WindowsCurrentVersion\\Uninstall\\Notepad++:DisplayVersion" < "8.1.3.0"

On a client machine, you can then review if the rules would evaluate.

![Example Registry Value WSUS Applicability Rules Third-Party Update](images/Notepad-Registry-Value2.png)
