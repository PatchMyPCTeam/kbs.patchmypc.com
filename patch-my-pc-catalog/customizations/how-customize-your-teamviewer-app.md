---
title: "How to customize your TeamViewer App or Update"
date: 2022-04-08
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

Many applications have configuration options available where you can pass parameters, transformation files, or configuration files, to the installer in order to set user defaults, configuration, or install license keys. This article will provide instructions on configuring applications or updates using the Patch My PC Publisher using the right click options **[Modify command line](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#modify-command-line)**, [Add MST transformation file](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#mst-transform), and **[Add custom pre/post scripts](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#custom-scripts)**.

In order to understand how software can be customized via command line, mst file, or configuration file, please refer to the software vendor’s documentation.

**Scenarios:**

For this guide, we will configure the TeamViewer full client for mass deployment on Windows Devices using the Patch My PC Publishing Tool.  
We will cover the following scenarios:

- **[Scenario 1 - Pass Parameters to the MSI/EXE installer](#topic1)**

- **[Scenario 2 - Add a transformation file](#topic3)**

- **[Scenario 3 - Pass a custom TeamViewer Options File (\*.tvopt) to the installer](#topic4)**

> **Note:** This KB article applies to the TeamViewer MSI installer.  
> The TeamViewer MSI installer cannot be downloaded automatically using the Patch My PC Publishing tool, as it is not publicly available for download. You will have to manually download the installer and place it in the **[local content repository](https://patchmypc.com/local-content-repository-for-licensed-applications-that-require-manual-download)**.

## Scenario 1: Pass parameters to the TeamViewer installer

1\. Right click your TeamViewer app or update from within the Patch My PC Publisher and select the **[Modify command line](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#modify-command-line)** option:

![](/_images/1-manage-custom-script-full2.png)

2\. Add your desired arguments here.

![](/_images/2-add-custom-parameter-V2.png)

If you are unsure what parameters to enter here, please refer to **[TeamViewer’s documentation](https://community.teamviewer.com/English/kb/articles/39639-mass-deployment-on-windows)** (or the software vendor’s documentation if you’re following along and not working with TeamViewer).

> **Note:** Any arguments you add here will be appended to the default arguments we apply. You can see what arguments we apply by default by right clicking on a product and choosing [Show package info: title, command-line, download URL, etc.](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#PackageInfo)

3\. Synchronize the Patch My PC Publisher

- For Intune Apps/Updates, please delete the package in Intune before syncing

- For everything else, syncing the Publisher alone is sufficient

4\. Once the deployment is received on an endpoint, you will see the below log lines in PatchMyPC-ScriptRunner.log.

![](/_images/2_1-log-file-Copy.png)

## Scenario 2: Add a transformation file

1\. Right click your TeamViewer app or update from within the Patch My PC Publisher and select the [Add MST transformation file](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#mst-transform) option.

![](/_images/3-Select-MST-Option2.png)

![](/_images/3_1-select-MST-file2new.png)

2\. Synchronize the Patch My PC Publisher

- For ConfigMgr/WSUS software updates, you must [republish the update](https://patchmypc.com/when-and-how-to-republish-third-party-updates).

- For ConfigMgr Apps, syncing the Publisher alone is sufficient.

- For Intune Apps/Updates, please delete the package in Intune before syncing.

3\. Once the deployment is received on an endpoint, you will see the below log lines in PatchMyPC-ScriptRunner.log and content in ccmcache / IMECache / SoftwareDistribution.

![](/_images/3_2-scriptrunner-log-file-Copy.png)

![](/_images/3_3-ccmcache-content2new.png)

## Scenario 3: Pass a custom TeamViewer Options File (\*.tvopt) to the installer

For this scenario, we will need to use a combination of the **[Modify command line](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#modify-command-line)** option (see [Scenario 1](#topic1)), and the **[Add custom pre/post scripts](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#custom-scripts)** option to include the options file within the package.

1\. Firstly, let’s select the TeamViewer options file, and add it. Right-click the desired TeamViewer version, and select the [Add custom pre/post scripts](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#custom-scripts) option.

![](/_images/4-tvopt-main-screenshot2.png)

2\. In the **Additional Files** section, browse and select your file, then confirm with **OK**. You don’t need to add anything else here.

![](/_images/4_1-choose-the-tvopt-file.png)

3\. Now follow the steps from [Scenario 1](#topic1) to add the custom parameter for specifying TeamViewer Options files. Here we will make use of the %CurrentDir% special variable within the Patch My PC Publisher in order to provide a valid path to the .tvopt file during installation.

![](/_images/4_2-command-line-tvopt-file.png)

4\. Synchronize the Patch My PC Publisher

- For ConfigMgr/WSUS software updates, you must [republish the update](https://patchmypc.com/when-and-how-to-republish-third-party-updates).

- For ConfigMgr Apps, syncing the Publisher alone is sufficient

- For Intune Apps/Updates, please delete the package in Intune before syncing

5\. Once the deployment is received on an endpoint, you will see the below log lines in PatchMyPC-ScriptRunner.log and content in ccmcache / IMECache / SoftwareDistribution.

![](/_images/4_3-log-file-Copy.png)

![](/_images/4_4-cache-contentnew.png)