---
title: Modify, Delete, and Decline Third-Party Updates in WSUS with Patch My PC
date: 2020-05-10T00:00:00.000Z
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
    - general-configuration-and-usage
    - application-and-update-publishing
    - workarounds
---

# Modify Delete Decline Third Party

You can **view**, **delete**, **decline**, and show **applicability rules**, much more for third-party updates within WSUS using the **modify published updates** wizard.

The **modify published updates wizard** can help perform various actions on third-party updates that are published to WSUS. The wizard can be accessed with the button shown below in the **Options** window that can be opened from the **Updates tab** in the **Publisher**.

Open the **Modify Published Updates** wizard in the **Options** window found in the updates tab.

![Publisher - Updates - Options - Run Wizard](/_images/ModifyPublishedUpdatesLoc.png "Publisher - Updates - Options - Run Wizard")

### Filter Selections for Published Updates

Once all updates have loaded, you can easily filter the list of published updates.

![Modify Updates Wizard with filters highlighted](/_images/Modify-Updates-Filters.png "Modify Updates Wizard with filters highlighted")

You can filter updates by **vendor**, **declined** status, **expired** status, **metadata** status, **superseded** status, or **enabled** status.

> If you select multiple updates in the list view, you can check the checkbox by clicking the space key on your keyboard.

> **Note**: The filter for **Enabled Status** can be a great option to **decline** or **delete** old updates for products not currently enabled. If you filter by those that are not enabled, you will find all WSUS updates which are published, but no longer selected in the list of products.Â&#x20;

### Available Actions to Perform on Published Third-Party Updates

There are a variety of actions that are available for published third-party updates within WSUS.

![Modify updates wizard buttons highlighted](/_images/Modify-Updates-Buttons.png "Modify updates wizard buttons highlighted") Patch My PC Modify Updates Wizard Available Options

#### Re-Sign Update

The re-sign update will allow you to re-sign an already published update with a new [WSUS Code-Signing Certificate](../../wsus-signing-certificate-options-for-third-party-updates-in-configuration-manager/). Re-signing can be helpful if your WSUS Code-Signing certificate expired and [timestamping was disabled](../../how-to-disable-timestamping-for-patch-my-pc-update-publishing/). Important: If timestamping is enabled (enabled by default), clients would still **trust updates signed after the certificate's expiration date**.

> **Important**: If updates are re-signed, you will need to **delete all re-signed third-party updates from your ConfigMgr deployment package**, trigger a software update sync, and **redownload** the updates for re-signed updates to be used by clients.

#### Decline

This option will [**decline**](https://docs.microsoft.com/en-us/windows-server/administration/windows-server-update-services/manage/updates-operations#declining-updates) the third-party update in WSUS. When a third-party update is declined, clients will no longer scan against the third-party update. After an update is declined and a software update point sync is performed, the update will show as [**expired**](https://docs.microsoft.com/en-us/mem/configmgr/sum/understand/software-updates-icons#expired-icon) in the Configuration Manager console.

#### Un-Decline

This option will revert the declined status in WSUS. Clients will start to **scan against this update,** and the status will show as [normal](https://docs.microsoft.com/en-us/mem/configmgr/sum/understand/software-updates-icons#normal-icon) in Configuration Manager after the next software update point sync.

#### Delete

This option will **completely delete the update** from the WSUS database. We don't recommend deleting an update if the product for the update is still enabled for publishing.

> The delete update button **is not enabled by default** because it can cause hash issues if an update is deleted, and that product is still enabled, causing the same UpdateID to be republished. If you have a scenario to delete updates, such as cleaning up [vendors from WSUS](https://patchmypc.com/an-error-occurred-while-publishing-an-update-to-wsus-publishing-operation-failed-too-many-locally-published-categories) you can enable the button using the value below:
>
> &#x20;REG ADD "HKLM\SOFTWARE\Patch My PC Publishing Service" /v EnableDeleteUpdates /t REG\_DWORD /d 1 /f

#### Show in WSUS

This option will configure the update to **show directly in the WSUS console**. By default, third-party updates aren't visible directly in the WSUS console, and if you are using configuration manager, they don't need to be visible. This option can be helpful when using standalone WSUS for updates.

#### Hide in WSUS

This option will configure the update to **not show in the WSUS console**. By default, third-party updates aren't visible directly in the WSUS console.

#### Show Applicability Rules

This option can be helpful if you are [**troubleshooting the update detection states**](../../how-to-view-applicability-rules-and-troubleshoot-detection-states-for-third-party-updates/) on a device. When clicked, you will be able to see the applicability for the **installable** and **installed** logic for the update.

![Show Applicability Rules](/_images/applicability-rules-third-party-update.png "Show Applicability Rules")

#### More Details

This option will show more **advanced details** about the published updates in WSUS. This may be helpful for more **troubleshooting scenarios**.

![Third-Party Update More Details](/_images/more-details-of-third-party-update-in-wsus.png "Third-Party Update More Details")

#### Extract Content

This option will open a a window and allow you to save the WSUS content for the specified update in the selected location. Update content is saved as a .cab file in the selected extraction folder.