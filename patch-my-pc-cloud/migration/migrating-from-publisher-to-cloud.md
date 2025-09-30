---
title: "How to migrate from the Publisher to Cloud - Patch My PC"
date: 2024-11-29
taxonomy:
    products:
        - patch-my-pc-cloud
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - migration
        - best-practices
        - updates
---

If you're a Patch My PC customer currently using the On-Prem Patch My PC Publisher Console to integrate and automate the publishing of Win32 apps to Intune, but want to transition to our new SaaS Cloud Portal, this guide is here to help.

## Important Notes Before You Begin

**No automated migration**:  
Currently, there is no automated migration process available to transition from the On-Prem Publisher to the Cloud Portal.

We will build one once we achieve Full-Feature parity. If you are interested in this option, please upvote [this idea](https://ideas.patchmypc.com/ideas/PATCHMYPC-I-4662) on our UserVoice and we'll notify you when it's available.

**Feature Parity in Progress**:  
While we're working to achieve full feature parity between the On-Prem Publisher and the Cloud Portal, there may be differences in functionality. You can track our progress on [this page](https://docs.patchmypc.com/installation-guides/patch-my-pc-cloud/intune-apps/feature-comparison-with-publisher).

**Manual App migration**:  
You will need to manually migrate your apps and updates to the Cloud Portal. Please follow the steps outlined below to ensure a smooth transition.

## Steps to Migrate Your Apps and Updates to the Cloud Portal

**1\. Review Cloud Portal Documentation  
**Begin by familiarizing yourself with the [documentation for our Cloud Portal](https://docs.patchmypc.com/installation-guides/patch-my-pc-cloud). Understanding its capabilities and workflows will make the migration process easier.

**2\. Sign Up on portal.patchmypc.com  
**Please skip this step if you have already signed up on our Cloud portal.

Follow [this guide](https://docs.patchmypc.com/installation-guides/patch-my-pc-cloud/onboard-to-patch-my-pc-cloud) to sign up to our Cloud portal.  
Once you've signed up, follow [this guide](https://docs.patchmypc.com/installation-guides/patch-my-pc-cloud/intune-apps/onboard-to-intune-apps) to integrate our Cloud portal with your Intune tenant.

**3\. Take Note of Right-Click Customizations**  
Identify and document the customizations applied to apps published through the On-Prem Publisher [right-click options](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications).

> **Note:** Ensure that the customization options you use are available in the Cloud Portal before continuing.

**4\. Uncheck the Software in the On-Prem Publisher Console**  
In the On-Prem Publisher Console, uncheck the software you wish to migrate.

**5\. Delete the Win32 App in Intune**  
Remove the Win32 app that was previously published by the On-Prem Publisher from Intune.

**6\. Reconfigure and Deploy from the Cloud Portal**  
Using the Cloud Portal, reconfigure the app as a new deployment (app, update, or both) and deploy it to your environment.

## FAQ

Can I Migrate Only Some Apps While Testing the Process?

Absolutely! We encourage you to start small.

You can migrate a few apps initially to familiarize yourself with the Cloud Portal. While you’re testing, the On-Prem Publisher will continue to publish new versions of any apps you leave selected, while the Cloud Portal will manage only the ones you’ve migrated.
