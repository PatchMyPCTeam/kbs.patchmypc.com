---
title: "Update CAB Files Deleted from WSUSContent when Using Shared SUSDB"
date: 2020-11-03
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
        - troubleshooting
        - common-issues-and-error-codes
        - updates
---

We've recently seen an increase in customer cases where published third-party software update (**.CAB files**) are automatically deleted from the **[WSUSContent folder](/clean-up-third-party-updates-from-the-wsus-updateservicespackages-folder#wsuscontent)**. For this scenario, it occurs when customers are using a **shared SUSDB** among **multiple WSUS servers.**

## Determine if You are Affected

The first step to determine if this could be your issue is to check if you have **multiple WSUS servers** (Software Update Points in ConfigMgr) using the same **SUSDB**. The most accurate way to check if multiple servers are using the same SUSDB is to **connect to the SUSDB using SQL Server Management Studio**.

Once connected, run the following **SQL commands**:

Use SUSDB  
exec spGetFrontEndServers

The result of this query should list **all WSUS front end servers** using the same **SUSDB**.

![spGetFrontEndServers Against SUSDB in SQL Management Studio](images/spGetFrontEndServers-Against-SUSDB-in-SQL-Management-Studio.png)

If there aren't multiple servers listed in the **ServerName** column, this blog post will not be related to your issue.

