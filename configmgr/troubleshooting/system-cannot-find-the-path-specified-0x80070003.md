---
title: "DownloadUpdateContent() failed with hr=0x800070003 - The system cannot find the path specified"
date: 2020-04-29
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
        - common-issues-and-error-codes
        - troubleshooting
        - deployments
---

## Error 0x80070003 - Downloading Updates to a Deployment Package Using an Automatic Deployment Rule

This article discusses a common issue seen when an Automatic Deployment Rule (ADR) is set up to download updates from a specific UNC path rather than to download updates using the internet.

If your ADR is configured to download from a specific UNC path, you may see an error in your **SMS\_CCM\\LogsPatchDownloader.log** similar to the one seen below:

ERROR: DownloadUpdateContent() failed with hr=0x80070003 **OR** ERROR: DownloadContentFiles() failed with hr=0x80070003

Looking up this error in CMTrace yields this explanation: The system cannot find the path specified.

### Resolution: Set ADR Download Location to Download Software Updates From the Internet

This error occurs because the ADR was initially configured to "**download software updates from a location on my network**" that does not contain the update it attempted to download, hence the "**The system cannot find the path specified**" error description.

A common mistake here is that the UNC path specified in the download location is set to a location intended to download the updates **_to_**, such as a deployment package, when it is asking where to download updates **_from_**.

![specify location ADR downloads updates from](/_images/ADR-Download-Location-Config.png "specify location ADR downloads updates from")

You do not need to designate a specific UNC path to download Patch My PC third-party updates. When setting up an ADR, you will need to configure the deployment package to "**Download software updates from the Internet**" instead of a UNC path. The ADR download will automatically use the WSUSContent website where the update was published when the option "**Download software updates from the Internet**" is checked.

![ERROR: DownloadUpdateContent() failed with hr=0x80070003](/_images/download-from-internet-3rd-party-updates-SCCM.png "ERROR: DownloadUpdateContent() failed with hr=0x80070003")

Unfortunately, this is not a setting you can change in the properties of an ADR after it is created. You will need to create a new ADR to configure the download location, as seen in the picture above.

Deleting the previously failing ADR and configuring the new one to download updates from the Internet should resolve this error.