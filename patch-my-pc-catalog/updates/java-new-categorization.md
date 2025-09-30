---
title: "New Categorization Methodology for Patch My PC Java Releases"
date: 2021-07-10
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
        - updates
        - application-and-update-publishing
        - best-practices
---

In July 2021, Patch My PC added a new flavor of OpenJDK – Zulu JDK. With this new addition we are trying a new methodology for the JRE/JDK releases. If this new methodology proves to be successful, Patch My PC plans to utilize the same methodology for all other available versions of Java in the catalog. This post will explain the changes we plan to make to the Java installers and why.

Currently (for Oracle Java), we create a new product in the catalog for every major Java release (11, 14, 15, etc.), and _only_ allow our updates to update existing versions of java that are of the same major release. We believe this is counter to the intentions of these latest releases of Java and are proposing the following (using Zulu OpenJDK as our testbed).

In the catalog, we now have the following Products:

- **Zulu JDK Latest**
    - If this product is selected and deployed, the latest Zulu JDK version available will be deployed, this product will upgrade **_all_** versions of Zulu OpenJDK on applicable devices to the latest available.

- **Zulu JDK 8**
    - This product if selected and deployed will only upgrade existing Zulu JDK 8 installations to the latest version of Zulu JDK 8

- **Zulu JDK 11**
    - This product if selected and deployed will only upgrade existing Zulu JDK 11 installations to the latest version of Zulu JDK 11

Switching to this method will allow our customers to choose if they want to patch OpenJDK to the latest available or stay on a supported LTS release. As new LTS releases come out, Patch My PC will add them as new products and provide update definitions until that LTS release hits end-of-life.

One major drawback to this new method is that deployments for this new methodology is that if both "Zulu JDK Latest" **and** any of the Zulu JDK LTS releases are selected as a product in Patch My PC, there is the chance of 2 updates being applicable on a single device (say if Zulu JDK Latest and Zulu JDK 8 are both selected and deployed to a device with an old version of Zulu JDK 8, that device will get both updates as applicable and the version that the device will end up with is Zulu JDK Latest).

The benefits of this new method are:

- Customers will not need to update their product selections as often

- Customers will be able to update to the latest version of Java without an application deployment (Zulu JDK Latest will update any version to the latest, so app deployments to update between major versions are unnecessary)

- Patch My PC will not need to create a new product for every new release of Java every 6 months (Adding a new product node to the catalog is more time consuming than adding a new update for the same product)

- Customers will have a clear choice between staying on LTS versions or following the latest Java release.

Following a successful test period of Zulu JDK using this methodology, Patch My PC plans to use this same methodology on the next major Java update (planned for September 2021), for all supported Java products in our catalog. If you have any comments on this change, please leave a comment on this idea on the Patch My PC ideas portal: [https://ideas.patchmypc.com/ideas/PATCHMYPC-I-1497](https://ideas.patchmypc.com/ideas/PATCHMYPC-I-1497)
