---
title: "Configure Azure App Registration Permissions for Win32 Applications in Intune"
date: 2020-05-13
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
        - general-configuration-and-usage
        - security
        - automation
---

This article covers integrating the Patch My PC Publisher with your **Intune tenant**.  We will go over creating an **app registration** in your **Azure AD** environment and configuring the Graph API permissions required for the Publisher to automatically create, update and assign **Win32 applications** in your Intune tenant; as well as configuring the tenant authority, application ID and application secret within the Publisher.

## Step 1: Registering the Patch My PC Application in Azure AD

In order for our service to have permissions to your Intune tenant for application management, start by navigating to your environment's [Azure AD portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps), head to **App registrations,** and click **New registration** in the top left of the main pane.

![](/_images/azure-app-registration-12.png)

Give your app registration a relevant name such as "Patch My PC - Intune Connector".  Configure the account types base on your tenant requirements.  For the Redirect URI, leave it default unless you have specific requirements for configuring the Redirect URI.  Then click **Register**.

![](/_images/azure-app-registration-7.png)

## Step 2: Configure API Permissions for the New Application

After you register a new application, we will need to delegate certain permissions in order for the Patch My PC Publisher to create and update Win32 applications in your Intune tenant, as well as view Azure groups and create assignments for the applications automatically.

Once the new app is registered, navigate to the **API permissions** node in the left column of the newly created app's page. In the **API permissions** page, click the button to **Add a permission**, then in the right pane that appears, select the **Microsoft Graph** API.

![](/_images/azure-app-registration-11.png)

Then, you are prompted for what type of permissions your app requires, select **Application permissions**. In the **Select permissions** table view, search for "**DeviceManagement**" and under those permissions, enable the following:

- **DeviceManagementApps.ReadWrite.All**
    - View and create applications in Intune

- **DeviceManagementConfiguration.Read.All**
    - View properties and relationships of assignment filters

- **DeviceManagementManagedDevices.Read.All**
    - View device inventory for the auto-publish feature

- **DeviceManagementRBAC.Read.All**
    - View scopes to be assigned to applications

- **DeviceManagementServiceConfig.ReadWrite.All**
    - Update Enrollment Status Page configurations

![App Registration Permissions](/_images/IntuneAppRegPerms.png "App Registration Permissions")

Then, search for “GroupMember”, and under Group permissions, enable:

- **GroupMember.Read.All**
    - View Azure AD groups to enable automatic application deployment

![GroupMember.Read.All Permission Selected](/_images/GroupMemberReadAll.png "GroupMember.Read.All Permission Selected")

Click **Add permissions**.

To approve the new permissions, click **Grant admin consent for** . Choose **Yes** if you are prompted to consent for the required permissions.  You must be logged into an Azure AD account with permissions to perform this task.

The result is shown below.

![](/_images/azure-app-registration-14.png)

> **Note:** Granting admin consent may require one of the following roles: [Global Administrator](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#global-administrator) or [Privileged Role Administrator](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#privileged-role-administrator).

## Step 3: Getting the Client Secret and Application ID

Now, we must add a client secret, a string that our app will use to prove its identity when requesting a token.  Navigate to the **Certificates & secrets node** in the left column, and click the button to add a **New client secret**. Decide on a description and expiration date (in months) that best suits your organization's needs, then click **Add**.

![](/_images/azure-app-registration-10.png)

Copy the **Value** for the Client Secret you created. Save this value to a secure location, you will enter the value under **Application Secret** in the **Intune Options** of the Publisher.

![](/_images/azure-app-registration-6.png)

Then, navigate to the **Overview** node, and copy the **Application (client) ID**.  Save this value to a secure location along with your secret key value.

![the application client id in azure ad](/_images/application-client-id.png "the application client id in azure ad")

> You may receive an error similar to **'An error occurred while connecting to Intune: AADSTS7000215: Invalid client secret is provided.'** within the PatchMyPC.log file. If you receive this error please **repeat step 3 above** to create a new secret, or review your existing secret configuration within the Publisher to ensure you are using the correct value.

> In addition to the client secret, certificate-based authentication is also available in the Patch My PC Publisher. For more information, see the Microsoft documentation for more information: [Create a self-signed public certificate to authenticate your application](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-self-signed-certificate).

## Step 4: Configuring the Patch My PC Publisher to Connect to the Intune Tenant

If you do not know your Intune tenant domain, navigate to the [tenant status page](https://devicemanagement.microsoft.com/#blade/Microsoft_Intune_DeviceSettings/TenantAdminMenu/tenantStatus) in your Intune tenant, and look at the property for **Tenant name**.

![tenant status page in intune tenant](/_images/tenant-status.png "tenant status page in intune tenant")

Now, it is time to go to the **Patch My PC Publisher** and input the **Authority**, **Application ID**, and **Application Secret** into the **Intune Options** window of the Publisher.

![](/_images/azure-app-registration-16.png)

Replace with the **Tenant name** you found in the **tenant status page** of your Intune tenant.  Paste the **Application ID** and **Application Secret** that was saved from earlier.  Click **Test** to view the **Intune Connection Status** and validate that the **Publisher** can connect to your Intune tenant.  If the listed permissions all have a green checkmark under **Enabled**, you can now begin to publish applications to your Intune tenant.

![](/_images/azure-app-registration-15.png)

> In addition to the client secret, certificate-based authentication is also available in the Patch My PC Publisher. For more information, see the Microsoft documentation for more information: [Create a self-signed public certificate to authenticate your application](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-self-signed-certificate).
> 
> If you would like to use certificate-based authentication, choose the radio option **Certificate** and browse out to select the certificate. The certificate needs to be in the local machine’s Personal store.

> If the associated tenant is on GCC High, the changes below are required:
> 
> - Authority: [https://login.microsoftonline.us](https://login.microsoftonline.us/)
> 
> - Authentication URL: [https://graph.microsoft.us](https://graph.microsoft.us/)
> 
> - Graph Base URL: [https://graph.microsoft.us/beta](https://graph.microsoft.us/beta)