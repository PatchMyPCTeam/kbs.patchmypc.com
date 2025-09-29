---
title: "Error Code 193 When Adding Patch My PC Catalog to SCCM?"
date: 2018-09-12
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

In this post, we will cover an issue you might encounter when trying to subscribe to our third-party software update catalog in Microsoft System Center Configuration Manager.

![](/_images/third-party-catalog-patchmypc-in-sccm-subscribe.png)

When attempting to subscribe to the custom catalog added for your subscription to Patch My PC, configuration manager will show the following error message "Unable to create the subscription. The console failed to download the catalog from (URL) because of **error code 193**."

![](/_images/third-party-catalog-patchmypc-in-sccm-error-193.png)

## Why does this happen?

This will happens when you are only subscribed to the [Basic subscription](/frequently-asked-questions#subscription-comparisons) or your subscription has **expired**.

This basic subscription is only compatible with [Microsoft System Center Updates Publisher (SCUP)](/scup-setup-documentation) and not the third-party software update catalogs [feature in SCCM 1806 and newer](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#add-a-custom-catalog).

## Why does the Catalog Cost More for SCCM?

One of our primary goals is to ensure we provide an affordable solution for third-party patch management in SCCM, so it's important to address why the [Enterprise Subscription](/frequently-asked-questions#subscription-comparisons) is required to support SCCM **in-console publishing**.

The reason is related to a new catalog format required for SCCM to support importing and publishing the catalog. The new catalog format is more complex form an engineering perspective. This new format causes overhead for every catalog release.

#### **Additional Certificate Requirements (v2 Catalog)**

If we take a look into the new catalog format, you will notice there is a new folder called "ContentCertificates"

![](/_images/third-party-catalog-patchmypc-in-sccm-contentcertificates.png)

Whenever we release a new catalog update, we need to ensure we include every certificate used to sign all update binary files within our [supported products](/supported-products) for our catalog. The added content certificates allow SCCM to have a layer of trust when publishing all third-party update content. There are currently around 119 certificates we need to manage and maintain when new certificates are used to code-sign update files.

![](/_images/third-party-catalog-patchmypc-in-sccm-certificates.png)

#### **Unique Catalog for Every Supported Product**

Since SCCM doesn't support any filtering when publishing updates from a catalog, we wanted to ensure we came up with a solution that allows our customers to publish specific products while using our catalog with SCCM. We came up with an optional parameter you can use for only [importing specific products into SCCM](/selectively-choose-products-to-publish-patch-my-pc-update-catalog).

The solution for importing the catalog by product requires us to manage a separate catalog for each product on our backend. Whenever new products are updated, we need to maintain the main catalog in addition to a catalog for each product and the content certificates required for each product catalog. We currently support over 150 unique products.

#### **v3 Catalog Formating**

Microsoft released a [v3 catalog format](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#new-subscription-to-a-third-party-v3-catalog) that includes filtering options and options for staging content. This new format requires significant work on our end to maintain.

## Wrapping Up?

Hopefully, this has been helpful, and you understand why we decided to require the "**Catalog + Automation**" subscription to use our catalog with Microsoft SCCM. The "**Catalog + Automation**" subscription will also allow you to alternatively use our [publishing service](/introducing-automated-third-party-patch-management-for-microsoft-sccm) that allows more customization such as killing application processes, skipping the update if the application is running, adding custom [pre/post scripts](https://www.youtube.com/watch?v=3vNpV13PxvQ), and we are working on more awesome features!