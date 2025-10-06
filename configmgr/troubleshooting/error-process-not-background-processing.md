---
title: >-
  Error: The process is not in background processing mode. ERROR:
  DownloadContentFiles() failed with hr=0x80070193
date: 2019-04-11T00:00:00.000Z
taxonomy:
  products:
    - null
  tech-stack:
    - configmgr
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - troubleshooting
    - common-issues-and-error-codes
    - known-issues-and-considerations
---

# Error Process Not Background Processing

When attempting to download third-party software updates into an SCCM deployment package, you receive the following error message in the console: **Failed to download content id . Error: The process is not in background processing mode.**

![](/_images/download-error-0x80070193-1.png)

If you review the [**PatchDownloader.log file for ADR's**](../../collecting-log-files-for-patch-my-pc-support/#automatic-deployment-rules-logs) or [PatchDownloader.log when downloading using the console](../../collecting-log-files-for-patch-my-pc-support/#deployment-package-download-logs), you will see errors similar to the errors listed below:

HttpSendRequest failed HTTP\_STATUS\_FORBIDDEN or HTTP\_STATUS\_DENIED

ERROR: DownloadContentFiles() failed with hr=0x80070193

#### Why Does ERROR: DownloadContentFiles() failed with hr=0x80070193?

This error occurs when the WSUS **Content virtual directory is configured to require SSL**. In Internet Information Services (IIS) Manager, click the **WSUS Administration website**, click the **Content** virtual directly, then click **SSL Settings** in the right pane.

####

![](/_images/wsuscontent-ssl-settings-in-IIS.png)

If Require SSL is checked, **uncheck** that option and choose to **apply** the changes

![](/_images/wsuscontent-ssl-settings-in-IIS-1.png)

The WSUS **Content virtual directly in IIS should not be configured to Require SSL** even if your WSUS server is configured in SSL. Please see the following Microsoft documentation for more details: [**How to Configure the WSUS Web Site to Use SSL**](https://docs.microsoft.com/en-us/mem/configmgr/sum/get-started/software-update-point-ssl)&#x20;

If this was your issue, third-party updates should now successfully download to an SCCM deployment package.

![](/_images/sccm-third-party-update-download-working.png)