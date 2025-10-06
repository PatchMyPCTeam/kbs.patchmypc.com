---
title: "Patch My PC Publisher Configuration Overlap"
date: 2025-10-06
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - configmgr
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - troubleshooting
---

# Patch My PC Publisher Configuration Overlap

This article is designed to help customers ensure their Patch My PC Publisher configuration is accurate and aligned with its intended purpose.

In certain versions of the Publisher, using a new filter selection function and then switching between tabs (for example, Intune Apps and Intune Updates) could cause some settings to carry over to another tab unintentionally.

In this scenario, product selections, right-click options, or assignments may have been duplicated across tabs without the user intending it.

**Topics** covered in this article:

* [Understanding the Issue](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#understandingtheissue)
  * [Issue](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#issue)
  * [Impact By Platform](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#impactbyplatform)
  * [Impacted Versions](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#topic1)
  * [Fixed Version](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#topic2)
  * [Am I Impacted?](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#amiimpacted)
* [Restoring Configurations](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#restoringconfigurations)
  * [Option 1 – Restore Configuration from Backup](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#restorefrombackup)
  * [Option 2 – Manually Restore Configurations](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#manualrestore)
* [Contacting Support](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#contactsupport)

### Understanding the Issue <a href="#h-understanding-the-issue" id="h-understanding-the-issue"></a>

#### Issue <a href="#h-issue" id="h-issue"></a>

In [Publisher version 2.1.37.0](https://docs.patchmypc.com/publisher/publisher-release-notes#id-2.1.37.0-2025-08-28), released 28th August 2025, we introduced a new filter button in the Publisher UI. This feature allows you to reduce the size of the product tree by displaying only the products that are currently selected for publishing.

* **Before**\
  The product tree showed all available vendors and products, which could be lengthy and harder to navigate.
* **After**\
  When the filter button is enabled, the tree is condensed to show only the products you’ve selected for publishing, making it easier to review and manage your configuration.

You can toggle the filter on or off at any time by using the filter button in the toolbar.

![](/_images/image-13.png)

The configuration overlap could occur in the following sequence when using the filter option in affected versions of Publisher:

1. Apply a filter on **Tab 1** (e.g. _Intune Apps_).
2. Apply a filter on **Tab 2** (e.g. _Intune Updates_).
3. Make a change on **Tab 2**.
4. Click **Apply** to save changes.
5. Return to **Tab 1**.
6. Remove the filter (unfilter).
7. Click **Apply** again.

In this scenario, settings from Tab 1 may have carried over into Tab 2.

#### Impact by Platform <a href="#h-impact-by-platform" id="h-impact-by-platform"></a>

When the filter scenario described above occurred, right-click options, customizations, and product selections could carry from one tab to another. The way this manifests varies depending on the platform:

**ConfigMgr**

* If product selections carried from the Updates tab into the ConfigMgr Apps tab, applications could have been created in ConfigMgr without the customer intending to select them.
* These applications are not deployed automatically, so while they may exist in ConfigMgr, they will only impact devices if the customer later chooses to deploy them.
* Impact level: Low

**WSUS**

* If selections moved from the ConfigMgr Apps tab into the Updates tab, additional updates could have been marked for publishing to WSUS.
* If an ADR rule later evaluated those updates after a Software Update Point (SUP) sync, they may have been deployed unintentionally.
* In this case, devices may have received updates the customer did not explicitly plan to publish, though typically this will align with standard update workflows.
* Impact level: Low

**Intune**

* If selections moved between the Intune Updates and Intune Apps tabs, product selections, options, and assignments could all carry across.
* Since Intune Updates are often deployed as Required to device or user groups, those same assignments may also have been configured on the Intune Apps tab.
* This could result in some applications being deployed as Required unintentionally.
* Impact level: High

#### Impacted Versions <a href="#h-impacted-versions" id="h-impacted-versions"></a>

This issue may occur in the following versions of the Patch My PC Publisher when the filter option is used, and the user navigates to another tab and then saves the configuration.

* [2.1.37.0](https://docs.patchmypc.com/publisher/publisher-release-notes#id-2.1.37.0-2025-08-28) – Released 28th August 2025
* [2.1.41.0](https://docs.patchmypc.com/publisher/publisher-release-notes#id-2.1.41.0-2025-09-04) – Released 4th Septermber 2025
* [2.1.43.0](https://docs.patchmypc.com/publisher/publisher-release-notes#id-2.1.43.0-2025-09-11) – Released 11th September 2025
* [2.1.46.0](https://docs.patchmypc.com/publisher/publisher-release-notes#id-2.1.46.0-2025-09-18) – Released 18th September 2025

#### Fixed Version <a href="#h-fixed-version" id="h-fixed-version"></a>

The issue was identified and resolved in Publisher version 2.1.50.0. Any version higher than 2.1.50.0 includes the fix.

* [2.1.50.0](https://docs.patchmypc.com/publisher/publisher-release-notes#id-2.1.50.0-2025-09-25) – Released 25th September 2025

**Important:** If the **filter option** was used in an earlier affected version, configuration overlap may still be present in your environment. Please review the [**Am I Impacted?**](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#amiimpacted) section below to confirm whether your configuration requires attention

#### Am I Impacted <a href="#h-am-i-impacted" id="h-am-i-impacted"></a>

You can determine if you have been impacted in two ways:

1. [Run the Provided Support Script (recommended)](https://patchmypc.com/kb/publisher-configuration-overlap/?preview_id=162295\&preview_nonce=a653a4e338\&preview=true#runtheprovidedsupportscript)
2. Perform a visual check in Publisher

**Run the Provided Support Script**

1. Download the following script \<url for Check-PublisherConfigOverlap.ps1>.
2. Open PowerShell as Administrator on the server where Patch My PC Publisher is installed.
3. Change directory to the folder where the script was downloaded, for example:\
   `cd "C:\Users\\Downloads"`
4. Allow the script to run in the current session, execute:\
   `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`
5. Run the script:\
   `.\Check-PublisherConfigOverlap.ps1`

**Perform a Visual Check in Publisher**

If you prefer a quick confirmation in the UI. For customers publishing apps and updates to Intune:

1. **Tabs**\
   Open the Intune Apps and Intune Updates tabs and look for the same products appearing in both where you wouldn’t expect them.
2. **Right-click options & customizations**\
   Check if identical options (e.g custom pre/post scripts) show up on both tabs unintentionally.
3. **Assignments**\
   Verify that Required/Available assignments applied on one tab (especially at the _All Products_ level) do not also appear on the other tab.

For customers publishing apps and updates to ConfigMgr and WSUS:

1. **Tabs**\
   Open the ConfigMgr Apps and Updates tabs and look for the same products appearing in both where you wouldn’t expect them.
2. **Right-click options & customizations**\
   Check if identical options (e.g custom pre/post scripts) show up on both tabs unintentionally.

**Cross-Platform Review**

In addition to reviewing the Publisher UI, we recommend checking directly in the **Intune admin center** and the **ConfigMgr console** to confirm whether any applications or updates were published unexpectedly.

### Restoring Configurations <a href="#h-restoring-configurations" id="h-restoring-configurations"></a>

#### Option 1 – Restore Configuration from Backup <a href="#h-option-1-restore-configuration-from-backup" id="h-option-1-restore-configuration-from-backup"></a>

#### Option 2 – Manually Restore Configurations <a href="#h-option-2-manually-restore-configurations" id="h-option-2-manually-restore-configurations"></a>
