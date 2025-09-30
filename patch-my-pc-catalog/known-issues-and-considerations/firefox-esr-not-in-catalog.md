---
title: "Why is the newest version of Firefox ESR not available in the Application Catalog?"
date: 2024-04-04
taxonomy:
    products:
        - patch-my-pc-catalog
    tech-stack:
        - 
    solution:
        - 
    post_tag:
        - 
    sub-solutions:
        - known-issues-and-considerations
        - application-and-update-publishing
        - updates
---

## Symptoms

When looking in the Patch My PC Application Catalog, why do I see an older version of Firefox/Thunderbird ESR listed compared to what is available according to the [Release Notes](https://www.mozilla.org/en-US/firefox/organizations/notes/) on the Mozilla website?

## Cause

This issue occurs because Mozilla keeps two versions of the Extended Support Release (ESR) version of Firefox/Thunderbird live for three-point releases whenever they issue a major ESR release:

- An older stable version.

- A newer version that is potentially unstable but can be rapidly stabilized by Mozilla as issues are resolved.

## Resolution

As Mozilla officially supports both versions, they provide the same vulnerability patches to both versions until they reach their end of life.

However, adding newer ESR versions of Firefox/Thunderbird every time Mozilla releases them would significantly increase the number of Firefox/Thunderbird updates in our catalog and potentially introduce complicated applicability considerations.

We could add the newer unstable ESR versions as separate products, but these would only be valid during this 3-month transition period.

Therefore, at Patch My PC, we have decided only to publish the stable ESR versions of Firefox/Thunderbird, as we assume customers choosing the ESR release are doing so for its stability benefits.