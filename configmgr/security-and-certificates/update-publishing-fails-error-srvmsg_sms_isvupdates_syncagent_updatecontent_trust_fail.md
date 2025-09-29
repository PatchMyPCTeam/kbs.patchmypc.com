---
title: "Publishing Fails with Error SRVMSG_SMS_ISVUPDATES_SYNCAGENT_UPDATECONTENT_TRUST_FAIL"
date: 2020-05-14
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

In this article, we're going to be reviewing an error that can occur when using the built-in **[third-party software update catalogs feature](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates)** within Configuration Manager.

This error is related to updates failing to publish with full content due to a certificate trust issue in Configuration Manager. If you are experiencing this issue, you will see the following errors in the **[SMS\_ISVUPDATES\_SYNCAGENT.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-in-console-logs)** log file.

SyncUpdate: Certificate '%CERTIFICATE-THUMBPRINT%' is BLOCKED.  
SyncUpdate: ff1bb935-7ecd-482f-988e-ba7fafe0eacf - Signature check on downloaded binary has failed.  
STATMSG: (SRVMSG\_SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_TRUST\_FAIL).  
STATMSG: (SRVMSG\_SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_FAIL).

## How Certificate Trust Works for Third-Party Update Catalogs

When you **[subscribe to a third-party software update catalog](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#add-a-custom-catalog)** in the Configuration Manager console, you will be required to accept the primary certificate used to sign the catalog file. During the catalog export, we will also include **code-signing certificates from the installer files** for updates within our catalog.

For example, in the extracted catalog in the screenshot below, you can see a certificate used to **code-sign Dropbox's installer**.

![](/_images/catalog-content-certficate.png)

During a catalog sync in the **SMS\_ISVUPDATES\_SYNCAGENT.log**, you can review the **certificates being imported** into Configuration Manager from a third-party update catalog.

![](/_images/content-certificate-import-SMS_ISVUPDATES_SYNCAGENT.png)

## Why The STATMSG: (SRVMSG\_SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_  
TRUST\_FAIL). Error?

Before any third-party update can be published to WSUS, the SMS\_ISVUPDATES\_SYNCAGENT component will always **perform a certificate check on the downloaded update file**.

![](/_images/SMS_ISVUPDATES_SYNCAGENT-retrieved-certificate-checking-signature.png)

When an update file is downloaded, and the code-signing certificate **isn't trusted** within Configuration Manager, you will receive the error:

STATMSG: (SRVMSG\_SMS\_ISVUPDATES\_SYNCAGENT\_UPDATECONTENT\_TRUST\_FAIL).

Within the Configuration Manager Console, you can navigate to **Administration** > **Security** > **Certificates**

![](/_images/unblock-certificate-configuraton-manage-third-party-updates.png)

You can then right-click the certificate of the blocked certificate and click **Unblock**. Once unblocked, you should be able to go back to the update and choose **Publish Third-Party Software Update Content,** and it should publish successfully.

![](/_images/Signature-check-on-download-binary-has-completed.png)

## Why was the Certificate Blocked in Configuration Manager

There are two primary reasons a certificate may be blocked.

- You **unsubscribed** from the catalog that imported the Third-party Software Update Content certificate

- The certificate was **blocked** from the console

- We forgot to include the certificate in our catalog

If you believe it's the last point, you can **[open a support case](https://patchmypc.com/technical-support)**, and attach your **SMS\_ISVUPDATES\_SYNCAGENT.log** file. The log file will allow our team to review if the certificate used for a vendor's installer may not be included in our catalog export process.