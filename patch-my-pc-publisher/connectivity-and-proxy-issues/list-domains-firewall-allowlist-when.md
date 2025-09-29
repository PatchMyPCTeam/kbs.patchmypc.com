---
title: "List of Domains for Firewall Allowlist when Using Patch My PC"
date: 2018-04-14
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

When updates are published using **full content** from our catalog, the Publisher will need to download content files **from patchmypc.com** and **other vendor's websites**.

You need to allow traffic to the domains for products that you choose to publish with full content. The Publisher downloads content for updates and applications directly from the vendor's website. In the case any vendor **resides in a region that is geo-blocked**, the Publisher cannot download the binaries for the corresponding vendor.

If **user-agent filters** are in place, you need to allow "**Patch My PC Publishing Service\***" for the publishing service's user agent.

## Review Download Failures from Firewall Rules for Previous Synchronizations

In addition to the full list of domains provided below, we also store the **last download response code and domain for currently enabled products**. This can be helpful when reviewing download failures for currently enabled products.

The download history can be found on a CSV file in: **%InstallDir%\\PatchMyPC-DownloadHistory.csv**

![PatchMyPC-DownloadHistory.csv File for Download History](/_images/PatchMyPC-DownloadHistory-csv-File-for-Download-History.png "PatchMyPC-DownloadHistory.csv File for Download History")

Here's an example of the data included in the **PatchMyPC-DownloadHistory.csv file**. You can use the **domain column to create firewall exceptions** based on the products enabled and failing to download.

![PatchMyPC-DownloadHistory.csv Example List](/_images/PatchMyPC-DownloadHistory-csv-Example-List.png "PatchMyPC-DownloadHistory.csv Example List")

## List of Domains Always Required

\[table id=29 /\]

**Note**: The list of potential Digicert domains can be found here (Only CertCentral Global CRLs are required): [DigiCert Certificate Status IP Addresses](https://knowledge.digicert.com/alerts/digicert-certificate-status-ip-address)

## List of Domains/IP Ranges for Intune

If you are publishing to Intune, as well as the above domains, you will also need the necessary domains, ports, and protocols for Microsoft Azure too.

**Allow the following Azure portal URLs on your firewall or proxy serve**r  
[https://docs.microsoft.com/en-us/azure/azure-portal/azure-portal-safelist-urls?tabs=public-cloud](https://docs.microsoft.com/en-us/azure/azure-portal/azure-portal-safelist-urls?tabs=public-cloud)

**Azure IP Ranges and Service Tags – Public Cloud \***  
[https://www.microsoft.com/en-us/download/details.aspx?id=56519](https://www.microsoft.com/en-us/download/details.aspx?id=56519)

> **\*** If you are a US government cloud customer, use this link for Azure IP Ranges and Service Tags instead  
> [https://www.microsoft.com/en-us/download/details.aspx?id=57063](https://www.microsoft.com/en-us/download/details.aspx?id=57063)

## List of Domains for Update Content (Based on Products Enabled for Published)

You can also download a .CSV file with the list of possible domains, ports, and protocols that would need to be whitelisted.

- [Download CSV file for Firewall Allowlist](https://patchmypc.com/scupcatalog/downloads/PatchMyPC-DomainList.csv)

\[table id=30 /\]

**Keywords:** whitelist, firewall, exceptions, filter, filters, allowlist