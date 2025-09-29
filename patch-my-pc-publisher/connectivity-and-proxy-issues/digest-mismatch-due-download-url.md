---
title: "Digest Mismatch Due To Download URL Being Filtered by Firewall or Web Filter"
date: 2019-02-16
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

If you received an error message in the [PatchMyPC.log](/collecting-log-files-for-patch-my-pc-support#publishing-service-logs) similar to the one shown below when publishing a third-party update this article should help.

Digest of the downloaded update doesn't match the digest from the catalog: Hash from catalog doesn’t match downloaded update hash of  
This error appears to be a known error. Please see our KB article https://patchmypc.com/digest-mismatch-download-filtered for the resolution.

## Why the Hash Check?

Whenever we download an update file from a vendor's website to publish to WSUS/SCCM, we validate the **hash of the current binary downloaded** matches the **original hash within the catalog metadata**. This check ensures that **if a binary is compromised or changed** on the vendors websites, we will **not publish the software update**. We have a deep dive into our [security validation process here](https://patchmypc.com/deep-dive-into-security-validation-of-third-party-software-updates-in-microsoft-sccm).

## Review Download Failures from Firewall Rules for Previous Synchronizations

We store the **last download response code and domain for currently enabled products**. This can be helpful when reviewing download failures for currently enabled products.

The download history can be found on a CSV file in: **%InstallDir%\\PatchMyPC-DownloadHistory.csv**

![PatchMyPC-DownloadHistory.csv File for Download History](/_images/PatchMyPC-DownloadHistory-csv-File-for-Download-History.png "PatchMyPC-DownloadHistory.csv File for Download History")

Here's an example of the data included in the **PatchMyPC-DownloadHistory.csv file**. You can use the **domain column to create firewall exceptions** based on the products enabled and failing to download.

![PatchMyPC-DownloadHistory.csv Example List](/_images/PatchMyPC-DownloadHistory-csv-Example-List.png "PatchMyPC-DownloadHistory.csv Example List")

## Resolution to this Specific Hash Check Failure

If you received the error message above that links our to this article, that means the **hash check failed**, and **the downloaded file size was less than 100 kb**.

When the downloaded file is **less than 100 kb** in size, this almost always correlates to a **web filter** or **firewall blocking the download** from the server that is running our publishing service.

**Step 1** - **Open the PatchMyPC.log** from the **publishing service.**

![PatchMyPC-Publishing-Service-Open-PatchMyPC-Log-File-UI](/_images/PatchMyPC-Publishing-Service-Open-PatchMyPC-Log-File-UI.png "PatchMyPC-Publishing-Service-Open-PatchMyPC-Log-File-UI")

**Step 2 - Copy the download URL from the PatchMyPC.log** for any updates **receiving this hash error.** ![Copy-Download-URL-For-Update-Hash-Error-Due-To-Filtering](/_images/Copy-Download-URL-For-Update-Hash-Error-Due-To-Filtering.png "Copy-Download-URL-For-Update-Hash-Error-Due-To-Filtering")

**Step 3 - Paste the download URL into a web browser** on the same server running the publishing service and check if you receive an **error that a web filter is blocking the download**.

![Download URL Being Filtered in Browser](/_images/Download-URL-Being-Filter-in-Browser.png "Download URL Being Filtered in Browser")

**Note:** Patch My PC Publisher will attempt to download files from the internet in the **SYSTEM** context. This must be considered when using proxy servers or firewalls that require authentication.

**Step 4 -** You will need to get exceptions created for any downloads receiving this hash error for "**digest mismatch download filtered**".

A full list of possible domains used for products in our catalog can be found at [https://patchmypc.com/list-of-domains-used-for-downloads-in-patch-my-pc-update-catalog](https://patchmypc.com/list-of-domains-used-for-downloads-in-patch-my-pc-update-catalog)