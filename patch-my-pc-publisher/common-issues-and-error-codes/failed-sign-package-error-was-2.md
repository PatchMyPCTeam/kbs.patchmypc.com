---
title: "Failed to sign package; error was: 2148081670"
date: 2023-03-29
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

# Failed to sign package; error was: 2148081670

In this article, we will be reviewing an error that can occur when trying to publish third-party software updates to **WSUS**.

## Determine if You are Affected

If you are affected by this error, you will see the following error(s) in the **[PatchMyPC.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-app-logs-intune)** or **[SoftwareDistribution.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-logs)**.

An error occurred while publishing with timestamping, and timestamping is enforced: Failed to sign package; error was: 2148081670

An error occurred while publishing an update to WSUS: Failed to sign package; error was: 2148081670

PublishPackage(): Operation Failed with Error: Failed to sign package; error was: 2148081670

This error can occur if the **code signing certificate** used for WSUS **has been revoked** by the Certificate Authority (CA).

You can check if your certificate has been revoked by following these steps using the Patch My PC Publisher:

1. Click **Show Certificate** in the **General** tab

3. Click **Validate Trust Chain...**

![Validate Trust Chain](images/ValidateTrustChain-Revoked.png)

> **Note:** We have also had reports of this specific error from customers even when the certificate was not revoked. Therefore, **all circumstances as to why WSUS uses this error code are not yet known**.

## Solution

If the certificate has been revoked by your CA, **you may be able to unrevoke it**. For example, if the certificate was issued by an Active Directory Certificate Services (ADCS) CA and the "Certificate Hold" reason code was used to revoke, then it is possible to unrevoke the certificate.

If your certificate is not revoked and you still have the original .pfx for your current code signing certificate, you may want to first try re-importing that instead of issuing a new certificate.

However, if re-importing the original code signing certificate does not solve the issue, then you will need to **import a new code signing certificate**.

You will need to do the following in order:

1. Import your new code signing certificate

3. Deploy your new code signing certificate to your devices

5. Re-sign all of your existing third-party updates in WSUS

7. Download all third-party updates into a new Deployment Package

### 1\. Import your new code signing certificate

For guidance to import a code signing certificate into the Patch My PC Publisher, see **[What is the WSUS Signing Certificate and How to Create It](/wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager)**.

### 2\. Deploy your new code signing certificate to your devices

For guidance to deploy your new code signing certificate, see [How to Deploy the WSUS Signing Certificate for Third-Party Software Updates](https://patchmypc.com/how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates).

### 3\. Re-sign all of your existing third-party updates in WSUS

Using the [Modify Published Updates Wizard](https://patchmypc.com/modify-published-third-party-updates-wizard), select all of your existing third-party software updates in WSUS and choose the option to **Re-Sign Update**.

![Re-sign all third party updates](images/ModifyPublishedUpdates-ReSignUpdate.png)

### 4\. Download all third-party updates into a new Deployment Package

At this point, after re-signing all of your third-party updates, all of the .cab files in the WSUS directory should have a digital signature. They will be signed with your new code signing certificate.

You can verify this by browsing to your WSUSContent or UpdateServicesPackages folder and looking at the digital signature for any of your .cab files containing a third-party update.

![Verify digital signature is of new certificate after re-signing](images/WsusContentDigitalSignature.png)

Now, you must **delete any previously downloaded third-party software updates in any existing Deployment Packages**. After you have done this, you need to **download all of your third-party software updates** into a **new Deployment Package in Configuration Manager**.

Updates need to be downloaded again because the .cab files in your current Deployment Package(s) are signed with your old code signing certificate. They need to be re-downloaded since their digital signatures have changed, and you need your clients to trust all of your third-party updates.

![Delete deployment package in Configuration Manager](images/DeleteDeploymentPackage.png)