If affected, you will see **.CAB updates automatically be deleted** right after the publishing is completed. In many cases, this will happen so fast you will not see the file appear unless you run **[Procmon](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)** to verify it was created. You will likely only see a new folder created in the **[WSUSContent directory](/clean-up-third-party-updates-from-the-wsus-updateservicespackages-folder#wsuscontent)** that is empty, and the third-party update will fail to download to a deployment package with **error 404**.

![](../../_images/deleting-update-wsus-content.gif)

## Why the Third-Party Update CAB Files are Auto-Deleting

If this is your scenario, we'll dig into our findings after working with multiple customers having this same issue. The big thing to note when using a shared SUSDB with multiple WSUS servers is what WSUS server is acting as the **NLBMasterFrontEndServer**. From a third-party update perspective, the **NLBMasterFrontEndServer** is the **WSUS server that performs hash validation** after a third-party software update is published to the WSUSContent folder.

WSUS has a stored procedure (**spUpdateServerHealthStatus**) that will automatically change the WSUS server that holds the **NLBMasterFrontEndServer** role if the current **NLBMasterFrontEndServer** server has been **unavailable for 5 minutes**. This includes the server being offline, WSUS service stopped, or database connectivity issues.

To determine what WSUS front end server currently holds the **NLBMasterFrontEndServer** role, you can run the following query against the **SUSDB**.

select \* from tbReference

In the example below, we can see the **NLBMasterFrontEndServer** is set to **SITESYSTEM.CONTOSO.LOCAL**. The publishing of updates is being performed from the WSUS server **DEMO4.CONTOSO.LOCAL**, because it is the top-level software update point within our Configuration Manager environment.

![SUSDB Get NLBMasterFrontEndServer Server](images/SUSDB-Get-NLBMasterFrontEndServer-Server.png)

The root cause for the actual .CAB files being deleted is because the **NLBMasterFrontEndServer** server doesn't trust the WSUS signing certificate from the WSUS server that is performing the publishing of third-party updates.

Within the **WSUS log file** (C:\\Program Files\\Update ServicesLogFilesSoftwareDistribution.log) on the **NLBMasterFrontEndServer** (SITESYSTEM.CONTOSO.LOCAL in our scenario), you will actually be able to see the CAB files being deleted. 

CabUtilities.CheckCertificateSignature File cert verification failed for demo4j$WSUS\\WsusContent7E88A3263D0FBFB63F57D93A0322958B051B18B97E.cab with 2148204809  
ContentSyncAgent.WakeUpWorkerThreadProc Importing file 88A3263D0FBFB63F57D93A0322958B051B18B97E failed at file cert verification  
ContentSyncAgent.WakeUpWorkerThreadProc Invalid file deleted: demo4j$WSUS\\WsusContent7E88A3263D0FBFB63F57D93A0322958B051B18B97E.cab

The error code **2148204809** mentioned in the log above translates to _**A certificate chain processed, but terminated in a root certificate which is not trusted by the trust provider.**_

## How to Resolve CAB File Auto-Deletion for Third-Party Updates

The previous section gives insight into the root cause of the issue which is the **NLBMasterFrontEndServer** doesn't trust the **[WSUS Signing Certificate](/wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager)** from the WSUS server performing the publishing. The fix is to **export the WSUS Signing Certificate** from the server performing the publishing and install it in the **Trusted Root** and **Trusted Publishers** certificate store on the **NLBMasterFrontEndServer** WSUS server.

If are using the **[Patch My PC Publisher](/docs)**, you can **export** the WSUS Signing Certificate from the general tab to a **(.CER file)**.

![Export WSUS Signing Certificate](images/Export-WSUS-Signing-Certficate.png)

If you are not using the Publisher, you can **export** the WSUS Signing Certificate using the following steps:

1. Open **certlm.msc** on the WSUS server performing the publishing

3. Click the **WSUS** store

5. **Right-click** the **WSUS Signing Certificate** > **All Tasks** > **Export =**
    1. **
        ![certlm.msc Export WSUS Signing Certificate](images/certlm.msc-Export-WSUS-Signing-Certificate.png)
        **

7. In the **Export Private Key** dialog, be sure to choose the option "**No, do not export the private key**"

Copy the **(.CER certificate file)** from the WSUS server that it was exported from (DEMO4.CONTOSO.LOCAL in our case) to the **NLBMasterFrontEndServer** (SITESERVER.CONTOSO.LOCAL in our case). On the **NLBMasterFrontEndServer** WSUS server, open the **(.CER certificate file)**. In the **Certificate Path** tab, you will likely see the certificate shows an error similar to the one below:

![This CA Root certificate is not trusted because it is not in the Trusted Root Certification Authorities store.](images/This-CA-Root-certificate-is-not-trusted-because-it-is-not-in-the-Trusted-Root-Certification-Authorities-store.png)

On the General tab of the certificate, click **Install Certificate...** button

![Install Certificate... from certlm.msc](images/Install-Certificate.-from-certlm.msc_.png)

In the **Certificate Import Wizard** choose **Local Machine** for the **Store Location**

![Store Location as Local Machine](images/Store-Location-as-Local-Machine.png)

Click the radio button **Place all certificates in the following store** and browse to **Trusted Root Certification Authorities**

![Install Certificate to Trusted Root Certification Authorities certlm](images/Install-Certificate-to-Trusted-Root-Certification-Authorities-certlm.png)

Once installed to **Trusted Root Certification Authorities**, **repeat the certificate installation** steps above, but choose the certificate store **Trusted Publishers**

![Install Certificate to Trusted Publishers certlm](images/Install-Certificate-to-Trusted-Publishers-certlm.png)

> **Important:** The WSUS Signing Certificate must be installed at both the **Trusted Root Certification Authorities** and **Trusted Publishers**

Once the [WSUS Signing Certificate](/wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager) is installed on the **NLBMasterFrontEndServer** WSUS server, any future published updates should not be automatically deleted.

![WSUS Update CAB File in WSUSContent Folder](images/WSUS-Update-CAB-File-in-WSUSContent-Folder.png)

For any third-party update (.CAB files) that have already been deleted, you will need to republish those products as new updates using the article **[When and How to Republish Patch My PC Third-Party Updates](/when-and-how-to-republish-third-party-updates)**

> **Note:** The WSUS Signing Certificate should be installed on **all devices in your environment**. You can review our guide **[How to Deploy the WSUS Signing Certificate for Third-Party Software Updates](/how-to-deploy-the-wsus-signing-certificate-for-third-party-software-updates)** for information on deploying the WSUS Signing Certificate site-wide.
