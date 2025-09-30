---
title: "What Are Metadata-Only Updates and How to Download and Deploy Metadata-Only Updates? (Blue Icons)"
date: 2020-09-22
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
        - education
        - best-practices        
---

In this article, we will cover the topic of [metadata-only updates](https://docs.microsoft.com/en-us/mem/configmgr/sum/understand/software-updates-icons#metadata-only-icon). This is a useful feature that is available in WSUS, and subsequently Configuration Manager.

## 
![](/_images/metadataonly.png)
 What is a Metadata-Only Update (Blue Icon)

To clarify what metadata-only updates are, let's first discuss what the metadata is for updates.

The metadata for updates in WSUS contains all **the information about an update**, and they appear in the console with a blue icon next to them.

> **Note:** A **metadata only update will not show up in a ConfigMgr Automatic Deployment Rule (ADR) Preview for updates**. This is because they are not deployable and would not be processed by the ADR.

##  **WSUS Update Metadata** includes information such as:

- Title

- Description

- Update Severity

- Update Classification

- Superseded Updates

- Applicability Rules
    - IsInstalled
    
    - IsSuperseded
    
    - IsInstallable

- Command Line

- Return Code Behavior

> **Note:** The above is not a complete list of what is included in update metadata, but provides a summary of important attributes included. 

Knowing the above information about **WSUS metadata**, a metadata-only update is simply **an update with no content, only metadata.** 

This means the **metadata-only updates will not have the content downloaded to the 'UpdateServicesPackages' or 'WSUSContent' directories** which WSUS manages. As there is no content the publishing action for an update as metadata-only will be very quick! There is no binary to codesign. But, **because this content does not exist the update cannot be 'downloaded' within the Configuration Manager console**, as it is a metadata-only update.

Below is an example of an update that is published with full content. Note that we do see our applicability based metadata, required, installed, not required. More importantly, **this update has a content source path**.

![](/_images/Update_FullContent.png)

We can look at the same set of information below for an update that is **published as metadata-only**.

You can determine if updates are **metadata-only** from the **All Software Updates** node if the updates are displayed with a **blue icon**. Aside from the icon, the '**Metadata Only**' column is marked with a 'Yes' and we can also see the **Content Information tab shows no source path for the update**. This is the key difference for metadata-only updates.

![](/_images/Update_MetadataOnly.png)

Attempting to download an update that is metadata-only will present an error message box stating '**All software updates in this selection are expired or meta-data only, and cannot be downloaded.**'

![](/_images/Err_DownloadMetadataOnly.png)

> **Note:** Updates **do not need to be deployed** to receive applicability statistics such as required, installed, and not required. The update only needs to be in the WSUS catalog as metadata-only, or full content.

## Manually Download By Publishing

One method of publishing an update as full content is to manually '[Publish Third-Party Software Update Content](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#publish-and-deploy-third-party-software-updates)' as shown below.

![publish third-party update with full-content from metadata-only](/_images/publish-third-party-update-with-full-content-from-metadata-only.png "publish third-party update with full-content from metadata-only")

You can review the full-content publishing progress in the [SMS\_ISVUPDATES\_SYNCAGENT.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SU_NAPLog). The log is located on the top-level software update point in the site system Logs folder.

Once the publishing of third-party update content is complete, you can [sync your software update point](https://docs.microsoft.com/en-us/mem/configmgr/sum/get-started/synchronize-software-updates#manually-start-software-updates-synchronization) for SCCM to pick up the change from metadata-only to full-content faster.

![sync sccm software update point for declined updates](/_images/sync-sccm-software-update-point-for-declined-updates.png "sync sccm software update point for declined updates")

Once the software update point sync is complete, the third-party updates should show in a [normal state](https://docs.microsoft.com/en-us/mem/configmgr/sum/understand/software-updates-icons#normal-icon) with a green icon meaning they are available for software update deployment.

![third-party updates in normal state](/_images/third-party-updates-in-normal-state.png "third-party updates in normal state")

## Automatically Download Via Stage Content (v3 catalog, ConfigMgr 1910+)

Microsoft has made many improvements to the built-in third-party patching features in Configuration Manager. Most notably, starting in **ConfigMgr 1910** you can [set a custom schedule for catalogs](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#set-the-schedule-for-a-catalog-in-a-new-catalog-subscription), [set a schedule per catalog](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#update-the-schedule-per-catalog), and full support for the [version 3 catalog](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#new-subscription-to-a-third-party-v3-catalog) for updates.

The key improvement with the version 3 catalog is the ability to **select individual categories to synchronize**. This is key to ensuring the WSUS catalog is kept to a minimal number of updates. The greater the number of updates in a WSUS infrastructure, the more resources are required on the server, and the more resources are used when a client performs a Windows Update scan.

To use these features, '**Subscribe to Catalog**' on the desired third-party software update catalog. This will bring up a wizard that is shown below.

![](/_images/v3_Subscribe.png)

Within the '**Select Categories**' of the wizard, the list of categories provided by the vendor is listed. With the 'Select categories for synchronization' option selected it is possible to specify any number of categories to select. This allows only updates that are of interest to synchronize instead of the entire catalog.

![](/_images/V3_SelectCategories.png)

Beyond just select certain categories for synchronization, specific categories can also be selected such that ConfigMgr will '**Stage the content for the selected categories automatically**.'

When the updates are automatically staged for a category, any new updates will be **published with full content** and automatically downloaded into the WSUSContent directory according to the schedule associated with the catalog. This removes the manual step of selecting updates publishing them.

![](/_images/V3_StageCategories.png)

## Automatically Publish Updates as Full Content Using the Publisher

When using the Patch My PC Publisher, the option is provided to select if an update is published as full content, or metadata-only. **By default, all updates will be published as full content**, but it is very simple to [right-click any update](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#publishing-type), vendor, or the 'all products' node in the publisher and select to publish as metadata only.

![](/_images/Publisher_FullContentMetadataRightClick.png)

The behavior is otherwise identical. Updates will publish into the console as expected according to the configuration specified in the Publisher.

> **Note:** If you are publishing updates using the Publisher, **only use the Publisher to change from metadata-only to full content**. If you have previously published an update as metadata-only, you can open the Publisher back up, right-click the update, and select full content publishing. The next synchronization will ensure the update is published with full content.