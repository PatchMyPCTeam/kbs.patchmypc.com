---
title: "Third-Party Updates Fail to Install with Error 0x800b0101 in SCCM"
date: 2021-04-07
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

When installing some, or all, third-party software updates, you receive the error code **0x800b0101**.

## Determine if you are affected - Possible Cause 1

![](/_images/WindowsUpdate-log_.png)

In **[WindowsUpdate.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#update-troubleshooting-client-logs "Collecting Log Files to Send to Support for SCCM and Intune")** you will see the following error in the log:

Failed WS error: The date in the certificate is invalid or has expired

\*FAILED\* Web service call

In [WUAHandler.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#update-troubleshooting-client-logs "Collecting Log Files to Send to Support for SCCM and Intune") you will see the following error in the log:

OnSearchComplete - Failed to end search job. Error = 0x800b0101.

Scan failed with error = 0x800b0101.

## Step 1: Renew the SSL certificate bound to the WSUS instance in IIS

The error suggests that the certificate bound to the WSUS instance in IIS has expired. You will need to generate a new SSL certificate and bind it to the site

![](/_images/IIS_Certificate.png)

## Determine if You are Affected - Possible Cause 2

This error generally will occur when attempting to **install some (not always all) third-party software updates**. You may see the following error in software center:

![Error-0x800b0101-Third-Party-Updates-SCCM](/_images/Error-0x800b0101-Third-Party-Updates-SCCM.png "Error-0x800b0101-Third-Party-Updates-SCCM")

In **[WUAHandler.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#update-troubleshooting-client-logs "Collecting Log Files to Send to Support for SCCM and Intune")** and [UpdatesHandler.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#update-troubleshooting-client-logs "Collecting Log Files to Send to Support for SCCM and Intune") you will see the following error in the log:

Failed to download updates to the WUAgent datastore. Error = 0x800b0101.

Failed to initiate install of WSUS updates, error = 0x800b0101  
Failed to start batch install through WSUS Install handler , error = 0x800b0101  
InstallUpdatesInBatch failed with error 0x800b0101

![Error-0x800b0101-Third-Party-Updates-SCCM-2](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-2.png "Error-0x800b0101-Third-Party-Updates-SCCM-2")

![Error-0x800b0101-Third-Party-Updates-SCCM-3](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-3.png "Error-0x800b0101-Third-Party-Updates-SCCM-3")

In [WindowsUpdate.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#update-troubleshooting-client-logs "Collecting Log Files to Send to Support for SCCM and Intune") you may see something similar to the below:

Error: 0x800b0101 when verifying trust for C:\\Windows\\SoftwareDistribution\\Download  
Copy update to cache failed with exit code = 0x800B0101  
ISusInternal:: CopyUpdateToCache2 failed, hr=800B0101

![Error-0x800b0101-Third-Party-Updates-SCCM-4](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-4.png "Error-0x800b0101-Third-Party-Updates-SCCM-4")

**0x800b0101 = A required certificate is not within its validity period when verifying against the current system clock or the timestamp in the signed file.**

This error occurs when a client attempts to install a software update which is signed with a code signing certificate that is **expired**, and it **was not timestamped**.

By default, **[timestamping](https://en.wikipedia.org/wiki/Trusted_timestamping "Trusted_timestamping")** is enabled in the Patch My PC Publisher to provide you with the flexibility of ensuring software updates can still install on your devices even if your code signing certificate has expired.

However if timestamping is disabled, and updates were not timestamped, you will experience this error when clients attempt to install said updates **after the code signing certificate has expired**.

Sometimes customers [disable timestamping in the Publisher](https://patchmypc.com/how-to-disable-timestamping-for-patch-my-pc-update-publishing "How to Disable Timestamping for Patch My PC Update Publishing") to workaround proxy or firewall issues to **timestamp.digicert.com**. We do not recommend disabling timestamping.

To determine if have timestamping enabled or disabled in the Publisher, check the registry value **HKEY\_LOCAL\_MACHINE\\Software\\Patch My PC Publishing Service:DisableTimestamping**. A value of 1 means it is disabled, whereas 0 or a non-existent registry value means it is enabled (the default).

You can see the certificate used to sign an update and whether it is timestamped by right clicking on the .cab file and going in to **Properties**. The .cab file for the problem update can be found in **ccmcache** on the client, or the **WsusContent** directory on the WSUS/SUP server.

Go to the **Digital Signatures** tab, select the certificate from the list and choose **Details**.

![Error-0x800b0101-Third-Party-Updates-SCCM-6](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-6.png "Error-0x800b0101-Third-Party-Updates-SCCM-6")

From here you can see if the .cab file is timestamped in the **Countersignatures** section at the botom.

![Error-0x800b0101-Third-Party-Updates-SCCM-7](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-7.png "Error-0x800b0101-Third-Party-Updates-SCCM-7")

Click **View Certificate** and verify if the certificate has expired.

![Error-0x800b0101-Third-Party-Updates-SCCM-8](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-8.png "Error-0x800b0101-Third-Party-Updates-SCCM-8")

## Step 1: Renew your code signing certificate and import it into the Publisher

If you use an internally issued certificate from your own PKI root / intermediate Certificate Authorities, **re-issue a new code signing certificate**. Ensure your devices have this new certificate in their **Trusted Publishers** certificate store.

If you use a self-signed certificate, regenerate a new code signing certificate and ensure your devices have this new certificate in their **Trusted Root** and **Trusted Publishers** certificate store.

After you have obtained **a new valid certificate**, import it into the Publisher.

![Error-0x800b0101-Third-Party-Updates-SCCM-5](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-5.png "Error-0x800b0101-Third-Party-Updates-SCCM-5")

> **Note:** If your SUP is remote from your site server, ensure this new certificate is also in the **Trusted Root** and **Trusted Publishers** certificate store **on the site server** itself.

## Step 2: Re-publish the software update(s)

Next, you must **re-publish the problem updates,** so they are recreated in WSUS with **a new .cab file using a new code signing certificate**.

We have an article that discusses **[When and How to Republish Patch My PC Third-Party Updates](https://patchmypc.com/when-and-how-to-republish-third-party-updates "When and How to Republish Patch My PC Third-Party Updates")** in more detail if you want to learn more.

Right-click on the offending updates and choose **Republish update(s) for these product(s) during the next sync schedule**.

For the subsequent pop-ups, select **Yes** for the first pop-up, and we recommend you consider selecting **Yes** for the second pop-up.Ã 

![Error-0x800b0101-Third-Party-Updates-SCCM-9](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-9.png "Error-0x800b0101-Third-Party-Updates-SCCM-9")

![Error-0x800b0101-Third-Party-Updates-SCCM-10](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-10.png "Error-0x800b0101-Third-Party-Updates-SCCM-10")

![Error-0x800b0101-Third-Party-Updates-SCCM-11](/_images/Error-0x800b0101-Third-Party-Updates-SCCM-11.png "Error-0x800b0101-Third-Party-Updates-SCCM-11")

## Step 3: Sync, the Publisher, download the updates, distribute, and deploy

After that, you need to **sync the Publisher** to republish the software update to WSUS again. In the **Sync Schedule** tab, click **Run Publishing Service Sync**.

Once the Publisher has finished synchronizing and your SCCM SUP has synchronized with WSUS, you can now download, distribute and deploy the newly republished software update(s), **which will be code signed using the new certificate**.

You can verify the republished update is using the new certificate by observing the certificate on the .cab file stored in the WsusContent directory on your WSUS/SUP server as shown previously at the end of the **[Determine if You are Affected section](#topic1 "Determine if You are Affected section")**.