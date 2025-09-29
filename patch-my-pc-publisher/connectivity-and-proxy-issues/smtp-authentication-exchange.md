---
title: "Patch My PC SMTP Authentication for Exchange Online"
date: 2024-07-23
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

## Purpose of this article

Patch My PC’s Publisher, at this time, cannot support Modern authentication and relies online SMTP authentication for sending its email reports.

In this article we will look at how to reenable SMTP authentication for 1 specific mailbox, which will allow Patch My PC’s Publisher to authenticate and send emails through your Exchange Online environment.

## History

it is generally a bad practice to have an open/anonymous/unauthenticated relay for messaging on the internet, as a messaging server that is either intentionally or unintentionally configured in such a way to allow messages from any source to be routed through the relay completely transparently. This type of configuration is very sought after and can be used maliciously if discovered.

Exchange Online does not allow this type of relay by default, and configuring one is somewhat obfuscated. We will not discuss how to configure that here today.

Where such a relay is not configured, systems must authenticate against a standard relay to be permitted to send emails, which is the preferred approach. However, Exchange Online, by default (as of a recent change – 2023), does not allow basic authentication or the use of SMTP authentication. The default now is Modern authentication (Oauth 2.0 token-based authenticate), which uses tokens and provide enhanced security features such as multi-factor authentication (MFA). Disabling basic authentication helps protect against attacks like credential stuffing, brute force, and phishing, which helps improve the overall security of Exchange Online environments.

That said, this type of relay is still a common requirement for many organizations with internal web, app, or database servers, monitoring applications, or MFPs that can generate emails but cannot always send them without using such a relay. Additionally, many of these applications, servers, and MFPs cannot perform the actions required to use Modern authentication, especially if something like MFA is configured.

Lastly, in newer tenants, all of this is topped off by Authenticated SMTP being blocked, even if you turn it on for 1 mailbox, if Security Defaults are enabled in your Entra ID Tenant.

> **Note: Basic Auth is now deprecated in Exchange Online -** [https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/deprecation-of-basic-authentication-exchange-online](https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/deprecation-of-basic-authentication-exchange-online) / [https://techcommunity.microsoft.com/t5/exchange-team-blog/improving-security-together/ba-p/805892](https://techcommunity.microsoft.com/t5/exchange-team-blog/improving-security-together/ba-p/805892)

## Determine if you are affected.

When providing an email address and password for email authentication in the Publishers alerts tab, if that mailbox does not SMTP authentication enabled, you will see the following error message.

_An error occurred while sending the test mail: The SMTP server requires a secure connection, or the client was not authenticated. The server response was: 5.7.57 Client not authenticated to send mail. Error: 535 5.7.139 **Authentication unsuccessful**, **SmtpClientAuthentication is disabled for the Mailbox.** Visit https://aka.ms/smtp\_auth\_disabled for more information. HResult: -2146233088._

![](/_images/SMTP-Blog-Picture1.png)

If Authenticated SMTP is already enabled, but your Entra ID Tenant has Security Defaults enabled, Publisher will still be unable to authenticate and show this error instead.

_An error occurred while sending the test mail: The SMTP server requires a secure connection or the client was not authenticated. The server response was: 5.7.57 Client not authenticated to send mail. Error: 535 5.7.**139 Authentication unsuccessful, user is locked by your organization's security defaults policy**. Contact your administrator. HResult: -2146233088. Please see kb: https://patchmypc.com/troubleshooting-smtp-email-sending_

![](/_images/SMTP-Blog-Picture2.png)

## Resolution

To allow Publisher to send email reports through Exchange Online, you will need to configure an account to be allowed to authenticate using SMTP authentication and disable Security Defaults if enabled, following the steps outlined below

### Ensure your account is licenced to be a mailbox

In the example below I am using Exchange Online (Plan 2), but there are alternative licences available.

![](/_images/SMTP-Blog-Picture3.png)

### Enable SMTP authentication on your licenced mailbox

Navigate to the Microsoft 365 admin center and select Active Users - [Active users - Microsoft 365 admin center](https://admin.microsoft.com/#/users), Then select the mailbox you wish to configure, click on Mail, followed by Manage email apps.

![](/_images/SMTP-Blog-Picture4.png)

Check the box for **Authenticated SMTP** and click Save changes.

![](/_images/SMTP-Blog-Picture5.png)

### Disable Security Defaults in Entra ID

> **Note:** This may not be applicable if your tenant was created before October 2019 and you have not configured any policy around this, or you may already have it disabled for other reasons.
> 
> Tenants created after October 2019 have Security Defaults enabled by default, [Raising the Baseline Security for all Organizations in the World - Microsoft Community Hub](https://techcommunity.microsoft.com/t5/microsoft-entra-blog/raising-the-baseline-security-for-all-organizations-in-the-world/ba-p/3299048)

Navigate to your Entra ID Tenant overview, select Properties, Manage security defaults, Switch to disabled, click Save

Note: You can read more about security defaults and the impact this change may have here: [https://learn.microsoft.com/en-us/entra/fundamentals/security-defaults](https://learn.microsoft.com/en-us/entra/fundamentals/security-defaults)

[Security defaults - Microsoft Entra admin center](https://entra.microsoft.com/#view/Microsoft_AAD_IAM/TenantOverview.ReactView)

![](/_images/SMTP-Blog-Picture6.png)

### 1000 years later (When these changes propogate)...

Wait a while (It took about an hour for me) for it to take effect, and then retry the test email in Publisher.

![](/_images/SMTP-Blog-Picture7.png)