---
title: "Integrating Patch My PC Products with GCC High Intune Tenants"
date: 2024-10-24
taxonomy:
    products:
        - 
    tech-stack:
        - intune
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - best-practices
        - migration
        - cloud-integration
---

At Patch My PC, we currently offer two solutions designed to simplify the process of publishing third-party software in Intune.

**1\. Patch My PC Publisher  
**An on-premises application that provides a user-friendly console to automate app publishing in Intune. It leverages an Entra ID App Registration for authentication within your environment.

**2\. Patch My PC Cloud  
**Our cloud-based SaaS solution offers similar functionality to the Publisher, simplifying third-party app publishing. While the Portal is still evolving toward feature parity with the on-premises Publisher, it offers seamless integration using an Enterprise Application (service principal) within Entra ID to securely publish apps.

## Choosing the Right Solution for GCC High Customers

For most customers, both solutions are available, with the SaaS solution often being preferred due to no infrastructure requirements. However, for **GCC High customers**, **Patch My PC Publisher** is currently the only compatible option. This is due to the specific restrictions and compliance requirements in GCC High environments, which are not yet supported by our SaaS solution.

## Why Patch My PC Cloud is Not Compatible with GCC High Environments

There are some key factors that prevent our SaaS solution from being used in GCC High environments:

#### 1\. Isolated API Endpoints

In GCC High environments, services like Entra ID and Microsoft Graph use isolated API endpoints unique to the environment. While commercial applications leverage common endpoints like _**microsoftonline.com**_ or _**graph.microsoft.com**_, GCC High requires endpoints specific to its domain, such as _**\*.microsoftonline.us**_. Curerntly we do not support these GCC High required endpoints.

#### 2\. Compliance Requirements

GCC High environments require stringent compliance measures, like **FedRAMP certification**. At present, our SaaS solution is not [FedRAMP](https://www.fedramp.gov/) certified, meaning it does meet the compliance standards for government organizations and contractors working with controlled unclassified information (CUI) and other sensitive data.

## Summary

For organizations operating in GCC High environments, **Patch My PC Publisher** remains the best and only option for publishing third-party apps to Intune. We are committed to enhancing our SaaS offering to eventually support government cloud environments. If you want to be notified when we do support this, please cast a vote on [this idea.](https://ideas.patchmypc.com/ideas/PATCHMYPC-I-4843)

In the meantime, our on-premises solution ensures you can still automate and streamline your third-party app publishing in a compliant manner.

If you'd like to explore our products further or see them in action, feel free to [book a free demo](#) with one of our engineers
