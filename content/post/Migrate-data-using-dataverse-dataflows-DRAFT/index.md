---
title: Migrate data to Dataverse using dataflows
description: Use dataflows to migrate business data from external systems to Microsoft's Dataverse
slug: Migrate-data-using-dataverse-dataflows
date: 2023-04-12 00:00:00+0000
draft: true
image: software.jpg
categories:
    - Power Platform
    - Solutions
    - Migrations
    - Dataflows
    - Dataverse
tags:
    - Power Platform
    - Dataverse
    - Migration
    - Data
---

Microsoft Dataverse is a great place to store Business data if you are using other aspects of the Power Platform as it offers native integrations and is easy to use (once you know what you are doing!).

If you have existing Business systems you wish to migrate, it can be a challenge getting the data into Dataverse without knowing where to start. Having competed a project recently which migrated Business data from **SharePoint to Dataverse**, I thought it would good to document the processes and tools used to establish a migration strategy and migrate data into Dataverse.

### Know your Source data

The first step in migrating your data is understanding your source data. In my example I will be using SharePoint with different column types and migrating these to Dataverse tables. Once I know what my data look like in SharePoint, I can then start to shape the data into an Entity relationship diagram to give me an idea as to what needs to go where.

In my example I have 3 SharePoint lists, Assets:

<Image placeholder

Vendors:

<Image placeholder

and Locations:

<Image placeholder

The Assets list is effectively the "master" list containing the column types:

    * Choice
    * Date/Time
    * Person
    * Number
    * Text
    * Yes/No
    * Lookup
    * Multiple-selection Lookup

The Lookup column pointa to the Location List and the Multiple-Selection Lookup points to the Vendors List.
Now that we have broken the list down, we can start to explore what this will look like in Dataverse. This is a relatively straightforward exercise in this example as it's only a small number of lists, however the same principles apply when migrating at a larger scale.

Based on the existing relationships between the lists, it would make sense to replicate this structire in Dataverse, an when creating the Entity relationship diagram, it gives us the following Table structure:

<Image Placeholder

What this shows is that we should create 3 tables, one for Assets, one for Locations, and one for Vendors.

### Keys, keys keys!

Now that we know how our tables should be set up in dataverse, we will go ahead and create them and set them up using the correct column types as needed based on our Entity relationship diagram.

For both the Locations and Vendors Table, we will need to set up an alternate key in Dataverse. This allows for the column data to be migrated from SharePoint to Dataverse, otherwise the columns won't show up as an option when configuring dataflows.

I would reccomend when setting up your key it is uniquely named as to avoid any conflicts or oddities, for this reason I would also reccomend naming your primary coluns something other than "Name" in Dataverse. For scenarios where you will have multiple keys on a table, you can select multiple columns to act as a key. This is useful when you need a composite key or to migrate a many-to-many relationship.

<image placeholder

