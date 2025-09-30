---
title: "How Patch My PC Handles Products with Multiple Versions"
date: 2022-09-02
taxonomy:
    products:
        - patch-my-pc-catalog
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - education
        - general-configuration-and-usage
        - best-practices
---

Some applications within the Patch My PC Catalog have multiple versions that are all supported at the same time. This article will explain how Patch My PC supports updates for these applications and general rules for working with these applications and updates.

## Topic 1: Determining Products with Multiple Supported Versions

Patch My PC supports a wide variety of applications and updates, some of the applications in the Patch My PC catalog support multiple major versions or tracks at the same time. Patch My PC attempts to support these multiple versions where possible.

To determine Patch My PC products with multiple supported versions, look for a version number specified at the end of the product name in the Patch My PC Publisher. If using SCUP or the ConfigMgr built-in 3rd party updates feature, the category usually will specify the version number as well.

For Patch My PC Updates (WSUS and Intune Updates) where the version is specified in the Publisher, the following applies:

- Patch My PC will not upgrade an older version of the software if it is older than the version specified in the Publisher

- Patch My PC will attempt to provide the latest available release of the version specified in the Publisher

> **Note:** To upgrade to a newer major version of a version locked app, deploy the ConfigMgr or Intune Application to the devices to be updated instead of the WSUS or Intune Update. Applications to not have version requirements for installs. Additionally the "Latest" designated update as described in this KB article can also be utilized to upgrade version locked applications (if available).

## Topic 2: List of Products Where Multiple Versions are Supported

\[table id=28 /\]

## Topic 3: "Latest" Version Designation for Patch My PC Products

Applications and Updates in Patch My PC with the word "Latest" in the product name will **always** install the latest version of the specified application on devices where **any** older version of the specified application is found. These Applications and Updates will also usually include a Pre-Script that will remove any older version of the software if found.

**Tips for "Latest" Version Designation Products:**

- Patch My PC Products with the "Latest" version designation will never be auto-enabled via the database scan features. They must be manually selected in order to be published.

- Target updates with the "Latest" designation to a specific group of devices where the major version upgrade is to be performed

- Filter updates with a title containing the word "Latest" from ADRs for Patch my PC Updates, this will ensure that "Latest" version designated products are not accidentally deployed to all systems and can prevent unwanted upgrades.

## Topic 4: List of Products with "Latest" Version Behavior Available

\[table id=26 /\]