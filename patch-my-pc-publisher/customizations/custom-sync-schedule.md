---
title: Custom Sync Schedule for Patch My PC Publisher
date: 2024-09-12T00:00:00.000Z
taxonomy:
  products:
    - patch-my-pc-publisher
  tech-stack:
    - null
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - customizations
    - general-configuration-and-usage
    - log-collection-and-analysis
---

# Custom Sync Schedule

There might be a scenario where you would like the publisher to sync at a frequency that is not available in the Sync Schedule tab. For example, every two weeks on Friday. This article will go over the current Publisher Sync Schedule options and how to use Task Scheduler to create a custom Publisher sync schedule.

### Current Publisher Sync Schedule Options

In the **Sync Schedule** tab of the Patch My PC Publisher, you can set the cadence for publishing new third-party updates and applications to your ConfigMgr, WSUS, and Intune environments. The Publisher runs as a service in the background on the device where it is installed. Based on the schedule you set, the service will run and evaluate if Patch My PC has released any new catalog updates for the products you have enabled.

You can also use the **Run Publishing Service Sync** option to trigger a manual or [Selective Sync.](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#SelectiveSync)

![](/_images/Sync-Schedule-12.png)

### How to Create a Custom Publisher Sync Schedule

You can use Task Scheduler to synchronize the Publisher on a schedule that isn’t available under the Sync Schedule tab. The following steps will guide you on how to use Task Scheduler to trigger a synchronization of the Publisher on a custom schedule.

First, open **Task Scheduler**.

![](/_images/Sync-Schedule-1.png)

Rights-Click **Task Scheduler Library** and select **Create Task**.

![](/_images/Sync-Schedule-2.png)

In the **Create Task** window, enter a name for the task under the **General** tab and set the **Security options**. You have a couple of options for the **Security options** section:

* Option 1: To run as a specific account (for example a Service Account) select **Run whether user is logged on or not** and enter the details required&#x20;
* Option 2: To run as System - select **Change User** and enter **System** then click **Check Names** and **OK.**&#x20;

We are going to use **Option 2** and run the Task as **System**.

![](/_images/Sync-Schedule-3-2.png)

![](/_images/Sync-Schedule-4.png)

Next, move to the **Triggers** tab and select **New**.

![](/_images/Sync-Schedule-6.png)

In the **Begin the task** drop down, select **On a schedule**.

Choose your frequency in the Settings section and select **OK**. In our example, we have chosen Weekly and Recur every 2 weeks on Friday.&#x20;

![](/_images/Sync-Schedule-7.png)

Next, move to the **Actions** tab and select **New**.

For the **Action** drop down, select **Start a program**.&#x20;

In the **Settings** section under **Program/script**, browse to the installation location of the publisher. The default location is **"C:\Program Files\Patch My PCPatch My PC Publishing ServicePatchMyPC-Settings.exe"**&#x20;

In the **Add arguments (optional)** section enter: **/syncnow**

![](/_images/Sync-Schedule-9.png)

Lastly, select **OK** to save the new task.

![](/_images/Sync-Schedule-10.png)

After the task is created, you can disable the sync schedule in the publisher, so it doesn't sync automatically outside the new scheduled task.

![](/_images/Sync-Schedule-11.png)