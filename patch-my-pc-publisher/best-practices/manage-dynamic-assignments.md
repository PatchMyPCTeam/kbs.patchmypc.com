---
title: Manage Dynamic Assignments
date: 2022-04-27T00:00:00.000Z
taxonomy:
  products:
    - patch-my-pc-publisher
  tech-stack:
    - null
  solution:
    - null
  post_tag:
    - null
  sub-solutions:
    - best-practices
    - deployments
    - application-and-update-publishing
---

# Manage Dynamic Assignments

In this article, we will cover the **Manage Dynamic Assignments** feature that allows you to publish application updates to Intune that will dynamically assign to security groups based on pre-defined search criteria.

We will discuss how the feature works and recommend some key best practices using it.

This feature requires the Enterprise Plus subscription.

### What are "Dynamic Assignments"

Prior to this feature, the Patch My PC Publisher was able to create assignments via the right click option [Manage assignments](https://patchmypc.com/custom-options-available-for-third-party-updates-and-applications#ManageAssignments). This enabled customers to define Available, Required, and Uninstall assignments which would always be applied to newly published Intune Apps and Intune Updates. These could be referred to as "static assignments".

While this provided customers with granular control and consistency, other customers wanted the ability to still automatically create assignments but only apply some groups if the **update contained a CVE**, or was **classified as a security update**, or contained a **specific keyword in its title** etc. This is the premise of **Dynamic Assignments**.

With Dynamic Assignments, the Patch My PC Publisher will evaluate every new update against a pre-configured set of rules (or "search criteria") to determine if an update should be assigned to a specific security group. In this way, Dynamic Assignments can also be thought of as [Automatic Deployment Rules](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/automatically-deploy-software-updates) (ADRs)... but for Intune!

### Evaluation Criteria

Below is a list of the available search criteria you can use to dynamically search and create assignments with using **Dynamic Assignments**:

* **Has CVE**
  * This is a Boolean property. Click the value to switch between **true / false**
* **Severity**
  * Multi-select list of the below values
    * Critical
    * Important
    * Moderate
    * Low
* **Title**
  * Plain text or Regular expression strings can be used to aggressively filter the evaluation.
  * Exclusions can be achieved by adding a "-" to the beginning of the string.
* **Update classification**
  * Multi-Select list of the below values
    * Updates
    * Critical Updates
    * Security Updates

> **Note:** To understand how Patch My PC classifies an update, please see this article: [**Aligning to Microsoft Standards for Update Classifications**](https://patchmypc.com/aligning-classifications-to-match-microsoft-standards-for-third-party-software-updates)

For example, you could create an evaluation criteria that only assigns updates with a CVE ID associated with them, of "Critical" severity, and classified as "Critical updates" and "Security updates":

![](/_images/adr3.png)

Search criteria options which allow multiple values (Severity, Title, Update Classification), **multiple values will be joined with an "OR" operator**. However, all search criteria options will be joined by an **"AND"** operator.

For example, looking at the above screenshot, the criteria for **Update Classification** will search for updates with the classification **Critical Updates** **OR** **Security Updates**. Therefore, the **Has CVE**, **Severity**,and **Update Classification** will all be joined together with an **AND** operator.

In English, the above rule will search for updates which contain a CVE ID, and severity of "Critical", and has Update Classification of "Critical Updates" or "Security Updates".

The following illustration of the same rule above may also help with understanding:

> ```
> Has CVE = "True" AND 
> Severity = "Critical" AND
> (
>   (Update Classification = "Critical Updates") OR (Update Classification = "Security Updates")
> )
> ```

> **Note:**  Dynamics Assignments will apply assignments to newly published updates in the same sync. In other words, it will not apply assignments to updates which already exist in Intune.

### How to Configure Dynamic Assignments

In this section we will detail how to leverage the Dynamic Assignments feature.

To configure and manage Dynamic Assignments, in the **Intune Updates** tab, right click on the **All Products** node and select **Manage dynamic assignments:**

![](/_images/adr1.png)

Opening the feature will bring you to a list of existing evaluation rules. To create a new rule, select **Add.**

![](/_images/adr2.png)

We will now build a dynamic assignment rule. Below is an example of a rule that will select only application updates that have:

* CVEs attached
* Severity classification of "Critical"
* Update classification of "Critical Updates" OR "Security Updates"

![](/_images/adr3.png)

To validate what application updates will pass evaluation, we can press **Preview** (bottom left) to run the evaluation rule against the catalog.

In the example below, the evaluation rule has returned only the products that meet the dynamic assignment rule from above.

![](/_images/adr4.png)

Once you are happy that with your new evaluation search criteria, you can associate Intune assignments with the rule by selecting the **Manage** button.

![](/_images/adr5.png)

This will bring up the standard **Manage Assignments** window used for existing "static" assignments. Here you can define either "Required" or "Uninstall" assignments, with the normal options to add Azure Active Directory groups and all the other assignment options, too, including notification options, installation deadline availability and deadline, and restart grace period.

These will be the assignments associated with the **Dynamic Assignment** rule.

![](/_images/adr6.png)

Once assignments are added, we can verify that the assignments have been applied by checking the **Assignments** list on the previous window.

![](/_images/adr7.png)

Once we are happy with our new rule, clicking **OK** will return us to the list of dynamic assignment rules, which should display our newly created rule.

![](/_images/adr8.png)

> **Note:** In the screenshot above, we can verify that the rule has assignments attached to it - this means the rule is "Active" and will perform evaluations when the publishing service runs next.

### Notes:

With this feature, there are some important things to take into consideration:

* Unlike Configuration Manager, where a product can be deployed to the same collection more than once, with Intune, an application can only assign to a security group once. What this means is that if you have an existing **static assignment** set to assign an application to "Group A" and you also set a dynamic assignment rule to assign to "Group A" based on certain criteria, a conflict will occur. In the case of a conflict, **Dynamic Assignments will always supersede static assignments.**