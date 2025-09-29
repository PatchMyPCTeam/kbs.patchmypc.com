---
title: "Introducing Proactive Customer Care"
date: 2020-09-12
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

Today, we're excited to announce our new **proactive customer care program**. Below we will cover what this actually means and why we think this program will benefit our customers.

## What is Proactive Customer Care?

Our proactive customer care program is a way to help ensure our **[Publishing software](/docs)** is running smoothly and successfully publishing [third-party updates](/application-patch-management) and [applications](/automating-application-packaging-in-microsoft-sccm) to Windows Server Updates Services (WSUS), Configuration Manager, or **[Microsoft Intune](/third-party-patch-management-for-microsoft-intune-now-available)**.

If we receive **[insights](/telemetry-data-collected-when-using-the-publisher#topic9)** that key operations failed, such as publishing a third-party update to WSUS, we want to notify you and ensure things are running smoothly again as soon as possible.

## Why We Think It's Important

From an engineering perspective, our biggest goal is for you to set our software up once, and then **it just works**. Generally, this works great since everything is fully automated after setup, and day-to-day work is performed using your **existing workflows** in the Configuration Manager console or **[Intune portal](https://devicemanagement.microsoft.com/#home)**.

If you enabled the [SMTP email feature](/troubleshooting-smtp-email-sending) or [Microsoft Teams notifications](https://www.prajwaldesai.com/integrate-patchmypc-with-teams-for-update-notifications/) in our Publisher, as shown below, we would already notify you about any **health issues**.

Here's an example **email alert** you would receive if your **[WSUSContent is misconfigured](/an-error-occurred-while-publishing-an-update-to-wsus-createdirectory-failed)**.

![email report of update that failed to publish](/_images/email-report-error-e1599795734300.png "email report of update that failed to publish")

Our goal with proactive customer care is to send alerts containing key information about **errors in your environment,** even if you **didn't enable emails using SMTP settings** directly in our software.

We don't want customers to realize there are issues when they notice a new **[0-day vulnerability](https://us.norton.com/internetsecurity-emerging-threats-how-do-zero-day-vulnerabilities-work-30sectech.html)** isn't showing up in their environment.

## Proactive Alerts for Optimizing Performance

Starting in July 2021, we will also send alerts for settings that could allow you to optimize various configurations to avoid future problems.

For example, in Configuration Manager 1806, there are new settings for [software update maintenance](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/software-updates-maintenance) that aren't enabled by default.

![WSUS Maintenance Turned Off in SCCM](/_images/WSUS-Maintenance-Turned-Off-in-SCCM.png "WSUS Maintenance Turned Off in SCCM")

If these settings aren't enabled, you could receive a proactive email to let you know this could help improve performance and reliability.

## How Does Proactive Customer Care Work?

By partnering with our customer base of over 2,000, we have seen **trends** and **recurring issues** that lead to a list of **[knowledge base articles](/category/knowledge-base)** to solve known issues quickly. Last month, we included a new **[telemetry option](/telemetry-data-collected-when-using-the-publisher#topic9)** to let us know if you receive any error messages during important publishing operations for updates and applications.

If we detect a **[known issue](/category/knowledge-base)** through **[error insights](/telemetry-data-collected-when-using-the-publisher#topic9)**, we will send you a direct email containing the information about how to resolve the issue. Here's an example of what a proactive email would look like:

![Proactive Customer Care Email Example](/_images/Proactive-Customer-Care-Email-Example.png "Proactive Customer Care Email Example")