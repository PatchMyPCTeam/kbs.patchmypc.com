---
title: "Third-Party Updates Fail with Error 0x80246002 in ConfigMgr (SCCM)"
date: 2020-12-18
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

Error **0x80246002** occurs when installing updates on a client in Configuration Manager (SCCM) when there is a **hash validation issue**. This article will review why **0x80246002** may happen and the steps to take to troubleshoot and hopefully resolve the issue.

## Determine if You are Affected

If affected, you will see **errors similar to the errors below**:

In the **[WUAHandler.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#update-troubleshooting-client-logs)** file:

Failed to download updates to the WUAgent datastore. Error = 0x80246002.

In the **[WindowsUpdate.log](https://docs.microsoft.com/en-us/windows/deployment/update/windows-update-logs)** file:

Hash check on file C:\\Windows\\SoftwareDistribution\\Download.cab using algorithm SHA256 failed; hash values did not match.

If the update is set to be **visible in software center**, the error show a message similar to below:

![Update Error Hash for 0x80246002](images/Update-Error-Hash-for-0x80246002-1.png)

## Background of Windows Update Content Hash Validation Checking

To understand **how hash validation** works and update content is transferred to clients, we will need to dig into the software update content flow, and where things could go wrong.

1. **Windows Server Update Services (WSUS) -> Windows updates**
    - In this step, your WSUS service will **sync the latest updates available from Microsoft updates** or **locally published third-party software updates**.
    
    - The update file hashes are stored in the WSUS database and this is the authoritative source.

3. **ConfigMgr Updates -> WSUS**
    - In the next step, ConfigMgr will sync its software update database from the latest WSUS database. This sync is tracked in the **[wsyncmgr.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SUPLog)**.

5. **ConfigMgr Update Deployment**
    - Updates are downloaded to a deployment package, distributed, and deployed to clients via ConfigMgr.

7. **Clients Install Updates from ConfigMgr**
    - Client computers will download the updates from distribution points to **C:\\Windows\\CCMcache** and copy the update binaries to **C:\\Windows\\SoftwareDistribution\\Download**. Next, the ConfigMgr agent will call the **Windows Update Agent** to perform the actual update installation.
    
    - The **Windows Update Agent** will then compare the update binary hash for the **update binary download from your ConfigMgr distribution point** against the **file hash stored in the WSUS database**.

In short, error **0x80246002** means the update binary file hash downloaded from the ConfigMgr distribution point doesn't match the expected value from the update in the WSUS database.

> **Note:** The hash error **0x80246002** is an issue that **isn't directly caused by Patch My PC** or any configurations related to Patch My PC. We will try to cover some common reasons that could cause hash issues and possible workarounds.

## Possible Cause 1: Corrupt Deployment Package Content

One of the more obvious root causes is a corrupted **[deployment package](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/manually-deploy-software-updates#process-to-download-content-for-the-software-update-group).** The deployment package is where the **update binaries are downloaded** from **Windows updates** (or from the WSUS server for third-party updates). The deployment package is then used as the source for how the updates are distributed to distribution points.

If the update content is ever **corrupted during this process**, clients' binary downloaded will fail with a hash error. A good first step is trying to **delete problematic updates** from the ConfigMgr **deployment package(s)**.

![Delete updates from deployment package](images/Delete-updates-from-deployment-package.png)

You can then **delete the problematic updates** from the deployment package.

![Delete specific update from a deployment package in ConfigMgr](images/Delete-specific-update-from-a-deployment-package-in-ConfigMgr.png)

We recommend **waiting up to one hour** to ensure the deletion of the updates from the **deployment package source folder** and the **content library**. After waiting, you can then try to **re-download the update(s)** to a deployment package and verify the deployment package is distributed.

## Possible Cause 2: Updates Deleted from the WSUS Database and Published as Same UpdateID

If you **[deleted updates directly from WSUS](https://patchmypc.com/modify-published-third-party-updates-wizard#topic3)**, and the product is still enabled, this can cause hash errors.

**Note:** The delete update button **is not enabled by default** because it can cause hash issues if an update is deleted, and that product is still enabled, causing the same UpdateID to be republished. 

![Delete Updates from WSUS for Third-Party Updates](images/Delete-Updates-from-WSUS-for-Third-Party-Updates.png)

If you **delete an update** and **the product is still enabled**, it will publish again using the **same UpdateID**. This is problematic because the **hash of the update will be updated in the WSUS database**, but those **hash changes will not sync to the ConfigMgr database** based on our simulations.

This means the ConfigMgr client will **download the binary from the original update** before it was published again. However, the Windows Update Agent will evaluate against the update published second, causing error **0x80246002**.

If facing this scenario, we recommend republishing the affected third-party updates as a new update using the process **[Republishing Third-Party Updates to Create New Updates and UpdateID's](#topic6)**

## Possible Cause 3: Was the Top-Level WSUS/SUP Server Rebuilt?

If the WSUS server was **completely rebuilt**, it could cause issues similar to scenario 2, where the new update published with the **same UpdateID to WSUS** may not sync over the new hash and file to the ConfigMgr database.

If facing this scenario, we recommend republishing the affected third-party updates as a new update using the process **[Republishing Third-Party Updates to Create New Updates and UpdateID's](#topic6)**

## Republishing Third-Party Updates to Create New Updates and UpdateID's

We have a knowledge base article dedicated to the process of republishing third-party updates using our Publisher here **[When and How to Republish Patch My PC Third-Party Updates](https://patchmypc.com/when-and-how-to-republish-third-party-updates)**
