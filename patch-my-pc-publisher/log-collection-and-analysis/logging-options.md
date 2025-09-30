---
title: "Patch My PC Publisher Logging Options"
date: 2022-06-02
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
        - log-collection-and-analysis
        - education
        - troubleshooting
---

The Logging Options section of the publisher is where you can access helpful logs to view for more detailed information.

![](/_images/logging1-3.png)

## Collect Logs

The Collect Logs button allows you to quickly collect the relevant server-side logs that we need to do further troubleshooting.

To collect the logs:

1. Click on the **Collect Logs** button in the Publisher

3. Pick a Folder where you want to save the logs to

5. a ZIP file will be created in that location containing the relevant server-side logs needed for troubleshooting. You can then send that ZIP file using our [technical support form.](https://patchmypc.com/technical-support) 

![](/_images/logging-options-8-4.png)

## Open PatchMyPC.log

Select this button to open the **PatchMyPC.log** file. This log is helpful for viewing detailed information relevant to the Patch My PC publisher. If you have CMTrace installed, and it is set to be your default log viewer, the log will open in CMTrace. Otherwise, it will open in Notepad. You can also access the log in the PatchMyPC Install Directory.

![](/_images/Logging-Option-6-2.png)

## Open wsyncmgr.log

If you are using the publisher with ConfigMgr and your Software Update Point is on your Primary Site Server, you can access the **wsyncmgr.log** by selecting the option on the General tab. This is helpful to view the status of a Software Update Point synchronization for any new third-party updates. 

![](/_images/Logging-Option-9-2.png)

**Note:** If your Software Update Point is remote from your Primary Site or if you are using the Publisher for an Intune only environment, you will not see the **Open wsyncmgr.log** button under the **Logging Options** section.

## Logging Level

In the **Logging Level** section you can choose between **Information** and **Debug** logging for the PatchMyPC.log file. By default, Information logging will be selected. Enabling [Debug logging](https://patchmypc.com/collecting-log-files-for-patch-my-pc-support#enable-debug-logging) can be helpful when troubleshooting unique issues with publishing.

**Logs to Retain:** This is the number of the PatchMyPC.log files that will be retained. The maximum number you can set here is 10.

**Max Size in MB:** This is the maximum size for the PatchMyPC.log. When the max size is reached a new PatchMyPC.log will be created. The maximum number you can set here is 10.

![](/_images/logging-options-2-1.png)