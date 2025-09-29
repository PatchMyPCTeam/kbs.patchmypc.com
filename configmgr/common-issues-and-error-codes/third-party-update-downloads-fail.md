---
title: "Third-Party Update Downloads Fail: Invalid certificate signature Error 0x800b0004 and 0x87D20417"
date: 2019-03-14
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

Error **0x800b0004,** **0x80073633,** or **13875** generally occurs when attempting to download third-party software updates into a deployment package. When attempting to download an update into a deployment package in the SCCM console, you receive the error message **Error: Invalid certificate signature**

## Determine if You are Affected

Whenever a software update is being downloaded, regardless of whether it's a Microsoft or third-party update, the **certificate used to sign the software update must always be trusted** by the machine running the **ConfigMgr console** or by the **site server** in the case it's an automatic deployment rule.

![third-party update download fails in the sccm console error Invalid certificate signature 0x800b0004](images/downloads-fail-in-the-sccm-console-error-Invalid-certificate-signature-0x800b0004.png)

If using **automatic deployment**, the error code will be a little different, as shown in the screenshot below:

![SCCM ADR Error Code 0x80073633 Invalid certificate signature](images/SCCM-ADR-Error-Code-0x80073633-Invalid-certificate-signature.png)

If the **[WSUS signing certificate](/wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager)** hasn't properly been deployed to the **Trusted Root** and **Trusted Publishers** certificate store on the **console device** or the **site server**, the update validation check will fail, and the update will not be downloaded into the deployment package.

### Is the download is failing when manually downloading in the ConfigMgr console?

If failing when using the ConfigMgr console, you will likely see the following errors in the **[PatchDownloader.log (For Console Downloads)](/collecting-log-files-for-patch-my-pc-support#deployment-package-download-logs)**

Authentication of file C:\\UsersAppData\\Local\\TempCAB85AE.tmp failed, error 0x800b0004

ERROR: DownloadUpdateContent() failed with hr=0x80073633

### Is the download is failing when using Automatic Deployment Rules (ADRs) in ConfigMgr?

If failing to download when using ADRs, you should check the **[PatchDownloader.log (For ADR)](/collecting-log-files-for-patch-my-pc-support#automatic-deployment-rules-logs).** You will likely see the following errors:

Authentication of file C:\\UsersAppData\\Local\\TempCAB85AE.tmp failed, error 0x800b0004

ERROR: DownloadUpdateContent() failed with hr=0x80073633

If failing to download when using ADRs, the [RuleEngine.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SUPLog) will likely see the following error:

Failed to download the update content with ID 16777699 from internet. Error = 13875

### Are updates failing to install on a client with 0x800b0004?

If updates are failing to install with error 0x800b0004, you will see the following error in **[WUAHandler.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#update-troubleshooting-client-logs)**:

Failed to download updates to the WUAgent datastore. Error = 0x800b0004.

If this error occurs **during the update installation on a client** (not the download to deployment package), it means the **WSUS Signing Certificate isn't installed** in the **Trusted Publishers** certificate store on the client. If this is **your scenario**, please see this article **[Third-Party Updates Fail to Install with Error 0x800b0109 in SCCM](https://patchmypc.com/third-party-updates-fail-to-install-with-error-0x800b0109-in-sccm)** [](https://patchmypc.com/third-party-updates-fail-to-install-with-error-0x800b0109-in-sccm)instead.

**0x800b0004 = The subject is not trusted for the specified action.  
0x80073633 = Invalid certificate signature  
13875 = Invalid certificate signature**

> **Note:** If using an **ADR**, the actual error in the last error code in the console for the ADR may be a more generic errorÂ [0X87D20417](/automatic-deployment-rule-adr-error-0x87d20417-or-0x80070194) or [0x80070194](/automatic-deployment-rule-adr-error-0x87d20417-or-0x80070194).

**Important:** The **patchdownloader.log** is located in a different location, depending on whether you **[manually download via the console](/collecting-log-files-for-patch-my-pc-support#deployment-package-download-logs)** or using an **[automatic deployment rule](/collecting-log-files-for-patch-my-pc-support#automatic-deployment-rules-logs)**.

## Resolution: Check if the WSUS Signing Certificate is Trusted

The most common reason error **0x800b0004** or **0x80073633** occurs when downloading an update is that the **[WSUS signing certificate](/wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager)** that signed the update isn't installed in the **Trusted Root** certificate store.

To check if the WSUS signing certificate is installed, perform the **following actions**:

**1\. Download** the specific update CAB file failing to download using a web browser or copy directly from the WSUSContent folder. You can get the update download URL or folder/file path to the WSUSContent folder based on the **PatchDownloader.log**

![manually download update failing with error 0x800b0004 Error Invalid certificate signature](images/manually-download-update-failing-with-error-0x800b0004-Error-Invalid-certificate-signature.png)

2\. **Copy** the .CAB file to the machine running the **ConfigMgr console if failing to download via console** or to the **site server if using an ADR**. On **properties** of the file, review the **Certification Path** tab, and review if there are any trust errors.

![This CA Root certificate is not trusted because it is not in the Trusted Root Certification Authorities store](images/This-CA-Root-certificate-is-not-trusted-because-it-is-not-in-the-Trusted-Root-Certification-Authorities-store.png)

If the certificate shows any **trust errors**, you will need to **[deploy this certificate to all machines](/how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates)** or **manually install** it to **Trusted Root** and **Trusted Publishers** on the affected machine if for testing only.

## Video Resolution Guide to Invalid certificate signature Error 0x800b0004

Please review our video guide below to explains how to resolve this download issue.

<iframe src="https://www.youtube.com/embed/xB8QnLTEWQY" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>

## Additional Information 

If you are using the SCCM console during the failure, you will also see the following error in **SmsAdminUI.log**:

Failed to download content id 16939262. Error: Invalid certificate signature
