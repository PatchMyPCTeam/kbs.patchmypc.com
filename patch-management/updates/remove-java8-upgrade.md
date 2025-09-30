---
title: "Remove Previous Versions of Java 8 Runtime During Upgrade"
date: 2019-04-21
taxonomy:
    products:
        - 
    tech-stack:
        - 
    solutions:
        - patch-management
    post_tag:
        - 
    sub-solutions:
        - updates
        - workarounds
        - patching
---

This article will describe your options for **uninstalling old versions of Oracle Java** while updating Java with Patch My PC.

## Video Guide for Java Runtime Environment Uninstall Process

Please review the step-by-step video guide for information on how to add the JRE removal during the update as a pre-update script.

_**Note:**_ The video shows the process of manually including pre-update scripts to remove JRE.  Skip this video and read **OPTION 1** for the Patch My PC defined scripts.

<iframe src="https://www.youtube.com/embed/ywOGLlJMpx0" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>

## OPTION 1: Using Patch My PC Defined Scripts

The Patch My PC defined scripts will allow you to automatically uninstall all versions of **Java Runtime Environment 8 (32-Bit)** and **Java Runtime Environment 8 (64-Bit)**. The defined scripts will appear as a right-click option in the Update Rules tab.

For example, by default, the install for **JRE 8 (32-bit)** installations will remove older versions of Oracle Java 8 (x86) prior to installing the latest update. To view this script, right-click the **Oracle Java 8 (x86)** product in our [publishing services](https://patchmypc.com/publishing-service-setup-documentation) Update Rules tab and choose Patch My PC defined pre/post update installation scripts.

![](/_images/java-uninstall-patchmypc-definied-script-3.png)

The script will automatically be inserted into the Pre-Update Script box.  This option is also available for **Oracle Java 8 (x64)** and **Oracle Java SE Development Kit 8 (x64)/(x86)**.

![java removal patchmy pc recommended script pre-update script box](/_images/java-patchmypc-recommended-script.png "java removal patchmy pc recommended script pre-update script box")

## OPTION 2: Add REMOVEOUTOFDATEJRES=1 as Custom Command Line

This option is **NOT** recommended if your environment uses **both the (32-bit) and (64-bit) versions of JRE 8** on **Windows 64-bit operating systems**. The reason we don't recommend it in this scenario is that the [REMOVEOUTOFDATEJRES=1](https://docs.oracle.com/javase/8/docs/technotes/guides/install/config.html#table_config_file_options) parameter will automatically **remove both (32-bit) and (64-bit) JRE 8 installs regardless of the version being updated**. This causes the other version to become not applicable as a software update, therefore, leaving only one installed version where a machine previously would have had both versions installed.

This option should **only be used if you are only running a single JRE 8 architecture within your environment**. For example only if you use the (32-bit) exclusively or you only use the (64-bit) exclusively.

To use this method to remove previous versions during the latest JRE 8 update, right-click the **Oracle Java 8 (x86) /** **Oracle Java 8 (x64)** and choose to **Modify command line for product**.

![](/_images/modify-jre8-command-line.png)

In the input box for the additional argument add **REMOVEOUTOFDATEJRES=1**

This command line should have the **JRE 8** installer automatically remove previous versions using Oracle's **[native installer](https://docs.oracle.com/javacomponents/msi-jre8/install-guide/use-installer-configuration-file-install-jre.htm#JSMSI-GUID-1430E70F-0E4F-4A8B-B058-21D5901BB715)**.

_**Note:** Neither of these options is supported if you are using the SCCM 1806+ in-console publishing option rather than our publishing service._