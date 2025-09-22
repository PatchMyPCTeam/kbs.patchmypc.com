# Custom Apps for MSPs using Patch My PC Publisher

In this article, we will learn how Managed Service Providers (MSPs) can create and manage custom apps and updates to be deployed using Patch My PC Publisher.

If you are an MSP using the Publisher and want to take advantage of the Custom Apps feature to add an application not currently supported in the[ Patch My PC Catalog](https://patchmypc.com/supported-products), you can onboard the Patch My PC Cloud portal to add in-house applications and publish them for each tenant configured in the Publisher.

### Onboarding to Patch My PC Cloud <a href="#h-onboarding-to-patch-my-pc-cloud" id="h-onboarding-to-patch-my-pc-cloud"></a>

As an MSP onboarding to Patch My PC Cloud, you’ll need to first create your company profile in the Patch My PC Cloud portal. Importantly, you will set the connection name in both the Cloud portal and the Publisher to **your** business name.

The customers that you manage in Patch My PC Publisher **do not** have their own Patch My PC Cloud portal instance or individual custom app catalogs. Both the Patch My PC Cloud portal and the Cloud connection in the Publisher will be associated to the MSP, not the customers you manage.

**IMPORTANT:** Please refer to [https://docs.patchmypc.com/patch-my-pc-cloud/onboarding-to-cloud](https://docs.patchmypc.com/patch-my-pc-cloud/onboarding-to-cloud) for up-to-date instructions on how to onboard to Patch My PC Cloud _**using the information below as a guide**_.

**Onboarding Information**\
When onboarding to Patch My PC Cloud, select **Create Company** and enter **your** business name in the **Company Name** field.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/msp_custom_apps_publisher_1.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://patchmypc.com/app/uploads/2025/04/msp_custom_apps_publisher_2.jpg" alt=""><figcaption></figcaption></figure>

Once onboarding to Patch My PC Cloud has been completed, you will need to create a connection to the Publisher.

### Create a connection between the Publisher and Patch My PC Cloud <a href="#h-create-a-connection-between-the-publisher-and-patch-my-pc-cloud" id="h-create-a-connection-between-the-publisher-and-patch-my-pc-cloud"></a>

Before we can create a custom app, we need to create a connection between the Publisher and Patch My PC Cloud.

**IMPORTANT:** Please refer to [https://docs.patchmypc.com/installation-guides/patch-my-pc-cloud/administration/manage-connections/add-a-connection](https://docs.patchmypc.com/installation-guides/patch-my-pc-cloud/administration/manage-connections/add-a-connection) for up-to-date instructions on how to create a connection between Patch My PC Cloud and the Publisher, _**using the information below as a guide**_.

**Publisher Connection Information**\
The **Connection Name** that you enter when configuring the **Cloud tab** in the Publisher should be your MSP business name.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/msp_custom_apps_publisher_3.jpg" alt=""><figcaption></figcaption></figure>

### Create a Custom App <a href="#h-create-a-custom-app" id="h-create-a-custom-app"></a>

Please refer to [https://docs.patchmypc.com/installation-guides/patch-my-pc-cloud/custom-apps/create-a-custom-app](https://docs.patchmypc.com/installation-guides/patch-my-pc-cloud/custom-apps/create-a-custom-app) for up-to-date instructions on how to create a Custom App.

### Managing Custom Apps across multiple Intune tenants <a href="#h-managing-custom-apps-across-multiple-intune-tenants" id="h-managing-custom-apps-across-multiple-intune-tenants"></a>

When a Custom App is created, it will appear in the Publisher across all Intune tabs as both an application and an update for all tenants. You can individually enable and customize each app or update uniquely for every tenant, using right-click options to apply specific configurations. Each tenant’s selection and customization within the Publisher remains unique, even though the app is available across all tenants configured in the Publisher.

<figure><img src="https://patchmypc.com/app/uploads/2025/04/msp_custom_apps_publisher_4.jpg" alt=""><figcaption></figcaption></figure>

> **Note:** Removing a Custom App from the Cloud Portal will also remove it across all tenant Intune tabs in the Publisher
