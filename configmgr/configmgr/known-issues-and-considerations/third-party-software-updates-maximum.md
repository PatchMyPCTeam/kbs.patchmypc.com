---
title: "Third Party Software Updates Maximum Runtime in ConfigMgr"
date: 2022-02-08
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
        - known-issues-and-considerations
        - best-practices
        - troubleshooting
---

This article will detail the requirement to increase the maximum runtime for third party software updates in Microsoft Configuration Manager (SCCM / MEMCM / MECM / ConfigMgr).

## Maximum Run Time Explained

All software updates in Configuration Manager have a configured maximum allowed time to run, measured in minutes. This is referred to as maximum run time. It is the allotted time for software updates to complete on client computers. If the update **takes longer than the maximum run-time value, Configuration Manager stops tracking the software updates installation** and **returns a failure** status message.

The software update installation will continue in the background, however **the client will no longer track its progress nor return its actual result**.

You may find after the next time the clients performs a scheduled software update scan evaluation cycle, the update may eventually report back as compliant, however this assumes the installation was a success.

> **Maximum Run Time** specifies the maximum number of minutes allotted for a software update installation to complete before the installation is stopped by Configuration Manager. This setting is also used to determine whether there is enough available time remaining to install the update before the end of a maintenance window. The default setting is 60 minutes for service packs. For other software update types, the default is 10 minutes if you did a fresh install of Configuration Manager version 1511 or higher and 5 minutes when you upgraded from a previous version. Values can range from 5 to 9999 minutes.
> 
> \- Microsoft documentation **[Configure software update settings | Set maximum run time](https://docs.microsoft.com/en-us/mem/configmgr/sum/get-started/manage-settings-for-software-updates#BKMK_SetMaxRunTime)**.
> 
> Some products in the Patch My PC catalog take **longer than the default maximum run timeout in ConfigMgr**, for example Autodesk Revit.

Microsoft expose options to change the default maximum run timeout for different types of software updates in Configuration Manager. **However, at the time of writing this, these options do not apply to third party updates** (see image below).

![ConfigMgr SUP updates max run time](images/SUPMaximumRunTime.png)

You can check your updates' current maximum run time value by adding the column to the All Software Updates view in-console for **Maximum Run Time (Mins).**

![In-console view of Maximum Run Time (Mins) for all software updates](images/SCCMConsoleMaxRunTimeColumn.png)

You can also check an individual update's maximum run time by right clicking on the software update > **Properties** > **Maximum Run Time** tab.

![A particular software update's maximum run time configuration](images/SoftwareUpdateMaxRunTime.png)

## Determine if You Are Affected

On the client side, when a software update exceeds the maximum allowed run time, you can expect to see the following:

Execution cancelled by job. Update will not be monitored **(UpdatesHandler.log)**

CTargetedUpdate::RaiseEnforcementStateMessage, m\_eUpdateAction = 1 **(UpdatesDeployment.log)**  
CTargetedUpdate::RaiseEnforcementStateMessage, ulMessageId = -1073730111 **(UpdatesDeployment.log)**  
CTargetedUpdate::RaiseEnforcementStateMessage, m\_hrError = -2016410012 **(UpdatesDeployment.log)**  
CTargetedUpdate::RaiseEnforcementStateMessage, MessageUserFlags = STATE\_USERFLAG\_CI\_USER\_ENFORCED **(UpdatesDeployment.log)**  
Raised update (TopicID) (Site\_x/SUM\_x) enforcement state message successfully. TopicType = STATE\_TOPICTYPE\_SUM\_CI\_ENFORCEMENT, StateId = 11, StateName = CI\_ENFORCEMENT\_UPDATE\_FAILED, StateCriticality = 1, TopicIdVersion = 1 **(UpdatesDeployment.log)**  
Update (Site\_x/SUM\_x) Progress: Status = ciStateError, PercentComplete = 0, DownloadSize = x, Result = 0x87d00664 **(UpdatesDeployment.log)**

**0x87d00664** resolves to **Updates handler job was cancelled**.

![Updates handler job was cancelled](images/2022-02-01_15-36-58.png)

## Changing the Maximum Run Time Value

For accurate reporting of software update installation, it is recommend to change the maximum run time for updates which typically exceed the default run time. Adjust its value by right clicking on the software update > **Properties** > **Maximum Run Time** tab:

![A particular software update's maximum run time configuration](images/SoftwareUpdateMaxRunTime.png)

> **Note:** if the software update was already deployed and you change its maximum run time value, in order for the client to recognise the changed maximum run time value **you must remove the deployment and re-deploy it**.

## Solution Moving Forward

Microsoft released a Technical Preview version of Configuration Manager which exposes the option on the Software Update Point to change the default maximum run time for all other software pudates, which includes third party software updates.

See the release notes for **[Technical Preview 2112](https://docs.microsoft.com/en-us/mem/configmgr/core/get-started/2021/technical-preview-2112#bkmk_maxruntime)** when they added this improvement.

Customers using Configuration Manager must wait for Microsoft to merge this change into the production Current Branch versions of Configuration Manager.

Until Microsoft implement this feature for all customers, you must **update the maximum run time value for all software updates that need it**.

At this time, we are aware of the following updates that require this change (_this list not is not maintained_):

- Autodesk Revit
