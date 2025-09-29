---
title: "Failed to set Subscriptions on the WSUS Server. Error:(-2147467259)Unspecified error"
date: 2019-11-26
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

In this video KB article, we will be reviewing an issue that can happen when a third-party vendor has been removed from the WSUS products list, but the product is still enabled in the software update points products list.

## Determine if You are Affected

In the [WCM.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SU_NAPLog), on the site server, you will see an error message similar to the **log lines below**:

Category Product:e244170c-ed31-e94b-172a-fcc94c0541ea (%productname%) not found on WSUS  
Subscription contains categories unknown to WSUS.  
Failed to set Subscriptions on the WSUS Server. Error(-2147467259)Unspecified error

In the **wsyncmgr.log**, on the site server, you will see an error message similar to the message below:

Sync failed: WSUS server not configured. Please refer to WCM.log for configuration error details.. Source: CWSyncMgr::DoSync

## Why Error(-2147467259)Unspecified error Occurs

Before reviewing the resolution, we want to make sure you understand why this error occurs in the first place. The error occurs in the [WCM.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SU_NAPLog). The WCM component _records details about software update point configurations and connections to the WSUS server for subscribed update categories, classifications, and languages_ based on the **[Microsoft Docs](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SU_NAPLog)**.

If you **view the log file**, you will actually see more details on the issue

![Failed to set Subscriptions on the WSUS Server. Error(-2147467259)Unspecified error](/_images/Failed-to-set-Subscriptions-on-the-WSUS-Server-Error-2147467259Unspecified-error.png "Failed to set Subscriptions on the WSUS Server. Error(-2147467259)Unspecified error")

The root cause is one of the products for third-party vendors was r**emoved from WSUS**, but it's **still enabled in the software update point**. One common reason products may be removed from WSUS is when **[Software update maintenance](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/software-updates-maintenance)** runs and then the WSUS cleanup wizard is run. Unfortunately, **ConfigMgr will not automatically un-check products that don't exist in WSUS** from the software update point products.

## Resolution for Error(-2147467259)Unspecified error (Post Version)

To fix this error, you will need to un-check all products that don't exist in WSUS from the **Product** tab in the **Software Update Point Component Properties**.

Based on the error in our example log, we can see the vendor **Adobe Systems, Inc.** and **Notepad++** Team no longer exist in WSUS. In our example, we would un-check these products in our software update point.

![Error(-2147467259)Unspecified error fix](/_images/Error-2147467259Unspecified-error-fix.gif "Error(-2147467259)Unspecified error fix")

Once un-checked monitor the [WCM.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SU_NAPLog) to review if the error is resolved.

## Resolution for Error(-2147467259)Unspecified error (Post Version)

Video resolution guide for this error.

<iframe src="https://www.youtube.com/embed/18--gYhar30" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>

If the missing product **doesn't appear in the products** of the software update point, please see the following blog post: **[Case Study - ConfigMgr 2012: Moved the SUP role with a new WSUS server (new DB) and now the WCM fails to sync.](https://techcommunity.microsoft.com/t5/configuration-manager-archive/case-study-configmgr-2012-moved-the-sup-role-with-a-new-wsus/ba-p/339918)**