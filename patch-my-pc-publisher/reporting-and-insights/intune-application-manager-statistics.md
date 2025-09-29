---
title: "Intune Application Manager Application Statistics Not Populating"
date: 2023-06-08
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

## UPDATE

This issue was fixed in the Patch My PC Publisher [production build 2.1.16.](https://docs.patchmypc.com/release-history/production-releases#2.1.16-2023-07-06)

## Issue

The Patch My PC Intune Scan Wizard does not populate any application statistics.

### Current Intune Application Manager functionality

### 
![](/_images/CurrentFunctionality.png)

###  Expected Intune Application Manager functionality

![](/_images/ExpectedFunctionality.png)

## Cause

### Microsoft has removed the following Graph API Endpoints

The Patch My PC Intune Scan Wizard uses these endpoints to gather information on the installation status of applications and updates created by the Patch My PC Publisher.

- /deviceAppManagement/mobileApps/{mobileAppId}/installSummary
    

![](/_images/InstallSummary.png)

- /deviceAppManagement/mobileApps/{mobileAppId}/deviceStatuses
    

![](/_images/deviceStatuses.png)

- /deviceAppManagement/mobileApps/{mobileAppId}/userStatuses
    

![](/_images/userStatuses.png)

(08/062023) There is no mention of this breaking change in the ChangeLog for Graph API

(09/06/2023) Microsoft are retroactively adding this change to the Changelog

[Changelog - Microsoft Graph](https://developer.microsoft.com/en-us/graph/changelog/?filterBy=&from=2023-06-01&search=)

## Resolution

Microsoft has confirmed that a change was put forward earlier this year to remove these endpoints; however, that change was not publicly visible.

Patch My PC is rewriting the Intune App Manager functionality to use a different Graph API endpoint that provides the same data but in a different format, as Microsoft will not be reverting the change and restoring those endpoints.