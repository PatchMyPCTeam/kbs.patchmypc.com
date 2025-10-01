---
title: "Troubleshooting Why Third-Party Updates Don't Show in SCCM"
date: 2019-08-28
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - configmgr
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - troubleshooting
        - common-issues-and-error-codes
        - patching
---

In this article, we will review the process to troubleshoot why **third-party software update(s) may not appear in the SCCM** console All Software Updates node.

It's essential to understand the flow of how third-party updates sync between **WSUS** and **SCCM**.

## Step 1: Validate Third-Party Update(s) are Publishing to WSUS

Before any third-party updates can sync into SCCM, they need to be **published to WSUS**. The publishing operating can be performed using the following methods:

- [Patch My PC's Publisher](/publishing-service-setup-documentation)

- [SCCM's Third-Party Software Update Catalogs Feature](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates)

- [System Center Updates Publisher (SCUP)](https://docs.microsoft.com/en-us/mem/configmgr/sum/tools/updates-publisher)

NOTE: When using our publishing service or the third-party software update catalog feature in SCCM, the publishing of third-party updates to WSUS is performed by a **background service**. If an update fails to publish to WSUS, you will need to review the **[SMS\_ISVUPDATES\_SYNCAGENT.log](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#publish-and-deploy-third-party-software-updates)** if using the third-party software update catalog feature in SCCM. If using the Patch My PC [Publisher](/docs), you can review the **[PatchMyPC.log](/frequently-asked-questions#log-files)** in the installation directory.

If there are no errors in the logs, the next step is to **review if third-party updates exist in WSUS**. Since the WSUS doesn't directly expose third-party software updates in the WSUS console, you will need to use our [publisher](/publishing-service-setup-documentation).

In the **Updates** tab of our publishing service, you can select **Options** and run the **Modify Published Updates** wizard to view what updates are published to WSUS.

![](/_images/troubleshooting-updates-KB-1.png)

![](/_images/troubleshooting-updates-KB-2-2.png)

If the third-party updates that aren't appearing in the SCCM console **do exist in WSUS**, proceed to the next troubleshooting steps.

## Step 2: Validate Software Update Point Synchronizes from WSUS

If the third-party updates were published successfully to WSUS, the next step in the process that could cause and issue is the synchronization between WSUS and SCCM.

We recommend performing a manual **synchronization** of your software update point within the SCCM console.

![sync sccm software update point for declined updates](/_images/sync-sccm-software-update-point-for-declined-updates.png "sync sccm software update point for declined updates")

Once the sync has started, you should review the [wsyncmgr.log](https://docs.microsoft.com/en-us/sccm/core/plan-design/hierarchy/log-files#BKMK_SUPLog) using the CMTrace log viewing tool. You will want to verify in the log file the sync succeeded.

![wsyncmgr log file verify successful](/_images/wsyncmgr-log-file-verify-successful.png "wsyncmgr log file verify successful")

In the Configuration Manager console under **Monitoring** > **Software Update Point Synchronization,** you can also verify if the last sync was successful.

![verify software update point sync completed](/_images/verify-software-update-point-sync-completed.png "verify software update point sync completed")

If the software update point synchronization is failing, you will need to [resolve the sync issue](https://support.microsoft.com/en-us/help/4505439/troubleshoot-software-update-synchronization-in-configuration-manager) before any third-party software updates will show up in the SCCM console.

## Step 3: Verify the Patch My PC Vendor is Enabled in the Software Update Point Products

In your SCCM console, you will need to ensure **Patch My PC** vendor and **SCUP Updates** product is enabled in the Software Update Point Products.

Within the SCCM console, navigate to **Administration** > **Site Configuration** > **Sites** > **Configure Site Components** > **Software Update Point**

![software update point products patch-my-pc](/_images/software-update-point-products-patch-my-pc.png "software update point products patch-my-pc")

If the product shown above is not enabled, you need to enable the product, click Apply then running another Software Update Point synchronization should resolve the issue for third-party software updates not appearing in SCCM.

> **Important:** If you see **multiple Patch My PC Products** and one is not checked, please see the following article **[Duplicate Patch My PC Category Listed in Software Update Point Products Tab](https://patchmypc.com/duplicate-patch-my-pc-category-listed-in-software-update-point-products-tab)**

## Step 4: Verify Classifications are Enable in the Software Update Point

If updates have been published to WSUS and there are no synchronization issues between WSUS and SCCM, you may not have the required [update classifications](https://support.microsoft.com/en-us/help/824684/description-of-the-standard-terminology-that-is-used-to-describe-micro) enabled in SCCM.

For our third-party updates, we will use **Critical, Security Updates, and Updates** classifications depending on what best matches the release of a specific third-party software update.

Within the **modify published updates wizard**, you can see what classification third-party updates are classified as.

![third-party software update classifications](/_images/third-party-software-update-classifications.png "third-party software update classifications")

If an **update classification isn't enabled in the Classifications** tab of the software update point, the update will not appear in SCCM unless that classification is enabled and another software update point synchronization is performed.

In the screenshot below, we can see the Updates classification isn't enabled, and that would cause many third-party software updates to not appear in SCCM.

![sccm software update point classifications](/_images/sccm-software-update-point-classifications.png "sccm software update point classifications")