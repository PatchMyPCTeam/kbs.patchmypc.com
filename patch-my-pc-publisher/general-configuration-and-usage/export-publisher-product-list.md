---
title: "Export Publisher Product List"
date: 2021-07-31
taxonomy:
    products:
        - patch-my-pc-publisher
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - general-configuration-and-usage
        - education
        - licensing
---

In this article, we will cover the feature with the Publisher that allows you to export the current list of enabled products and their right-click configurations.

## How to use it

This option is found within the Advanced tab of the Publisher. You will need to scroll down in order to find the Product Export section. The section can be seen below.

![Product Export](images/ProductExport.png)

Once you have found the section exporting the list is as simple as:

1. Check the boxes of the types of products you would like to export

3. Press the 'Export' button.

5. Select the location for the export CSV file.

## Notes

With this feature, there are some things to take note of. 

- You can only check a box to export a product type if the below conditions are met. If the conditions are not met the checkbox will be grayed out, such as the 'Export Intune Updates' checkbox above.
    - The product type is enabled (checkbox at the top of each tab)
    
    - There is at least one product of the type selected

- Some complex properties, such as those listed below, are exported as JSON strings in the CSV.
    - Intune assignments
    
    - Custom display info such as name, description, localized name, etc.
    
    - Manage Conflicting Process options

- Some properties have a 'default value' and so they may be populated but not in use.
    - For example, the Intune apps and updates may show a value for the max run time that is in use by ConfigMgr.
