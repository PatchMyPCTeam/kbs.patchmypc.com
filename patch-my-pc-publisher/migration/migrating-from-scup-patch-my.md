---
title: Migrating from SCUP to Patch My PC Publisher
date: 2024-09-01T00:00:00.000Z
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
    - migration
    - best-practices
    - updates
---

# Migrating From Scup Patch My

In a Microsoft Configuration Manager environment, our customers have traditionally used one of three primary methods to accomplish third-party patching:-

1. [System Center Updates Publisher](https://learn.microsoft.com/en-us/mem/configmgr/sum/tools/updates-publisher) (SCUP)
2. [In-console publishing](https://learn.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates) within Microsoft Configuration Manager
3. Patch My PC Publisher (on-premises publishing tool)

Microsoft announced the end-of-life of System Center Updates Publisher (SCUP) in January 2024. Customers who subscribe to our Basic licence and use SCUP should now transition to an alternative, supported, publishing method. You can read more about the SCUP deprecation at [https://patchmypc.com/system-center-update-publisher-deprecated](https://patchmypc.com/system-center-update-publisher-deprecated)

> **Important**
>
> While we strongly encourage customers to transition to using our on-premises publishing tool 'Patch My PC Publisher', this does not need to be an 'either-or' migration in your early testing phase. You can test and use the new solution side-by-side with SCUP as you gain familiarity with the new tooling.
>
> As Microsoft are no longer supporting SCUP, we do recommend customers to migrate to Patch My PC Publisher by January 31st, 2025. We do not know if/when Microsoft might release a breaking change to the operating system that SCUP is installed on.

### What is Patch My PC Publisher?

Patch My PC Publisher is a comprehensive tool, installed on a single Windows operating system, used for customizing and publishing third-party applications and updates to WSUS, Configuration Manager and Intune.

![Patch My PC Publisher](/_images/publisher.jpg "Patch My PC Publisher")

Some advantages of using the Patch My PC Publisher over SCUP include:-

* The ability to create applications in both Configuration Manager and Intune (requires an Enterprise Plus subscription).
* Many [customization options](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications).
* Modern features such as supporting [custom apps](https://docs.patchmypc.com/patch-my-pc-cloud/custom-apps) (requires an Enterprise Plus subscription), webhook notifications and more.
* Automatically detect software based on Configuration Manager hardware inventory and keep applications up-to-date automatically.
* Email and Webhook notifications.
* Multiple options to synchronize the catalog.
* Integration with [Patch My PC Cloud](https://docs.patchmypc.com/patch-my-pc-cloud) (requires an Enterprise Plus subscription).
* No agent required for client devices.
* Single console installation on the Software Update Point (SUP).
* Automatically update applications in a Task Sequence.
* Automatically clean-up un-needed WSUS content.

Current limitations of the Publisher include:-

* Publisher cannot publish updates to a remote WSUS server. \*
* Console access to modify and customize updates is only possible from the Software Update Point where the Publisher tooling is installed (either directly or by using RDP).

\* While the Publisher cannot publish updates to a remote WSUS server, it is important to note that the Publisher can be installed on a remote Software Update Point (SUP).

### Feature Comparisons

The following table highlights the feature differences between SCUP, In-Console Publishing method and Patch My PC Publisher:-

| **Feature**                                                                                                                                                                                                                                                                                                                  | **SCUP**                                                               | **In-Console Publishing** | **Patch My PC Publisher** |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | ------------------------- | ------------------------- |
| Publish third-party updates to [WSUS/ConfigMgr](https://docs.patchmypc.com/installation-guides/configmgr/updates?_gl=1*95zk82*_gcl_aw*R0NMLjE3MTc3NzcwNDUuQ2p3S0NBanczNHF6QmhCbUVpd0FPVVFjRjQ4VU1IazRNMUpJVHJ6bW5WaTNlRnM5Zm1lZVFCYTlIVF9XNWpsejA5enFZcTZoWnJEakJob0NRYjhRQXZEX0J3RQ..*_gcl_au*MTM3MjI4Nzg3MC4xNzIxODk3ODAw) | Yes                                                                    | Yes                       | Yes                       |
| Automatically publish updates on a scheduled sync                                                                                                                                                                                                                                                                            | -                                                                      | Yes                       | Yes                       |
| Publish updates to a remote WSUS server                                                                                                                                                                                                                                                                                      | Yes                                                                    | Yes                       | -                         |
| Disable [self-update feature](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#disable-updates) for updates                                                                                                                                                                           | -                                                                      | -                         | Yes                       |
| Interactively [notify users](https://patchmypc.com/manage-conflicting-processes-when-updating-third-party-applications) when installing applications                                                                                                                                                                         | -                                                                      | -                         | Yes                       |
| Remove [public desktop shortcuts](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#delete-shortcut) post update                                                                                                                                                                       | -                                                                      | -                         | Yes                       |
| Add custom [pre/post update scripts](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#custom-scripts)                                                                                                                                                                                 | -                                                                      | -                         | Yes                       |
| Modify installation commands for updates                                                                                                                                                                                                                                                                                     | Yes (but modifications are not maintained for new versions of updates) | -                         | Yes                       |
| Save update [install logs](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#install-logging) to a central location                                                                                                                                                                    | -                                                                      | -                         | Yes                       |
| Add [MST Transform Files](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#mst-transform) to updates                                                                                                                                                                                  | -                                                                      | -                         | Yes                       |
| Fully customize [third-party update packages](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications)                                                                                                                                                                                       | -                                                                      | -                         | Yes                       |
| Automatically publish updates on a [scheduled sync](https://docs.patchmypc.com/installation-guides/configmgr/sync-schedule)                                                                                                                                                                                                  | -                                                                      | -                         | Yes                       |
| Automatically create [ConfigMgr applications](https://patchmypc.com/base-installations-now-available-for-microsoft-sccm)                                                                                                                                                                                                     | -                                                                      | -                         | Yes                       |
| Automatically create [Intune applications](https://patchmypc.com/introducing-automated-win32-application-management-for-microsoft-intune)                                                                                                                                                                                    | -                                                                      | -                         | Yes                       |
| Add [custom applications](https://docs.patchmypc.com/installation-guides/custom-apps)                                                                                                                                                                                                                                        | -                                                                      | -                         | Yes                       |
| Use [Intune Filters](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#filters) for assigning applications                                                                                                                                                                             | -                                                                      | -                         | Yes                       |
| [Pause](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#pause-product-updates) application creation for specific products                                                                                                                                                            | -                                                                      | -                         | Yes                       |
| Add [categories](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#ManageCategories) to applications for easy sorting                                                                                                                                                                  | -                                                                      | -                         | Yes                       |
| Add [security scopes](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#security-scopes) to applications in ConfigMgr                                                                                                                                                                  | -                                                                      | -                         | Yes                       |
| Add [scope tags](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#Manage-RoleScopeTags) to applications in Intune                                                                                                                                                                     | -                                                                      | -                         | Yes                       |
| Add applications to [ESPs for Autopilot in Intune](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#Manage-ESP)                                                                                                                                                                       | -                                                                      | -                         | Yes                       |
| Set [applications as featured](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#featured-applications) in ConfigMgr and Intune                                                                                                                                                        | -                                                                      | -                         | Yes                       |
| Dynamically deploy updates in Intune based on [security data](https://patchmypc.com/manage-dynamic-assignments)                                                                                                                                                                                                              | -                                                                      | -                         | Yes                       |
| Top-ranked [customer support](https://patchmypc.com/frequently-asked-questions#support) included                                                                                                                                                                                                                             | Yes                                                                    | Yes                       | Yes                       |
| Live chat                                                                                                                                                                                                                                                                                                                    | Yes                                                                    | Yes                       | Yes                       |
| Tailored onboarding                                                                                                                                                                                                                                                                                                          | Yes                                                                    | Yes                       | Yes                       |
| Dedicated account executive                                                                                                                                                                                                                                                                                                  | Yes                                                                    | Yes                       | Yes                       |

### Publisher Requirements

There may be some additional requirements for the Publisher when you transition away from using SCUP, depending on your existing server specification. The Publisher installs on the top-level Software Update Point in your Configuration Manager hierarchy. The SUP must meet the following requirements:-

**Software**

* Windows Server 2012, Windows Server 2016, Windows Server 2019, Windows Server 2022
* Microsoft .NET Framework 4.6.2.
* Windows Server Update Services (WSUS) installed and configured. (This feature should already be installed and configured if you are currently using SCUP).

**Environment**

* Internet connection.
* The [relevant domain names](https://patchmypc.com/list-of-domains-used-for-downloads-in-patch-my-pc-update-catalog) for Patch My PC have been added to your firewall's allow list.
* Install the Publisher on the top-level WSUS/SUP server in the hierarchy.
* The Patch My PC Publisher will require the user launching the Publisher tool to be a local administrator on the server.
* Configuration Manager Console installed (Only required to communicate with the SMS Provider to initiate the SUP sync and create Configuration Manager application).

**Hardware**

* CPU: 2 CPU or more.
* Memory: 8GB of RAM or more.
* Disk Space: 80GB of disk space or more. (The amount of disk space required will depend on the number of selected products).

### Migration Overview

The transition from using SCUP to publishing third-party updates to Patch My PC Publisher is the recommended migration path for Patch My PC customers. Typically, customers would need a different license level which is [covered at the end of this article](migrating-from-scup-patch-my.md#licensing).

For the most part, migration is relatively straight forward. There are a handful things to consider before migration, some of which are service affecting. Those items are covered in detail in the section below and in the [video](migrating-from-scup-patch-my.md#migration-video) and [documentation](migrating-from-scup-patch-my.md#migration-documentation) sections of this article.

### Migration Considerations

Patch My PC engineers will help customers transition from SCUP to using the Patch My PC Publisher. Patch My PC will offer technical guidance over email or Teams and can assist with platform migration over a remote support session. The following items should be considered before transitioning from SCUP to Patch My PC Publisher:-

**Non Service-Affecting Considerations**

* Existing updates from the Patch My PC catalog, published using SCUP, will not be removed from the WSUS or Configuration Manager database.
* If a newer version of an update is published using the Patch My PC Publisher, older updates already published to WSUS using SCUP will be superseded as normal/expected.
* API and URL endpoints remain unchanged.
* Existing WSUS SSL certificates do not need to be changed.
* Existing WSUS code-signing certificate does not need to be renewed if either the following conditions are met:-
  * The private key of the certificate is marked as exportable.
  * SCUP is already installed on the top-level SUP and the code-signing certificate is in the WSUS certificate store for the local computer.

**Service Affecting Considerations**

* Any edits to the imported catalog to customize updates created by SCUP must be considered and manually added back when creating the updates using Patch My PC Publisher. Not all edits may be possible with the Publisher. See [https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications) to understand what customizations can be applied to updates using the Publisher.
* If a previous version of an update was customized using SCUP, and Patch My PC Publisher performs a synchronization, those customizations will be overwritten. The update will be revised if the version synchronized by Patch My PC Publisher matches the existing version that was previously published using SCUP. Any [customizations](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications) made to updates of the same version, that have already been published using SCUP, will require the updates to be [re-published](https://patchmypc.com/when-and-how-to-republish-third-party-updates) from the Publisher. This is because additional content is required for the update.
* Patch My PC Publisher must be installed on the top-level SUP. This might be a CAS in the hierarchy. Access to the CAS might be limited in some environments to configure the Patch My PC Publisher. If customers are unable to access the CAS, consideration should be given to moving to the [in-console publishing method](https://learn.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates) instead.
* Customizations to updates and/or applications applied using the Patch My PC Publisher will result in small Patch My PC “helper” binaries being packaged inside the update CAB file and/or application source folder. These binaries are used to customize the installation of third-party updates and applications. Environments that utilize whitelisting technologies such as Microsoft AppLocker or Windows Defender Application Control might need to trust the Publisher of these helper binaries. Note: All Patch My PC helper binaries are signed by Patch My PC.
* Customers must choose whether they want to deploy meta-data only or full content when synchronizing updates using the Patch My PC Publisher. Unlike SCUP, there is not a threshold configuration that will automatically deploy content to WSUS for updates based on the number of devices that require the update in the customers environment.

### Migration - Video

In this 25 minute long video we take a look at what is involved moving from SCUP to Patch My PC Publisher and discuss some of the features of the Publisher too.

> **Note:** Some features mentioned in this video, such as creating Configuration Manager applications, require the [Enterprise Plus](https://patchmypc.com/request-quote#pricing-chart) license subscription.

https://www.youtube.com/watch?v=dXf1eICDkeU

### Migration Documentation

Detailed documentation for both SCUP and In-Console Publishing to Patch My PC Publisher migrations can be found at [Changes-for-Publishing-Updates-to-Configuration-Manager-Using-Third-Party-Catalogs.docx](https://patchmypc.com/app/uploads/2025/06/Changes-for-Publishing-Updates-to-Configuration-Manager-Using-Third-Party-Catalogs.docx)

### Licensing

Customers publishing updates using SCUP are typically subscribed to our **Basic** subscription. At the announcement of the deprecation of SCUP by Microsoft in January 2024, Patch My PC will no longer be renewing customers at this subscription level or offering it to new customers.

The Basic subscription, used with SCUP, had many limitations, not least limited to:-

* It does not provide automated publishing of third-party software updates or applications.
* SCUP’s configuration is saved, per-user, in %AppData%.
* It does not permit the creation of third party apps in Configuration Manager, only software updates.
* It does not permit the creation of third party apps or updates in Intune.
* Update customization is very limited using SCUP and customizations do not persist across new update version.
* SCUP does not support un-signed third-party binary installers.
* No new features. Customers will not be able to leverage any of the Patch My PC Right-Click customizations or new features such as Custom Apps.\
  [https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications)\
  [https://docs.patchmypc.com/patch-my-pc-cloud/custom-apps](https://docs.patchmypc.com/patch-my-pc-cloud/custom-apps)
* It is not possible to re-sign existing third-party updates in WSUS with SCUP.

\*\*Reportiong: A noteworthy mention\
\*\*The Enterprise Patch subscription also includes out [Patch Insights](https://patchmypc.com/advanced-insights/patch-insights) reporting solution for software updates in Configuration Manager. Patch Insights offers a high-level overview dashboard, updates report, update groups report, update deployments statistics, OS upgrade report, and verbose computer status report.&#x20;

https://youtu.be/nQH3CRhpTq8

\*\*Subscriptions\
\*\*The current subscriptions offered by Patch My PC are:-

* Basic (no longer offered)
* [Enterprise Patch](https://patchmypc.com/request-quote#feature-comparison)
* [Enterprise Plus](https://patchmypc.com/request-quote#feature-comparison)
* [Enterprise Premium](https://patchmypc.com/request-quote#feature-comparison)

The minimum subscription required when changing the publishing method highlighted in this document can be found in the table below:-

| **Existing Publishing Method** | **Desired Publishing Method**     | **Minimum Subscription Required** |
| ------------------------------ | --------------------------------- | --------------------------------- |
| SCUP                           | Patch My PC Publisher (preferred) | Enterprise Patch                  |
| In-Console                     | Patch My PC Publisher (preferred) | Enterprise Patch                  |
| SCUP                           | In-Console \*                     | Enterprise Patch                  |

\* Customers may choose to migrate from using SCUP to the [In-Console method in Configuration Manager](https://learn.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates) if publishing updates to a remote WSUS server is a hard requirement. This could be because access to the top-level SUP in the hierarchy is not accessible for the system admin responsible for patching. The in-console publishing method, while missing out on some of the great innovations that Publisher brings, is still a supported publishing method. In-Console publishing requires an Enterprise Patch subscription.

Customers using the Basic subscription should have already been contacted by Patch My PC. If you have not, please reach out to [sales@patchmypc.com](mailto:sales@patchmypc.com) to speak with an Account Executive to understand how the deprecation of SCUP affects your current license subscription.