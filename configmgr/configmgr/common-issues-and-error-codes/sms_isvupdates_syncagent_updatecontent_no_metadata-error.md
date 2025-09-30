---
title: "SMS_ISVUPDATES_SYNCAGENT_UPDATECONTENT_NO_METADATA Error"
date: 2019-04-23
taxonomy:
    products:
        - 
    tech-stack:
        - configmgr
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - common-issues-and-error-codes
        - troubleshooting
        - log-collection-and-analysis
---

You may receive error **SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_NO\_METADATA** when attempting to right-click a third-party software update and choosing **Publish Third-Party Software Update Content**.

## Determine if You are Affected

If you are affected by this error, you will see one of the following errors in the **SMS\_ISVUPDATES\_SYNCAGENT.log when using an automatic deployment rule**

STATMSG: (SRVMSG\_SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_NO\_METADATA).

STATMSG: (SRVMSG\_SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_FAIL).

If you are trying to **manually download the update to a deployment package using the ConfigMgr console**, you will see the error **All software updates in this selection are expired to meta-data only, and cannot be downloaded.**

![metadata-only update unable to download into deployment package](/_images/metadata-only-update-unable-to-download-into-deployment-package.png "metadata-only update unable to download into deployment package")

This error occurs because the update you are trying to download to a deployment package in only published with **[metadata only](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#subscribe-to-a-third-party-catalog-and-sync-updates)** and not full-content. This is also a known issue **[documented in Microsoft Docs](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#known-issues)**.

## Resolve Error SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_NO\_METADATA (Post)

When you [subscribe to a custom third-party software update catalog](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#subscribe-to-a-third-party-catalog-and-sync-updates) using the [SCCM 1806+ third-party software update catalogs feature](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates), updates are published as metadata-only during the catalog synchronization until you manually choose to publish the update content.

You can determine if updates are **metadata-only** from the **All Software Updates** node if the updates are displayed with a **blue icon**.

![third-party updates published as metadata-only icon](/_images/third-party-updates-published-as-metadata-only-icon.png "third-party updates published as metadata-only icon")

Updates published as **metadata-only** can't be deployed until they a [re-published with full-content](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#publish-and-deploy-third-party-software-updates). If you attempt to download a metadata-only update into a deployment package, you will receive the error "**All software updates in this selection are expired or meta-data only, and cannot be downloaded.**"

![metadata-only update unable to download into deployment package](/_images/metadata-only-update-unable-to-download-into-deployment-package.png "metadata-only update unable to download into deployment package")

In order to download and deploy third-party updates in a metadata-only state, you will need to right-click the update(s) and choose to "**Publish Third-Party Software Update Content**".

![publish third-party update with full-content from metadata-only](/_images/publish-third-party-update-with-full-content-from-metadata-only.png "publish third-party update with full-content from metadata-only")

You can review the full-content publishing progress in the **SMS\_ISVUPDATES\_SYNCAGENT.log**. The log is located on the top-level software update point in the site system Logs folder.

Once the publishing of third-party update content is complete, you can [sync your software update point](https://docs.microsoft.com/en-us/mem/configmgr/sum/get-started/synchronize-software-updates#manually-start-software-updates-synchronization) for SCCM to pick up the change from metadata-only to full-content faster.

![sync sccm software update point for declined updates](/_images/sync-sccm-software-update-point-for-declined-updates.png "sync sccm software update point for declined updates")

Once the software update point sync is complete, the third-party updates should show in a [normal state](https://docs.microsoft.com/en-us/previous-versions/system-center/system-center-2012-R2/hh848254\(v=technet.10\)) with a green icon meaning they are available for software update deployment.

![third-party updates in normal state](/_images/third-party-updates-in-normal-state.png "third-party updates in normal state")

Starting in ConfigMgr 2002 and the v3 catalog format, you can now automatically choose to publish updates with **[full-content using the stage content option](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#new-subscription-to-a-third-party-v3-catalog)**.

You may also consider using the **[Patch My PC Publisher](/docs)** for syncing the catalog automatically with full-content rather than the in-console publishing:

**[What's the Difference Between the Publisher and SCCM In-Console for Update Publishing?](https://patchmypc.com/frequently-asked-questions#publishing-service-vs-sccm-publishing)**

## Resolve Error SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_NO\_METADATA (Video)

Please review the step-by-step video guide below for information about why the **SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_NO\_METADATA** error occurs while publishing third-party software update content and the resolution.

<iframe src="https://www.youtube.com/embed/5e_jLmvfgTk" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>

## Additional Details

To enable the Delete button in our publishing service, create a new DWORD registry value: **Computer\\HKEY\_LOCAL\_MACHINE\\Software\\Patch My PC Publishing Service:EnableDeleteUpdates = 1**

This scenario is also documented on the [Microsoft Docs - Third-Party Software Updates Known Issues page](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#known-issues).

- The third-party software update synchronization service can't publish content to metadata-only updates that were added to WSUS by another application, tool, or script, such as SCUP. The Publish third-party software update content action fails on these updates. If you need to deploy third-party updates that this feature doesn't yet support, use your existing process in full for deploying those updates.