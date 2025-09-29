---
title: "Updates or Applications Stuck at Preparing to download or fail with error 0x87D00669 or 0x87D00607"
date: 2021-03-19
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

This guide covers common reasons clients fail to download updates with the errors "**Preparing to download**", "**0x87D00669**", or "**0x87D00607**" within Configuration Manager.

## Determine if You are Affected

For **software updates failing to download**, you may see the below error in the [UpdatesHandler.log](/collecting-log-files-for-patch-my-pc-support#content-download):

CDeploymentJob -- Failed to download update (121de25b-8ca6-4e50-8a81-b6baf31d9e5d). Error = 0x87d00669

If your software updates are visible in the software center, you may see the **Status column** stuck at **Preparing to download**

![SCCM software update stuck Preparing to download](/_images/SCCM-software-update-stuck-Preparing-to-download.png "SCCM software update stuck Preparing to download")

If the **software update** download times out, you will see the following error: **The software change returned error code 0x87D00669(-2016410007).**

![The software change returned error code 0x87D00669(-2016410007).](/_images/The-software-change-returned-error-code-0x87D00669-2016410007.png "The software change returned error code 0x87D00669(-2016410007).")

If it's an **application** failing to download, you may see the below warning in the **[CIDownloader.log](/collecting-log-files-for-patch-my-pc-support#application-troubleshooting-client-logs)**:

ECCIInfo::SetError - Setting CI level error to (0x87d00607).

If the error is for an **application** and not a software update, the error will be: **The software change returned error code 0x87D00607(-2016410105).**

![The software change returned error code 0x87D00607(-2016410105). Content not found](/_images/The-software-change-returned-error-code-0x87D00607-2016410105-Content-not-found.png "The software change returned error code 0x87D00607(-2016410105). Content not found")

## **Troubleshooting Step 1**: Validate the Content is Successfully Distributed

We see a common issue for clients not to download content because the content is **not distributed** or the **content fails to distribute**.

For **software updates failing to download**, perform the following actions:

**Software Library** > **Software Updates** > **Deployment Packages** > Select the **Deployment Package** the updates is downloaded to:

Here is an example of a deployment package in a **failed state.** If a package failed to distribute, clients would be unable to download the content. If the package is in a failed state, you can use the following resource to resolve the issue: **[Troubleshoot Content Distribution issues - Configuration Manager | Microsoft Docs](https://docs.microsoft.com/en-us/troubleshoot/mem/configmgr/troubleshoot-content-distribution)**.

![SCCM Software Update Deployment Package in Failed State](/_images/SCCM-Software-Update-Deployment-Package-in-Failed-State.png "SCCM Software Update Deployment Package in Failed State")

Below is an example of a **deployment package that is not distributed** to any distribution points. This scenario will also cause clients to be unable to download updates until you [distribute the content](https://docs.microsoft.com/en-us/mem/configmgr/core/servers/deploy/configure/deploy-and-manage-content#bkmk_distribute).

![SCCM Software Update Deployment Package not Distributed](/_images/SCCM-Software-Update-Deployment-Package-not-Distributed.png "SCCM Software Update Deployment Package not Distributed")

For **applications failing to download**, perform the following actions:

**Software Library** > **Application Management** > **Applications** > **Select the Applications** > **Summary tab**

Validate the application content is successfully distributed and not in a **failed or undistributed state**.

![ConfigMgr Application content in failed state on summary tab](/_images/ConfigMgr-Application-content-in-failed-state-on-summary-tab.png "ConfigMgr Application content in failed state on summary tab")

If the content is successfully distributed and you still have issues, please continue the next troubleshooting step.

> **Tip:** There is **no direct way within the Configuration Manager console to view the deployment package(s) a specific software update is a part of**. You will need to right-click the deployment package and click view members to review if the update in question is part of the package.
> 
> If you have many deployment packages and updates, you can run the following **[SQL query](#topic5)** to see the packages every update is a part of.

## **Troubleshooting Step 2**: Check if Client is in Boundary Groups

The next troubleshooting step if the content is distributed is to validate a machine failing to download the content is **within a [boundary group](https://docs.microsoft.com/en-us/mem/configmgr/core/servers/deploy/configure/boundary-groups)**. Since ConfigMgr 2002, the easiest way to check if a client falls within a boundary group is to perform the following actions:

**Assets and Compliance** > **Overview** > **Devices**

Right-click on one of the columns and add the **Boundary Group(s)** column.

![Add Boundary Group(s) column to the Devices node in SCCM](/_images/Add-Boundary-Groups-column-to-the-Devices-node-in-SCCM.png "Add Boundary Group(s) column to the Devices node in SCCM")

The example below shows that DEMO1CLIENT is **within a boundary group**, but DEMO1 is **not within a boundary group**.

![Verify ConfigMgr Device is Within a Boundary Group in the SCCM Console Devices Node](/_images/Verify-ConfigMgr-Device-is-Within-a-Boundary-Group-in-the-SCCM-Console-Devices-Node.png "Verify ConfigMgr Device is Within a Boundary Group in the SCCM Console Devices Node")

After validating a client falls within a boundary group, you will want to perform the following steps to **validate there is a distribution point site system referenced** within that **boundary group**:

![Validate a Distribution Point Site System is Referenced within the Boundary Group](/_images/Validate-a-Distribution-Point-Site-System-is-Referenced-within-the-Boundary-Group.png "Validate a Distribution Point Site System is Referenced within the Boundary Group")

> **Note:** The **boundary group(s) column** was added in Configuration Manager 2002. Both the Configuration Manager server and client need to be upgraded to 2002 for this feature to work. The client version for 2002 is **5.00.8968.1008**

## **Troubleshooting Step 3**: Validate Content is Distributed to Correct Distribution Point(s)

If you are still having issues after the previous 2 steps, you should confirm the content is actually distributed to a **specific distribution point** in a boundary group from step 2.

Perform the following steps to check the specific distribution points a software update deployment package is successfully distributed to:

**Monitoring**ÃÂ > **Distribution Status** \> **Content Status > Select the deployment package**

Right-click on one of the columns and add the **Boundary Group(s)** column.

![View Status of Deployment Package in the Content Status Node of Distribution Status in SCCM](/_images/View-Status-of-Deployment-Package-in-the-Content-Status-Node-of-Distribution-Status-in-SCCM.png "View Status of Deployment Package in the Content Status Node of Distribution Status in SCCM")

Under the **Success** tab, ensure the content has been successfully distributed to a **specific distribution point [referenced in the boundary group](https://patchmypc.com/app/uploads/2025/04/Validate-a-Distribution-Point-Site-System-is-Referenced-within-the-Boundary-Group.png)** from the previous step.

![Content Success in Asset Details Pane in Content Status Node](/_images/Content-Success-in-Asset-Details-Pane-in-Content-Status-Node.png "Content Success in Asset Details Pane in Content Status Node")

> **Important:** If the content isn't distributed to a **referenced distribution point** of a **client's boundary group**, you can right-click the item and choose to **Distribute Content**.

## Optional Additional Resources for Advanced Scenarios

You can run the following SQL query to determine what deployment packages software updates are available.

\-- Run this query against your ConfigMgr database to see **all software updates** and the deployment package they are part of.

```
SELECT dp.Name AS 
    , dp.PkgID
    , ui.Title AS 
    , c.SourceSize
    , c.ContentSource
FROM vSoftwareUpdatesPackage dp
JOIN v_Content c on c.PkgID = dp.PkgID
JOIN v_CIToContent cc on c.Content_ID = cc.ContentID
JOIN v_UpdateInfo ui on cc.CI_ID = ui.CI_ID AND UI.citype_id in (1,8)
```

If you want to limit the software updates to **only Patch My PC third-party updates**, you can run the following query instead.

\-- Run this query against your ConfigMgr database to see **only Patch My PC third-party software updates** and the deployment package they are part of.

```
DECLARE @CatId int
SET @CatId = (SELECT cinst.CategoryInstanceID
FROM v_CategoryInstances cinst
    JOIN v_CategoryInfo ci ON ci.CategoryInstanceID = cinst.ParentCategoryInstanceID
WHERE ci.CategoryInstanceName = 'Patch My PC')

SELECT dp.Name AS 
    , dp.PkgID
    , ui.Title AS 
    , c.SourceSize
    , c.ContentSource
    , updcat.CategoryInstanceID
FROM vSoftwareUpdatesPackage dp
    JOIN v_Content c ON c.PkgID = dp.PkgID
    JOIN v_CIToContent cc ON c.Content_ID = cc.ContentID
    JOIN v_UpdateInfo ui ON cc.CI_ID = ui.CI_ID AND UI.citype_id = 1
    JOIN v_CICategories updcat ON updcat.CI_ID = ui.CI_ID
WHERE updcat.CategoryInstanceID = @CatId
```