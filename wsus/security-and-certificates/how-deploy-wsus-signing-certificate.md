---
title: "How to Deploy the WSUS Signing Certificate for Third-Party Software Updates"
date: 2020-01-08
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

Deploying the WSUS Signing Certificate to devices is a requirement for devices to trust and install third-party software updates from standalone WSUS or a Configuration Manager environment.

If the certificate is not installed within the **Trusted Root** and **Trusted Publishers** certificate store, you will receive error code [0x800b0109](/third-party-updates-fail-to-install-with-error-0x800b0109-in-sccm) when attempting to install third-party software updates on devices.

Here's the **list of options** covered in the article for **deploying the WSUS Signing Certificate** to devices:

- [Use the Configuration Manager 1806+ Certificate Management Feature](#topic1)
    - [Validate Client Setting Priority is Correct](#topic1.1)

- [Use Group Policy to Deploy the WSUS Signing Certificate  
    ](#topic2)

- [Using a Configuration Manager Task Sequence During Imaging](#topic3)

- [Use a Configuration Manager Configuration Item](#topic4)

## Option 1: Use the Configuration Manager 1806+ Certificate Management Feature

If your Configuration Manager site and clients are running current branch 1806 or newer, the easiest option is to enable the new built-in option within the software update point for **Configuration Manager manages the updates**.

To enable this option, **perform the steps below:**

- In the Configuration Manager console, go to the **Administration** workspace. Expand **Site Configuration**, and select the **Sites** node.

- Select the top-level site in the hierarchy. In the ribbon, click **Configure Site Components**, and select **Software Update Point**.

- Switch to the **Third-Party Updates** tab. Select the option **Configuration Manager manages the certificate**.
    - ![](/_images/configuration-manager-automatically-manage-the-wsus-signing-certficaite.png)
        

- A new certificate of type **Third-party WSUS Signing** will be created in the **Certificates** node under the **Security** node in the **Administration** workspace.

After performing the steps above, **validate the certificate is created/imported:**

- Trigger a **software update point synchronization** by right-clicking the All Software Updates node, and clicking **Synchronize Software Updates.**
    - **
        ![](/_images/sync-sccm-software-update-point-for-declined-updates.png)
        **

- If you review the **wsyncmgr.log**, you should see some details about the certificate being imported and created.
    - ![](/_images/how-to-deploy-wsus-cert-1.png)
        

- If the certificate was configured successfully, if you re-open the **Third Party Updates tab**, you should see the **certificate detail**s.
    - ![](/_images/how-to-deploy-wsus-cert-2.png)
        

Lastly, you need to enable the client settings to **enable third party updates on clients:**

- In the Configuration Manager console, go to the **Administration** workspace and select the **Client Settings** node.

- Select the default client settings, an existing custom client setting, or create a new one.

- Select the **Software Updates** tab on the left-hand side. If you don't have this tab, make sure that the **Software Updates** box is enabled.

- Set the **Enable third-party software updates** to **Yes**.
    - ![](/_images/client-settings-enabled-third-party-software-updates-sccm.png)
        

- Validate the client settings is **deployed to all devices**

You can also review the **step-by-step video guide** for deploying the WSUS certificate **using Configuration Manager** below:

<iframe src="https://www.youtube.com/embed/V7PwHhBEcmY?start=408" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>

> If your software update point is **remote** from your **top-level site server** and the software update point is **not configured for SSL**, the option within Configuration Manager to **Automatically manage the WSUS signing certificate** will not work and you will receive an error in wsyncmgr.log **[Remote WSUS connection is not HTTPS. This prevents software update point from getting the signing certificate for third-party updates](/remote-wsus-connection-is-not-https-this-prevents-software-update-point-from-getting-the-signing-certificate-for-third-party-updates)**. For more details about this scenario, please see the following Microsoft Doc **[Additional requirements when the SUP is remote from the top-level site server](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates#additional-requirements-when-the-sup-is-remote-from-the-top-level-site-server)**.

## Option 1.1: Validate Client Setting Priority is Correct

If you enabled the client setting to **enable third party software updates**, performed a **machine policy sync** and **software update deployment evaluation cycle,** and the client still didn't install the WSUS signing certificate, please perform the following check.

View the **[result client settings](https://docs.microsoft.com/en-us/mem/configmgr/core/clients/deploy/configure-client-settings#view-client-settings)** on the specific device, not installing the certificate from the ConfigMgr console's **Devices** node.

![Resultant Client Settings SCCM Third Party Updates](/_images/Resultant-Client-Settings-SCCM-Third-Party-Updates.png "Resultant Client Settings SCCM Third Party Updates")

If the **result client settings** shows the Enable third party software updates as **No**, that means there another client setting taking priority of the policy you deployed to set this value to Yes.

![Enable Third Party Updates is Yes](/_images/Enable-Third-Party-Updates-is-Yes.png "Enable Third Party Updates is Yes")

You can view all the client settings deployed to a specific device in the **Client Settings** tab.

![Client Setting Deployed to Device SCCM tab](/_images/Client-Setting-Deployed-to-Device-SCCM-tab.png "Client Setting Deployed to Device SCCM tab")

When troubleshooting client certificate installation issues, in addition to the client settings priority, you need to ensure the **client version** is also **ConfigMgr 1806** or **newer**. The client version for build 1806 is **5.00.8692.1003**

> **Important**: By default, the **Enable third party software updates** setting is configured to **No** for any existing policies. This often causes machines where the policy is deployed to be overwritten by other client settings deployed to the same device. When you have multiple custom client settings, they are applied according to their order number. If there are any conflicts, the setting that has the **lowest order number overrides the other settings**. To change the order number, on the **Home** tab, in the **Client Settings** group, choose **Move Item Up** or **Move Item Down**. ([Microsoft Docs](https://docs.microsoft.com/en-us/mem/configmgr/core/clients/deploy/configure-client-settings#view-client-settings))

## Option 2: Use Group Policy to Deploy the WSUS Signing Certificate

You can also use **group policy to deploy the WSUS Signing Certificate** to devices within your environment. This option is helpful if you can't manage the certificate **[using the Configuration Manager built-in option](#topic1.1)**.

Please see the **PDF guide below for a step-by-step** guide for how to deploy the WSUS signing certificate using group policy to devices.

**[Deploying the WSUS Signing Certificate Using GPO (PDF Guide)](/scupcatalog/documentation/CertificateAndGPODeploymentGuide.pdf)**

You can also review the **step-by-step video guide** for deploying the WSUS signing certificate **using GPO** below:

<iframe src="https://www.youtube.com/embed/V7PwHhBEcmY?start=407" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen" data-cookieconsent="ignore"></iframe>

## Option 3: Using a Configuration Manager Task Sequence During Imaging

Another option is to deploy the certificate within a Configuration Manager **task sequence step** or a **package deployment** that uses **certutil.exe** to import the WSUS Signing Certificate to the **Trusted Root** and **Trusted Publishers** certificate store.

For instructions using this method, please review the following **[knowledge base article](/applications-fail-to-install-during-osd-in-sccm-with-error-authorizationmanager-check-failed-0x87d00327#resolution)**.

## Option 4: Use a Configuration Manager Configuration Item

Another option is to deploy the certificate within a Configuration Manager using a **configuration item.**

For instructions using this method, please review the following **[knowledge base article](/wsus-signing-certificate-deployment-ci)**.