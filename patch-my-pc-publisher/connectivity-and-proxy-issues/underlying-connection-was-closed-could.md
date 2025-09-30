---
title: "The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel."
date: 2020-05-10
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - connectivity-and-proxy-issues
        - troubleshooting
        - security
---

The download error "_Could not establish trust relationship for the SSL/TLS secure channel"_ is generally related to your machine not trusting the website's root **[Certificate Authority](https://en.wikipedia.org/wiki/Certificate_authority)**. We are generally pretty limited in the amount of support we can provide for issues related to **web filters**, **firewalls**, **proxies**, **certificate trust**, or other **network-related errors**. However, our resolution below will generally help diagnose and resolve this specific error.

## Determine if you are affected

If affected, you will see an error similar to below in one of the following log files **[PatchMyPC.log](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#publishing-service-app-logs-intune)** or **[SMS\_ISVUPDATES\_SYNCAGENT.log](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/hierarchy/log-files#BKMK_SU_NAPLog),** dependent on the publishing method you are using.

WebClient report an error during download: The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.  
An error occurred while downloading the file: The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.

This error is usually because the **[Root Certificate authority](https://www.globalsign.com/en/ssl-information-center/what-are-certification-authorities-trust-hierarchies)** issuing the **[SSL certificate](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/)** is **not trusted** on your device.

## Possible cause 1: Validate the website's TLS/SSL certificate is trusted

A common reason you may receive the error **Could not establish trust relationship for the SSL/TLS secure channel** is because the SSL certificate isn't trusted.

On the machine receiving the error, browse to the URL from the log file for the download failing with this error using Internet Explorer running as NT AUTHORITY\\SYSTEM.

> **Important**: The Patch My PC Publisher uses the same .NET Framework classes as Internet Explorer to download content from the Internet. The default identity of the service is also NT AUTHORITY\\SYSTEM.
> 
> It may be possible you have different Internet access control policies applied between users and computers in your environment. Performing this test in a different browser or as another identity may produce different results.

A common reason you may receive the error **Could not establish trust relationship for the SSL/TLS secure channel** is because the **SSL certificate isn't trusted**.

![](/_images/ssl-certificate-not-trusted.png)

If the SSL certificate is not trusted, you will need to **install the SSL certificate's** root certificate.

For more details about this process for many common published certificate authorities, please review this article **[SSL Certificate Not Trusted Error](https://www.sslshopper.com/ssl-certificate-not-trusted-error.html).**

## Possible cause 2: The request was aborted: Could not create SSL/TLS secure channel.

The issue could also be related to the following:

- Firewall or proxy

- TLS/SSL cipher availability (limited by hardening policies or Operating System availability)

To troubleshoot the above items, please see the following article: **[The request was aborted: Could not create SSL/TLS secure channe](https://patchmypc.com/the-request-was-aborted-could-not-create-ssl-tls-secure-channel)**l.