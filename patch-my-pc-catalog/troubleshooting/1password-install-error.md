---
title: "1Password was unable to complete installation and will roll back any changes."
date: 2022-06-16
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

The installation of 1Password fails, and when running manually, the error message “1password was unable to complete installation and will roll back any change” appears.

## Determine if You are Affected

If the installation of 1Password fails, locate the installer in the cache or [download](https://downloads.1password.com/win/1PasswordSetup-latest.exe) the installer from the vendor's website and attempt the installation manually.  
This error should appear:

![](/_images/ApplicationFrameHost_FWqmtYCXwV.png)

## Potential Resolution

This error is generally caused due to 1Password being blocked from updating/installing by another program. Make sure you don’t have any pending restarts or any pending browser updates to be installed.  
The easiest fix is to restart the device, then retry the installation.