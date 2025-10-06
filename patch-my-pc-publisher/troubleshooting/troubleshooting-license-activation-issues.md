---
title: Troubleshooting License Activation Issues (Invalid License ID.)
date: 2019-11-01T00:00:00.000Z
taxonomy:
  products:
    - patch-my-pc-publisher
  tech-stack:
    - configmgr
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - troubleshooting
    - licensing
    - security
---

# Troubleshooting License Activation Issues

This article details common reasons why the **license validation may fail** within our [Publisher](../../docs/). If you're facing a license activation error similar to below, this will be the guide for you!

![](/_images/license-validation-1-2.png)

Almost all license validation **issues are related to proxy and web filters**.

### Step 1: Validate You are Using the Correct License key

The first step is to validate you are using the **correct license key.** When you receive your [full-trial](https://patchmypc.com/free-trial) or customer license email, it will contain your 20 character license key. Here's an example of a license key email.&#x20;

![](/_images/license-validation-2.png)

Enter the license key you received in the **License Information** section of the **General** tab and select **Validate**. &#x20;

![](/_images/license-validation-4.png)

If the license validation is successful, it should look like the image below.

![](/_images/license-validation-6.png)

### Step 2: Are You Using A Proxy?

If a proxy is required for internet access within your environment, you will need to configure it from the **Advanced tab's** and apply the new settings.

![](/_images/license-validation-7.png)

If a proxy is configured, **restart the Publisher** for the changes to take effect.

You will also need to confirm if **proxy authentication** is required. If so, the **Use Authentication** must be checked and a login configured.

### Step 3: Web Filters and Firewalls

The most common issue with license activation is web filters such as **firewalls**, **DNS filtering**, or other **security appliances** blocking the web request. For the license to be validated, please ensure the following traffic is allowed through any web filters, and review any blocked request to the domains listed below.

\[table id=29 /]

> **Note:** Depending on firewall restrictions, you may need to **whitelist other domains** for third-party content downloads for vendors in our catalog. For a full list of domains, please see [**List of Domains for Whitelisting when Using Patch My PC’s Catalog**](../../list-of-domains-used-for-downloads-in-patch-my-pc-update-catalog/)

You can download the Microsoft tool [**PortQryUI**](https://www.microsoft.com/en-us/download/details.aspx?id=24009) to perform a test to verify access to the domains listed above. Once downloaded, run portqueryui.exe. In the screenshot below we entered **patchmypc.com** for the domain, and **443** for the port over **TCP**. The result should show listening if there are no firewalls blocking traffic.

![portqueryui verify port 443 to patchmypc domain](/_images/portqueryui-verify-port-443-to-patchmypc-domain.png "portqueryui verify port 443 to patchmypc domain")

### Step 4: Verify the patchmypc.com SSL Certificate is Trusted

If trusted root certificate updates are disabled via windows update, the SSL certificate used for patchmypc.com may not be trusted.

On the server running the publishing service, open up internet explorer and navigate to [https://patchmypc.com/](https://patchmypc.com/). In the address bar, check the certificate lock to see if it's trusted.

Click the **lock icon** then click **View certificate**

![SSL View Certificate IE](/_images/SSL-View-Certificate-IE.png "SSL View Certificate IE")

Click the Certificate Path and validate the certificate for patchmypc.com is trusted.

![SSL View Certification Path](/_images/SSL-View-Certification-Path.png "SSL View Certification Path")

If the certificate is not trusted, [import the root certificates for DigiCert](https://www.digicert.com/digicert-root-certificates.htm) so the certificate is trusted.