---
title: "Verification of file signature failed for file during update publishing"
date: 2020-08-26
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

This article covers a common error "**Verification of file signature failed for file:**" that can occur when publishing a **new update** or **revising** a third-party software update in **WSUS.**

## Determine if You are Affected

If you are affected by this error, you will see errors similar to the errors below in the **[PatchMyPC.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)**, [SMS\_ISVUPDATES\_SYNCAGENT.log](/collecting-log-files-for-patch-my-pc-support#publishing-in-console-logs), or **[SoftwareDistribution.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)**. The actual log file you see the error in may vary depending on the **Publishing method** you are using.

An error occurred while publishing an update to WSUS: Verification of file signature failed for file: ServerUpdateServicesPackagesUpdate-ID.cab

PublishPackage(): Operation Failed with Error: Verification of file signature failed for file: ServerUpdateServicesPackagesUpdate-ID.cab

SyncUpdate: Exception Message: Verification of file signature failed for file: ServerUpdateServicesPackagesUpdate-ID.cab

## Is the File Signature Failed Error Happening when Publishing New Updates?

There are two common scenarios where this error could occur. The first is when **publishing a new update to WSUS,** and the second is when **[revising an already published update](#topic3)**.

If using the **[Patch My PC Publisher](/publishing-service-setup-documentation)**, you should see the following in the **PatchMyPC.log** before the signature validation error:

![Verification of file signature-failed for file](/_images/Verification-of-file-signature-failed-for-file.png "Verification of file signature-failed for file")

If using the **[in-console third-party catalog feature in SCCM](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#publish-and-deploy-third-party-software-updates)**, you should see the following line in the **[SMS\_ISVUPDATES\_SYNCAGENT.log](/collecting-log-files-for-patch-my-pc-support#publishing-in-console-logs)** to confirm a new update fails to publish:

![sccm in console publishing error for validation of signature](/_images/sccm-in-console-publishing-error-for-validation-of-signature.png "sccm in console publishing error for validation of signature")

If the error occurs when publishing a new update to WSUS, the **[WSUS Signing Certificate](/wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager)** is not **[properly installed](/how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates)** in the **Trusted Root** or **Trusted Publishers** certificate store on the WSUS Server.

Next, you need to determine the **thumbprint** of the **WSUS Signing Certificate**. If using the Publisher, you can click the **Show Certificate** in the **General** tab and take note of **thumbprint**.

![show wsus signing certificate thumbprint](/_images/show-wsus-signing-certificate-thumbprint.png "show wsus signing certificate thumbprint")

If you are using the SCCM in-console publishing or any other method, you can open **certlm.msc**  and navigate to the **WSUS** certificate store. If you double click the certificate, you can review the thumbprint in the **Details** tab.

![check the certificate thumbprint](/_images/check-the-certificate-thumbprint.png "check the certificate thumbprint")

Once the WSUS Signing Certificate is identified, you need to ensure it's added to the **Trusted Root** and **Trusted Publisher** certificate store on the WSUS server.

Open **certlm.msc** and review the **Certification Path** tab of the WSUS certificate.

![This CA Root certificate is not trusted](/_images/This-CA-Root-certificate-is-not-trusted.png "This CA Root certificate is not trusted")

You may notice an error in this tab about the **certificate not being trusted**. Next, you need to ensure the certificate exists on both the **Trusted Root** and **Trusted Publishers** certificate store.

In the WSUS certificate store, **right-click** the certificate and click **Copy.**

![Copy WSUS Certificate in certlm](/_images/Copy-WSUS-Certificate-in-certlm.png "Copy WSUS Certificate in certlm")

In the **Trusted Publishers** store, right-click and click **Paste**:

![Paste WSUS Certificate in Trusted Publishers](/_images/Paste-WSUS-Certificate-in-Trusted-Publishers.png "Paste WSUS Certificate in Trusted Publishers")

Validate the certificate was copied to the **Trusted Publishers store**. If you receive a message, the certificate already exists you can click **Yes** to overwrite the existing certificate.

![Validate Certificate now Exist in Trusted Publishers](/_images/Validate-Certificate-now-Exist-in-Trusted-Publishers.png "Validate Certificate now Exist in Trusted Publishers")

Repeat the same process to **Paste** the WSUS Signing Certificate into the **Trusted Root** certificate store.

![Paste WSUS Certificate in Trusted Root](/_images/Paste-WSUS-Certificate-in-Trusted-Root.png "Paste WSUS Certificate in Trusted Root")

The WSUS certificate should now be trusted on the WSUS server, and you can attempt to **publish the update again**.

![Update Published after Verification of file signature failed](/_images/Update-Published-after-Verification-of-file-signature-failed.png "Update Published after Verification of file signature failed")

> **Note:** Although manually copying the WSUS signing certificate to the **Trusted Root** and **Trusted Publishers** is a quick fix for the WSUS server, ideally, the WSUS signing certificate should **[automatically be deployed to all machines](/how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates),** including the WSUS server. 

## Is the File Signature Failed Error Happening when Revising Existing Updates?

The error can also occur when an update already published to WSUS is attempting to be **revised**. In the **PatchMyPC.log**, you will see the following line before the error if it's being revised:

![Revising update validation failed](/_images/Revising-update-validation-failed.png "Revising update validation failed")

#### If the CAB file for the update being revised **does exist**:

If the **.CAB** file for the update referred to in the error exists. You will want to review the certificate used to sign it in the **Properties** of the file.

![CAB file in UpdateServicesPackage for Verification Failed](/_images/CAB-file-in-UpdateServicesPackage-for-Verification-Failed.png "CAB file in UpdateServicesPackage for Verification Failed")

You will need to **repeat the steps in the previous section** to ensure the certificate for the previously published update exist in the **Trusted Root** and **Trusted Publishers** certificate store on the WSUS server.

#### If the CAB file for the update being revised **does not exist**:

If the cab file doesn't exist in the **UpdateServicesPackages** folder, this means the file was likely removed during a **WSUS cleanup task,** or the **WSUS content** folder may have been **[moved](/how-to-move-the-wsus-content-folder-to-a-new-location)** without copying old updates.

Whenever an **update is revised** (a command line change is one example when this may happen), the WSUS API needs access to the originally published CAB file in the **UpdateServicesPackages** folder. If this file doesn't exist, you will always receive the error: "**Verification of file signature failed for file:**".

If you are unable to restore the CAB file in the **UpdateServicesPackages** from a backup, the only other workaround is to **[republish the updates for the product as a new update](/frequently-asked-questions#republishing-updates)** and supersede the current update that is unable to be revised during the republishing confirmation prompt.

## Video Guide

https://www.youtube.com/watch?v=AGXp8izcLBo