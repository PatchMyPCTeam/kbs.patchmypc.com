---
title: "SCCM Update Download Error: The Thread is not in Background Processing Mode (0x80070191)"
date: 2018-10-27
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

We have been seeing some instances where third-party software updates will fail to download into a deployment package when **WSUS is configured to use a UNC share for the WSUS Content folder**.

## Determine if You are Affected

When attempting to download a third-party update from the Configuration Manager console, you receive the following message in the console **Error: The thread is not in background processing mode.**

![SCCM Download Failed The thread is not in background processing mode. 0x80070191](images/The-thread-is-not-in-background-processing-mode.png)

If you open the PatchDownloader.log in the **%temp%** folder, you can also find the following errors.

HttpSendRequest failed HTTP\_STATUS\_FORBIDDEN or HTTP\_STATUS\_DENIED

ERROR: DownloadContentFiles() failed with hr=0x80070191

This error happens because the **user identity** configured in the WSUS Administrator **Content** virtual directory can't authenticate to a **remote UNC share** specified for the **[WSUSContent folder](/clean-up-third-party-updates-from-the-wsus-updateservicespackages-folder#wsuscontent)** .

## Troubleshooting Step 1: Verify the Content Virtual Directory is Correct

Validate the **Content virtual directory** in the **WSUS Administration** website is correct.

There is a bug in the WSUS setup process that can remove the leading when using a **UNC path** for the WSUSContent folder.

To check the **Physical Path** for the Content virtual directory: **Right-click Content** > **Manage Virtual Directory** > **Advanced Settings...**

![WSUS Content Virtual Directory Physical Path](images/WSUS-Content-Virtual-Directory-Physical-Path.png)

Here's an example of the WSUS bug where the leading are removed from the virtual directory **Physical Path**. Because the directory is invalid in the Content virtual directory updates will fail to download with error **0x80070194** or **0X87D20417**.

![WSUS Bug Removed Backslashs in UNC Path](images/WSUS-Bug-Removed-Backslashs-in-UNC-Path.png)

If you are affected by this scenario, simply update the path to include the leading as shown below and click **OK**

![Add Leading Backslashs in UNC Path for WSUS Content Directory](images/Add-Leading-Backslashs-in-UNC-Path-for-WSUS-Content-Directory.png)

## Troubleshooting Step 2: Is WSUSContent a UNC Share with the Default IUSR Authentication

If steps 1 and 2 are correct, we often see customers where a **UNC path** is being used. Still, the authentication identity hasn't been changed to use the **Application pool identity** for the **Content virtual directory**. Perform the following actions to review and resolve this issue if applicable:

**Click** the **Content virtual directory** > Double Click **Authentication** in the **IIS Group**

![Click Anonymous Authentication WSUS](images/Click-Anonymous-Authentication-WSUS.png)

**Right-Click** the **Anonymous Authentication** and Click **Edit**

![Check Anonymous Authentication WSUS](images/Check-Anonymous-Authentication-WSUS.png)

 Here you can review the **Anonymous user identity** used for authenticating to the WSUSContent directory. By default, the authentication is set to use the specific user: **IUSR**. IUSR is a local user-specific to the WSUS server and will not have access to a authenticate to any **remote UNC share**.

In the **Edit Anonymous Authentication Credentials** dialog for the **Anonymous Authentication** ensure it is set to use the **Application pool identity**

![Use Application Pool Identity for Content](images/Use-Application-Pool-Identity-for-Content.png)

## Resolution for DownloadContentFiles() failed with hr=0x80070191 (Video Guide)

The video guide below goes into more detail about why the downloads fail with error **0x80070191** and the possible resolutions.

Please start at **20:18** and watch to **24:30** for the resolution.

<iframe src="https://www.youtube.com/embed/y7w7hBSHShc?start=1218" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>

## Resources

- **IIS LoginMethod** - [https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/site/application/virtualdirectory#configuration](https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/site/application/virtualdirectory#configuration)

- **IIS 401.3 Error Code** - [https://support.microsoft.com/en-us/help/943891/the-http-status-code-in-iis-7-0-iis-7-5-and-iis-8-0](https://support.microsoft.com/en-us/help/943891/the-http-status-code-in-iis-7-0-iis-7-5-and-iis-8-0)

- **Managing WSUS from the Command Line** - [https://docs.microsoft.com/de-de/security-updates/windowsupdateservices/18127395](https://docs.microsoft.com/de-de/security-updates/windowsupdateservices/18127395)
