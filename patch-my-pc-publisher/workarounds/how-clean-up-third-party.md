---
title: "How to Clean Up Third-Party Updates from the WSUS UpdateServicesPackages Folder"
date: 2020-01-21
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

This article and video will describe the steps required to clean up obsolete/declined updates from the WSUS Database and how to properly remove the content folders for these updates from the **UpdateServicesPackages** folder.

## What is the WSUS Content Folder

The **WSUSContent** folder is the folder where published third-party updates (.CAB files) are stored. The **WSUSContent** folder maps to the **Content** virtual directly in the **WSUS Administration** website. The **Content** virtual directory is where the updates are downloaded by clients or downloaded to a Configuration Manager deployment package.

![WSUSContent Directory](/_images/WSUSContent-Folder.png "WSUSContent Directory")

If you review the **Content** virtual directory in IIS, you can see how it maps to the WSUSContent directory on the file system.

![](/_images/WSUS-WSUSContent-IIS-Virtual-Directory.png)

When reviewing the **Content Information** tab in the properties of a software update in Configuration Manager, you can view the download path for the third-party update from the **WSUSContent** folder.

![](/_images/Update-WSUSContent-Download-Location-From-WSUS.png)

If you enabled the [WSUS Maintenance](https://docs.microsoft.com/en-us/configmgr/sum/deploy-use/software-updates-maintenance#wsus-cleanup-starting-in-version-1906) feature in Configuration Manager 1906 or newer, older declined update content should be removed automatically.

For a deeper look into the **WSUSContent**, please review the video guide below.

## What is the WSUS UpdateServicesPackages Folder

The **UpdateServicesPackages** folder is used primarily for third-party update publishing operations. It's common for the **UpdateServicesPackages** to become bloated with content from obsolete updates.

Each folder within the **UpdateServicesPackages** corresponds to a third-party update and contains the content downloaded from the vendor and the digitally signed CAB file used for a specific UpdateID.

![](/_images/WSUS-UpdateServicesPackages-UpdateID-Folder.png)

In the folder **93171a38-662f-47a7-92c1-4862cbf16146**, from the screenshot above, you can find the original binaries downloaded from the vendor for the software update.

![](/_images/Downloaded-Third-Party-Update-Content.png)

For a deeper look into the **UpdateServicesPackages** and how to clean up obsolete updates from this directory, please review the video guide below.

## Automatically Cleanup Declined/Deleted Updates from the UpdateServicesPackages Folder

In **[build 2.0.7 of the Publisher](https://patchmypc.com/clean-up-third-party-updates-from-the-wsus-updateservicespackages-folder)**, we added a feature to run the UpdateServicesPackages cleanup automatically based on **[UserVoice idea 797](https://ideas.patchmypc.com/ideas/PATCHMYPC-I-797)**.

When enabled, this feature will **automatically delete any declined third-party updates from Patch My PC** and then **delete any unreferenced update folders** from the UpdateServicesPackages folder.

![](/_images/wsus-options-1.png)

By default, we only delete declined updates for "Patch My PC" updates. If you would like, you can uncheck "**Only delete declined Patch My PC third-party updates.**". When unchecked, we will also delete third-party updates for declined updates of other vendors such as Dell, HP, Lenovo, etc.

You also have the option to **Automatically run the Unneeded files clean action in the WSUS Server Cleanup Wizard.** This will help clean up the WSUS content folder.

> **Note:** If you have downstream WSUS servers that do not share the same SUSDB you may want to disable the automatic deletion and cleanup feature. Update cleanup in a WSUS infrastructure should be performed from the bottom up per bullet 2 in the [Microsoft documentation](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/update-management/wsus-maintenance-guide#important-considerations).

## Video Guide on Manually Deleting Obsolete Update Content from the UpdateServicesPackages Folder

Please review the video guide below for a deep dive into how to properly clean up unreferenced updates from the **UpdateServicesPackages**.

<iframe src="https://www.youtube.com/embed/S3kHKNDShyE" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>