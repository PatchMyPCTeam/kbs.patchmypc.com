---
title: "Patch My PC Publisher Now Writes to the Windows Event Log"
date: 2019-10-03
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
        - best-practices
        - automation        
---

We recently had a customer request our Publishing Service save certain events of publishing operations to the Windows event log.

In the customer's scenario, they wanted to automate specific actions based on certain events. Using the Windows event log with specific event ID's would allow for an easier experience than parsing the PatchMyPC.log

![Windows-Event-Log-Publishing-Service-PatchMyPC](/_images/Windows-Event-Log-Publishing-Service-PatchMyPC.png "Windows-Event-Log-Publishing-Service-PatchMyPC")

Below are the current **event ID's** we use for the actions listed below.

## List of Events

\[table id=27 /\]