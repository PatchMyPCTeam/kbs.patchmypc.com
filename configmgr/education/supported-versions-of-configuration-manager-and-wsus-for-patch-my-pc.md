# Supported Versions of Configuration Manager and WSUS for Patch My PC

```yaml
---

taxonomy:
    products:
        - 
    solutions:
        - 
    tech-stack:
        - configmgr
    post_tag:
        - 
    sub-solutions:
        - education
        - known-issues-and-considerations
        - best-practices
        
---
```

We’re often asked whether the [Patch My PC Software Update Catalog](https://patchmypc.com/application-patch-management) or [Publisher Software](https://patchmypc.com/publishing-service-setup-documentation) is supported on the latest versions of [**Configuration Manager**](https://docs.microsoft.com/en-us/mem/configmgr/core/servers/manage/current-branch-versions-supported) before customers perform site upgrades. In this article, we will review supported versions of **Configuration Manager** and **WSUS**.

We generally support the [**supported**](https://docs.microsoft.com/en-us/mem/configmgr/core/servers/manage/current-branch-versions-supported) versions of Configuration Manager current branch as documented by Microsoft.

> **Note:** The Publisher primarily interacts with the Configuration Manager **Console API’s**, therefore, it’s unlikely there would be any compatibility issues due to a Configuration Manager site upgrade even if there’s a Configuration Manager version that isn’t listed below as being tested and supported.

#### Versions of Configuration Manager Officially Tested by Patch My PC <a href="#versions-of-configuration-manager-officially-tested-by-patch-my-pc" id="versions-of-configuration-manager-officially-tested-by-patch-my-pc"></a>

The versions of **Configuration Manager** listed below have been tested by our team to ensure core function compatibility including:

| Feature Support               | CM 2309                                                                                                                         | CM 2403                                                                                                           | CM 2409                                                                                                           | CM 2503                                                                                                           |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Connecting to the SMSProvider | ![Feature Is Depreciated on this Subscription Level](/_images/Depreciated-Orange-Triangle.svg "Feature Is Depreciated on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |
| Creating applications         | ![Feature Is Depreciated on this Subscription Level](/_images/Depreciated-Orange-Triangle.svg "Feature Is Depreciated on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |
| Updating applications         | ![Feature Is Depreciated on this Subscription Level](/_images/Depreciated-Orange-Triangle.svg "Feature Is Depreciated on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |
| SUP Synchronization           | ![Feature Is Depreciated on this Subscription Level](/_images/Depreciated-Orange-Triangle.svg "Feature Is Depreciated on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |

![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") **= Feature is tested and supported**\
![Feature Is Depreciated on this Subscription Level](/_images/Depreciated-Orange-Triangle.svg "Feature Is Depreciated on this Subscription Level") **= Support has been deprecated**\
![Feature Not Supported on this Subscription Level](/_images/No-Red-X.svg "Feature Not Supported on this Subscription Level") **= Feature is not supported**

> **Upgrade Tip:** If the Patch My PC Publisher software is **installed on the site server**, we recommend stopping the **PatchMyPCService** before the site upgrade is started. This step will ensure the service doesn’t have any locks on console files during the console upgrade portion of the site upgrade on the site server.

#### Versions of WSUS Officially Tested by Patch My PC <a href="#versions-of-wsus-officially-tested-by-patch-my-pc" id="versions-of-wsus-officially-tested-by-patch-my-pc"></a>

The versions of **WSUS** listed below have been tested by our team to ensure core function compatibility including:

| Feature Support                      | Server 2008 R2 or Before                                                                                                        | Server 2012                                                                                                       | Server 2012 R2                                                                                                    | Server 2016                                                                                                       | Server 2019                                                                                                       | Server 2022                                                                                                       |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Publishing of updates                | ![Feature Is Depreciated on this Subscription Level](/_images/Depreciated-Orange-Triangle.svg "Feature Is Depreciated on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |
| Deletion of updates                  | ![Feature Is Depreciated on this Subscription Level](/_images/Depreciated-Orange-Triangle.svg "Feature Is Depreciated on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |
| Creation of WSUS Signing Certificate | ![Feature Is Depreciated on this Subscription Level](/_images/Depreciated-Orange-Triangle.svg "Feature Is Depreciated on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |
| Timestamping during publishing       | ![Feature Not Supported on this Subscription Level](/_images/No-Red-X.svg "Feature Not Supported on this Subscription Level")                     | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |
| Updating applications                | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level")               | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |
| SUP Synchronization                  | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level")               | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") | ![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") |

![Feature Is Supported on this Subscription Level](/_images/Yes-Green-Check.svg "Feature Is Supported on this Subscription Level") **= Feature is tested and supported**

![Feature Is Depreciated on this Subscription Level](/_images/Depreciated-Orange-Triangle.svg "Feature Is Depreciated on this Subscription Level") **= Support has been deprecated**

Newer versions of the publisher aren’t supported on deprecated versions of WSUS. You can download an [**older version of the publisher**](https://patchmypc.com/scupcatalog/downloads/publishingservice/PatchMyPC-Publishing-Service-Legacy.msi) that was tested for deprecated versions of WSUS.

![Feature Not Supported on this Subscription Level](/_images/No-Red-X.svg "Feature Not Supported on this Subscription Level") **= Feature is not supported**

#### Patch My PC Publisher System Requirements and Prerequisites <a href="#patch-my-pc-publisher-system-requirements-and-prerequisites" id="patch-my-pc-publisher-system-requirements-and-prerequisites"></a>

**When used for Microsoft Configuration Manager**

* Install on top-most WSUS/Software Update Point in the environment
* Configuration Manager Console Installed (For SUP Sync and ConfigMgr App features)
* Supported Operating Systems
  * Windows Server 2012, Windows Server 2016, Windows Server 2019, Windows Server 2022
  * Windows Server Update Services (WSUS) installed and configured
* Internet Connection
  * [List of Domains Used for Downloads](https://patchmypc.com/list-of-domains-used-for-downloads-in-patch-my-pc-update-catalog)
* Microsoft .NET Framework 4.6.2

**When used with Microsoft Intune Only**

* Supported Operating Systems
  * Windows Server 2012, Windows Server 2016, Windows Server 2019, Windows Server 2022
    * When using Windows Server, only the WSUS API component needs to be installed, not full WSUS. The WSUS API can be installed using PowerShell with the command: Install-WindowsFeature UpdateServices-API
  * Windows 10/11 x64
    * When using Windows 10, the [RSAT: Windows Server Updates Services](https://docs.microsoft.com/en-us/windows-server/remote/remote-server-administration-tools#BKMK_Thresh) needs to be installed.
* Internet Connection
  * [List of Domains Used for Downloads](https://patchmypc.com/list-of-domains-used-for-downloads-in-patch-my-pc-update-catalog)
* Microsoft .NET Framework 4.6.2