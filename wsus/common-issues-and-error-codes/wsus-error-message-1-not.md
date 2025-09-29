---
title: "WSUS Error Message %1 is not a valid Win32 Application | failed to sign package; error was: 2147942593"
date: 2020-05-12
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

In this article, we're going to cover an issue that we periodically see related to **WSUS** and the [WSUS Signing Certificate](/wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager).

This issue generally occurs when attempting to **import/create** a new WSUS signing certificate, and it may cause the **WSUS signing certificate to not be properly detected** even when it exists in the WSUS store in **certlm.msc**.

![](/_images/certlm-wsus-certificate.png)

## Errors Related to %1 is not a valid Win32 application

If you are affected by this issue, you will likely see the following errors in the **patchmypc.log**.

Export WSUS Certificate to C:\\Users\\%username%\\AppData\\Local\\Temp\\PMP-nia3xn2d5wszzsvn.tmp  
An error occurred while extracting the certificate from WSUS: C:\\Users\\%username%\\AppData\\Local\\TempPMP-nia3xn2d5wszzsvn.tmp : %1 is not a valid Win32 application  
Failed to extract the certificate from WSUS

If an update is being published, you will likely see an error

An error occurred while publishing an update to WSUS: Failed to sign package; error was: 2147942593

Depending on the build of the Publisher, you may also receive the error "_**No certificate found in the WSUS certificate store on this server.**_"

![No certificate found in the WSUS certificate store on this server.](/_images/No-certificate-found-in-the-WSUS-certificate-store-on-this-server.png "No certificate found in the WSUS certificate store on this server.")

## Resolution to %1 is not a valid Win32 application

The error **%1 is not a valid Win32 application** generally occurs due to a rogue file named "**C:\\Program**" that exists on the WSUS server. This file seems to cause issues related to the WSUS certificate being properly detected and imported.

We've seen the rogue file often is a log file for **ccmsetup.exe** or some other application log file that wasn't properly saved.

In some versions of Windows, you may even receive a warning on login about issues that can occur when "**C:\\Program**" file exists.

![](/_images/file-name-warning.png)

The fix is to rename the "**C:\\Program**" to "**C:\\Program1**" or you can delete the file if you determine it's not needed.

After renaming or deleting the file, you can restart the "**WSUS Service**" and the "**WSUS Certficate Server**" from **services.msc**