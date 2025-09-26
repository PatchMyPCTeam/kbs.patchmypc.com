# How does the Microsoft Intune Multi Admin Approval feature affect Patch My PC products

yaml global product = Advanced Insights

yaml global solution = Endpoint Management

yaml global tech Stack = ConfigMgr

The Microsoft Intune [Multi Admin Approval](https://learn.microsoft.com/en-us/mem/intune/fundamentals/multi-admin-approval) (MAA) feature introduces the option for administrators to add an additional layer of security when creating or modifying scripts and apps in Intune by requiring admin approval before changes become effective.

MAA **does not** impact the ability for any Patch My PC products to succesfully create and modify Win32 apps in Intune. Patch My PC continues to operate seamlessly with Intune, and no adjustments are needed when using the MAA feature. This article will provide an overview of  why this Intune feature has no effect on Patch My PC’s functionality

### Why are Patch My PC products not affected by MAA? <a href="#h-why-are-patch-my-pc-products-not-affected-by-maa" id="h-why-are-patch-my-pc-products-not-affected-by-maa"></a>

MAA only affects changes to applications when the delegated authentication flow is used. This could be when interactively creating/updating applications in the Intune Admin Centre or by invoking web requests to the Microsoft Graph directly.

Here is an example of the MAA feature in action within the Intune Admin Center. When an assignment is removed from a Win32 app, the change must be approved.

![](/_images/MAA-Intune_Admin_Center.jpg)

Here is another example of a response when using a delegated authentication flow to make changes to a Win32 app when a MAA access policy has been created.

![Multi Admin Approval prompt](/_images/MAA.png "Multi Admin Approval prompt")

Patch My PC Cloud and Patch My PC Publisher utilises the the application authentication flow, not a delegated authentication flow. This is why our products are unaffected by any MAA access policies.