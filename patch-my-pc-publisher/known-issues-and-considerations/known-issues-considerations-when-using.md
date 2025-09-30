---
title: "Known Issues and Considerations when Using Patch My PC"
date: 2020-05-09
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
        - known-issues-and-considerations
        - troubleshooting
        - best-practices
---

**The article below shows known issues and considerations** to make when deploying specific products using Patch My PC.

Patch My PC will periodically reevaluate product behavior and adjust these lists as necessary. Customers encountering **unexpected product behavior** should **[open a support request](https://patchmypc.com/technical-support)** so that Patch  
My PC can evaluate any possible issues.

## Products with Unique Install or Uninstall Behavior

The following table contains a list of products that have unique installation or uninstallation behaviors.

\[table id=21 /\]

## Products that Require Applications be Closed Before Installation

The following products will **fail to update if the process is open** while the update installation is attempted. You can control how to close or notify the user using our feature to **[manage conflicting processes when updating third-party applications](https://patchmypc.com/manage-conflicting-processes-when-updating-third-party-applications)**.  
**Note: These products will automatically have the Manage Conflicting Processes feature enabled and set to "Skip" by default when enabled in the Publisher.**

\[table id=22 /\]

## Products with Known Dependency Requirements

The following products will **fail to update or install if the listed dependency is not installed before** installation is attempted. Patch My PC does not natively handle dependencies, however dependencies manually created in Intune or ConfigMgr can be carried over when Patch My PC updates applications.

\[table id=36 /\]

## Products that Require a Manual Download

The following products require manual download because their installer binaries are behind a paywall. For more details about how to use the local content repository, please see our guide **[local content repository for licensed applications that require manual download](https://patchmypc.com/local-content-repository-for-licensed-applications-that-require-manual-download).**

\[table id=24 /\]

## List of Known Issues

The table below lists any known issues we are tracking.

\[table id=53 /\]
