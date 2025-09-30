---
title: "Digest Verification Failed on Content for Software Update"
date: 2019-03-11
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
        - security
---

The error **Digest verification failed on content for software update** is pretty common, In this article, we will review why file digest verification failures happen and how they are resolved.

## Determine if You are Affected by this Error

If using **[Patch My PC's Publisher](/docs)**, you will see an error similar to the one shown below in the [PatchMyPC.log](/collecting-log-files-for-patch-my-pc-support#publishing-in-console-logs):

The hash of file downloaded is different than the file hash in our catalog. Hash errors happen when vendors release updates that aren't available in our catalog yet. This error should be resolved in the next catalog update. Additional details: Downloaded Hash Catalog Hash

If you are using the **SCCM 1806+ [third-party updates](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates) feature directly in the SCCM console**, you will see the following errors in the **SMS\_ISVUPDATES\_SYNCAGENT.log** which is located on the top-level software update point in the site system logs folder.

STATMSG: (SRVMSG\_SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_HASH\_FAIL).

STATMSG: (SRVMSG\_SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_FAIL).

If you are using Microsoft's legacy method for publishing updates through **System Center Updates Publisher,** you will see an error similar to the error below in the **UpdatesPublishing.log** or **SCUP.log** located in your users **%temp%** directory.

\--- Digest verification failed on content for software update 'Google Chrome 72.0.3626.119 (x64) (UpdateId:'626b77a4-61e5-4e84-9870-3aa68abafd0e' Vendor:'Patch My PC' Product:'SCUP Updates')'.

## Why Does the Digest Verification Fail When Publishing Updates?

Whenever a third-party update is being published with full-content, the update needs to pass [security validation checks](https://patchmypc.com/deep-dive-into-security-validation-of-third-party-software-updates-in-microsoft-sccm). The first check is that the **binary downloaded from the vendor’s website** must **match the binary file digest when we created it.**

When vendors use the **same download URL** for a product such as Google Chrome: [https://dl.google.com/chrome/install/GoogleChromeStandaloneEnterprise64.msi](https://dl.google.com/chrome/install/GoogleChromeStandaloneEnterprise64.msi), hash validations can fail if you are **publishing an older version of the update** or if the **vendor just released a new update** that isn't in our catalog yet.

## Resolving Digest Verification Failures

To fix the digest validation error, you will need to **import** or **sync** the latest Patch My PC Update Catalog so you can publish **the latest software update** that will match the hash of the vendor's latest update binary.

#### **If you are using the [SCCM 1806+ third-party software update catalog feature](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates):**

You will need to [perform a sync of the catalog manually](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#subscribe-to-a-third-party-catalog-and-sync-updates). In the Third-Party Software Update Catalog node in your SCCM console, right-click the Patch My PC Catalog and choose **Sync now**. By default, SCCM will only [automatically sync a third-party software update catalog every 7 days](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#subscribe-to-a-third-party-catalog-and-sync-updates).

![](/_images/force-third-party-software-update-catalog-sync-in-the-sccm-console.png)

You can review the catalog sync progress in the **SMS\_ISVUPDATES\_SYNCAGENT.log**. located on the top-level software update point in the site system logs folder.

Once the catalog publishing sync is complete, you can **perform a sync of your software update point** to have any newly released third-party updates show up in the "All Software Updates" node of the SCCM console.

![sync sccm software update point for declined updates](/_images/sync-sccm-software-update-point-for-declined-updates.png "sync sccm software update point for declined updates")

Once the sync is complete, you should see **new updates** for the product, and the **previous updates that were failing should become superseded**. The latest version of the update should be able to publish the update content successfully.

![](/_images/new-updates-synced-in-sccm-publish-with-full-content.png)

#### **If you are using System Center Updates Publisher:**

You will need to import the latest catalog manually from the console:

![](/_images/import-the-lastest-patchmypc-third-party-update-catalog-to-resolve-hash-error-in-scup.png)

Once the newest catalog is imported, you should see **the latest update(s)** for the product that was previously failing to publish. Choose to publish the latest software updates(s).

![](/_images/scup-with-latest-third-party-updates-imported.png)

#### **If you are using Patch My PC'****s Publisher**

Our publisher always downloads the **latest catalog** before any publishing operations. If you receive a hash error while using our publisher, it means the vendor recently **released an update that isn't yet available in our catalog**. You will just need to wait for future catalog syncs for the issue to be automatically resolved.

## You Can Switch to Our Publisher to Reduce Hash Errors

When using **System Center Updates Publisher** or the **SCCM 1806+ Third Party Updates feature**, you will be more likely to run across hash errors, because there is a delay between when the catalog syncronizes and when you choose to manually publish the content.

For example, the SCCM 1806+ catalog feature can [only perform an automatic catalog sync every 7 days](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#subscribe-to-a-third-party-catalog-and-sync-updates), and this is not currently configurable.

- If SCCM syncronized our third-party update catalog on March 2, 2019, and on that date, Google Chrome 72.0.3626.119 (x64) was the latest version it would be published automatically to SCCM.

- If we then released Google Chrome 72.0.3626.121 (x64) on March 3, 2019, and you hadn't previously published the content for Google Chrome 72.0.3626.119 (x64) before it was updated on Googles web server you would receive a hash error.

- Since SCCM only syncs the catalog every 7 days, you would continue to have hash failures when trying to publish the update content until the next automatic sync occurs on March 9, 2019, or you perform a manual catalog sync in the SCCM console.

#### Benefits of using our publishing service with regards to hash digest errors

If you were to switch to our [publishing service](https://patchmypc.com/publishing-service-setup-documentation), you would have complete control over how often our catalog syncronizes. The scheduling options will allow you to sync the catalog more frequently to ensure you have the most recent catalog metadata.

![](/_images/publishing-service-sync-scedule.png)

Our publishing service will always download the most recent catalog before downloading any third-party update content. By downloading the newest metadata, it will ensure we are pulling the most recent updates with the current file digest before performing content publishing.

There are other benefits to switching to our publishing service including full automation. You can get more details about the benefits of our publishing service below. ?

**[What's the Difference our Publishing Service and SCCM 1806+ In-Console Publishing?](https://patchmypc.com/frequently-asked-questions#publishing-service-vs-sccm-publishing)**

## What if You Receive the Hash Error when Using the Latest Catalog?

If you receive the hash digest error after **verifying you have imported the latest catalog** and **publishing the latest update**, it's because the vendor just posted a new software update that hasn't been made available in our catalog yet. 

You can let us know you are getting a hash error when publishing the latest available update by using our **[technical support contact form](https://patchmypc.com/technical-support)**. Generally, we release updates the same day a vendor makes releases the update so it will just automatically resolve itself when we update the catalog.
