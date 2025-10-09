---
title: "Microsoft Breaking Change to Teams / Power Automate Webhooks"
wpseo_title: Microsoft Breaking Change to 
wpseo_metadesc: "test" 
date: 2025-09-04
taxonomy:
    products:
        - 
    tech-stack:
        - 
    solution:
        - reporting-and-analytics
    post_tag:
        - alerts
    sub-solutions:
        - known-issues-and-considerations
        - best-practices
        - connectivity-and-proxy-issues

    
---

Microsoft has announced a breaking change to the way HTTP or Teams Webhook trigger flows work in Power Automate. This change may affect customers using Patch My PC alerts configured with a Teams webhook URL.

For reference, Microsoft’s announcement can be found here: [Changes to HTTP or Teams Webhook trigger flows](https://learn.microsoft.com/en-us/troubleshoot/power-platform/power-automate/flow-run-issues/triggers-troubleshoot?tabs=new-designer&utm_source=chatgpt.com#changes-to-http-or-teams-webhook-trigger-flows). Microsoft is changing how Teams and HTTP webhooks work in Power Automate:

- From **August 2025**, your flow’s webhook will automatically be given a **new URL**

- You’ll need to copy this new URL into Patch My PC (and anywhere else you use it)

- Make the update before **November 30, 2025**, or your alerts will stop working - before this date, the old webhook URL will continue to work

- The old deprecated webhook contains **login.azure.com** within the URL, for example:

> https://prod-30.centraluseuap.**logic.azure.com**:443/workflows/873e29ec0e1b42dab3dac8647e3f56f9/triggers/manual/paths/invoke?api-version=2016-06-01

This article will help you determine if you are affected, retrieve a new webhook URL if required, and provide guidance for updating the webhook within **Patch My PC Publisher (On-Premises)** or **Patch My PC Cloud**.

## Determine if you are affected

This section will help you understand whether any action needs to be taken if you're a Patch My PC Publisher customer, or Patch My PC customer, or both.

### Email from Microsoft

Microsoft has started proactively emailing users when one of their Power Automate flows is impacted. Below is an example of the email you might receive.

If you’ve received this message, it means you have a flow using an HTTP webhook with the old URL. However, this does not necessarily mean that the webhook is connected to Patch My PC.

Continue reading to learn how to check whether your Patch My PC alerts are affected and what steps to take.

![](/_images/Msft-flow-email.png)

### Patch My PC Publisher

To understand whether you need to replace any webhooks within the Patch My PC Publisher:

1. **Open the Publisher** and navigate to the **Alerts** tab

3. **Select each Teams webhook** (where _Message System_ reads _MSTeamsWorkflow_) and click **Edit**

5. **Observe the URL**; if the URL contains **logic.azure.com,** then you are affected and need to update it with the new webhook

![](/_images/Publisher-Flow-oldurl-1024x821.png)

### Patch My PC Cloud

To understand whether you need to replace any webhooks within Patch My PC Cloud:

1. **Sign in to Patch My PC Cloud**, and navigate to [Notifications](https://portal.patchmypc.com/settings/notifications)

3. Edit each notification ([read more](https://docs.patchmypc.com/patch-my-pc-cloud/cloud-administration/manage-cloud-notifications/modify-a-cloud-notification))

5. **For each Microsoft Teams webhook**, hover over the **Webhook URL** value

7. **Observe the URL** within the tooltip; if the URL contains **logic.azure.com**, then you are affected and need to update it with the new webhook

![](/_images/Cloud-Flow-oldurl2-1024x596.png)

## Retrieve the new webhook URL

Follow the below steps to identify the correct Flow and to retrieve the new webhook URL:

1. Sign in to [Power Automate](https://make.powerautomate.com/)

3. Locate the Flow used for Patch My PC

The default name for the flow will likely be "**Send webhook alerts to a channel**".

Any Flow, upon editing one, which previously used the old URL will have a warning banner along the top along with the option to **copy the new webhook / trigger URL**:

> "_Click here to copy the new trigger URL. The old trigger URL <trigger URL> will stop working on November 30, 2025. Your tools that use this flow WILL break unless you update them with the new URL._"
> 
> ![](/_images/Flow-warning-oldurl-1024x543.png)

If you have multiple Flows with the same name, you may be able to identify the correct Flow editing one and verifying the Team and Channel configuration:

![](/_images/Flow-identifychannel-1024x546.png)

If you have multiple Flows all sharing the same channel and perhaps used for different purposes, unless they're named appropriately, it may be hard to identify the Flow used for Patch My PC. In this circumstance, you may be better off generating a new webhook (new webhooks made after August 2025 will use the new URL).

For more details, see Microsoft's article: [Changes to HTTP or Teams Webhook trigger flows](https://learn.microsoft.com/en-us/troubleshoot/power-platform/power-automate/flow-run-issues/triggers-troubleshoot?tabs=new-designer&utm_source=chatgpt.com#changes-to-http-or-teams-webhook-trigger-flows)

## Update the webhook URL in Patch My PC

Once you have retrieved the new webhook URL, you simply paste it into the same area you triaged earlier in this article whether you are using Patch My PC Publisher and/or Patch My PC Cloud.

### Patch My PC Publisher

1. **Open the Publisher** and navigate to the **Alerts** tab

3. **Select the relevant Teams webhook** and click **Edit**

5. Paste in the new webhook URL, click **OK** and **Apply** to save your settings

7. Lastly, **select the Teams webhook again** and click **Test** and verify you received the test message in the Teams channel

### Patch My PC Cloud

1. **Sign in to Patch My PC Cloud**, and navigate to [Notifications](https://portal.patchmypc.com/settings/notifications)

3. **Modify the relevant notification** and **disconnect the old webhook**

5. Click **Add Webhook (+)**, choose **Microsoft Teams**, and paste in the new webhook URL ([read more](https://docs.patchmypc.com/patch-my-pc-cloud/cloud-administration/manage-cloud-notifications/create-a-microsoft-teams-webhook-notification-in-cloud))

7. Lastly, click the small envelope beside the webhook to test it and verify you received the test message in the Teams channel ([read more](https://docs.patchmypc.com/patch-my-pc-cloud/cloud-administration/manage-cloud-notifications/cloud-notifications-reference/test-a-microsoft-teams-webhook-notification-in-cloud))

## Next steps

We recommend reviewing any other services, besides Patch My PC, or automations that rely on Power Automate webhook URLs, as this change may affect them aswell.

If you encounter issues after updating, please reach out to [Patch My PC Support](https://patchmypc.com/technical-support/).
