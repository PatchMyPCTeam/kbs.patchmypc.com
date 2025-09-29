---
title: "TeamViewer Hash Mismatch Workaround"
date: 2022-09-01
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

A short summary of the issue including keywords.

## Explanation of TeamViewer Hash Issue

In August 2022, TeamViewer began adding unique data to their EXE-based downloads. Due to this change, Patch My PC cannot publish updates to the TeamViewer EXE-based applications, as the unique data added to each download changes the hash of each download file. Patch My PC relies on the hash of the downloaded file to match what is found in Patch My PC's catalog.

This article will cover how to work around this issue by modifying a manually downloaded TeamViewer EXE to ensure that the hash of the downloaded file matches the one found in Patch My PC's catalog.

## Workaround for TeamViewer Hash Issue

1. Download the TeamViewer installation file to be published
    - The link to the download can be found in the Patch My PC Publisher by right-clicking on the appropriate Product and selecting "Show Package Info.."
        ![Screenshot of the Patch My PC Publisher with the option to show package info selected](/_images/RemoteDesktopManagerFree_t5uiehYV0L.png "Screenshot of the Patch My PC Publisher with the option to show package info selected")
        Patch My PC Publisher - Show Package Info

3. Move the downloaded file to the "Local Content Repository" path
    - If the "Local Content Repository" path is not defined, it may be set in the Patch My PC Publisher under the "Advanced" tab
        ![Screenshot of the Patch My PC Publisher with the Local Content folder indicated.](/_images/RemoteDesktopManagerFree_mDlEYM2DLo.png "Screenshot of the Patch My PC Publisher with the Local Content folder indicated.")
        Patch My PC Publisher - Local Content Folder

5. Download the script from here:

7. Run the downloaded script on the device where the Patch My PC Publisher is installed
    - The script will modify the TeamViewer files in the Local Content Repository in-place

9. In the Publisher, ensure that the checkbox to "Check the local content repository for content files before attempting to download content files from the internet." under the "Advanced" tab is enabled
    ![Screenshot of the Patch My PC Publisher on the Advanced Tab with the ](/_images/RemoteDesktopManagerFree_voRM5G9tAx.png "Screenshot of the Patch My PC Publisher on the Advanced Tab with the ")
    Patch My PC Publisher - Check for Local Content

11. On the "**Sync Schedule**" tab, click "**Run Publishing Service Sync**" to sync the modified installers
     ![Screenshot of the Patch My PC Publisher on the Sync Schedule Tab with Run Publishing Service Sync button identified](/_images/RemoteDesktopManagerFree_utyQpai8dZ.png "Screenshot of the Patch My PC Publisher on the Sync Schedule Tab with Run Publishing Service Sync button identified")
     Patch My PC Publisher - Run Publishing Service Sync

> **Note:** The script provided by Patch My PC is digitally signed, and the installer files modified by the script should still have valid signatures even after this process is complete.