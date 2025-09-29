---
title: "Failed to sign package; error was: 2148204810"
date: 2020-08-25
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

In this article, we will be reviewing an error that can occur when trying to publish third-party software updates to **WSUS**.

## Determine if You are Affected

If you are affected by this error, you will see the following error(s) in the **PatchMyPC.log** or **SoftwareDistribution.log**

An error occurred while publishing an update to WSUS: Failed to sign package; error was: 2148204810

PublishPackage(): Operation Failed with Error: Failed to sign package; error was: 2148204810

This error generally will occur when the software update is being **[timestamped](https://en.wikipedia.org/wiki/Trusted_timestamping)**, but the WSUS Signing Certificate is not trusted on the WSUS Server.

> **Note:** Error code **2148204810** translates to **A certificate chain could not be built to a trusted root authority.**

## Check if the WSUS Signing Certificate is Trusted

On the WSUS Server, open **certlm.msc** from a **Run** prompt. In the **WSUS certificate store**, double click your WSUS Signing Certificate and click the **Certification Authority** tab.

![Check if WSUS Cert Root CA is Trusted](images/Check-WSUS-Cert-Root-CA.png)

The most common root cause for **error 2148204810** is the **WSUS Signing Certificate is not trusted at the [root level](https://docs.microsoft.com/en-us/windows-hardware/drivers/install/trusted-root-certification-authorities-certificate-store)** for the CA the issued the certificate.

You will need to determine why the root certificates are not trusted on the server. It's possible automatic root certificate updates were **[disabled via GPO](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-vista/cc749331\(v=ws.10\)?redirectedfrom=MSDN#how-turning-off-update-root-certificates-on-users-computers-can-affect-users-and-applications)**.

## Known Issue when Using Sectigo Code-Signing Certificate

We have worked with customers using a WSUS code-signing certificate **issued from Sectigo**. There's a known issue where if you don't have the **latest Sectigo Root CA and Code Signing certificates imported**, you will receive this error after May 30, 2020.

Please review the following article from Sectigo for information about this issue:

- [Sectigo AddTrust External CA Root Expiring May 30, 2020](https://support.sectigo.com/Com_KnowledgeDetailPage?Id=kA03l00000117LT)

- [How to Download & Install Sectigo Intermediate Certificates - RSA](https://support.sectigo.com/articles/Knowledge/Sectigo-Intermediate-Certificates)

## Is the WSUS Signing Certificate Expired?

Another possible reason you may receive **Failed to sign package; error was: 2148204810** is the **WSUS Signing Certificate may be expired**.
