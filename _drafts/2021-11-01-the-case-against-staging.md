---
layout: post
title: "The case against staging"
date:   2021-11-01 06:30:22
description: ""
tags: []
---

In Software Development the majority of professional projects, or products, use multiple environments. "Dev" (short for development), staging, "Prod" (production ("prod") or more. Most have at least two. Development and Production.

Production is what the customer uses. Development is where the developers can test changes or show them to stakeholders. These changes are usually meant to move to production at one point. Maybe going through multiple environments, or stages, in between. One of these environments is often called staging. Sometimes pre-production ("pre-prod"). Other times there are additional environments such as consolidation.

The closer we get to production the more stakeholders care if something does not work. A service disruption on dev is no big deal. On prod it is a big deal. The result could be lost revenue, damage to the reputation of the service or even loss of live.

Having multiple stages is supposed to alleviate the damages above. The idea is to find and fix potential issues before they reach production. If something does not work on development the developers can fix it right away. Other stakeholders might not even see it. If something goes wrong on staging, some stakeholders, but not the customer, might see it.

There are some issues in this train of thought. On first glance it seems true. When diving deeper there are multiple ways where staging can differ from production. Meaning if it works on staging it does not guarantee it will be working in production. It might even give us a false sense of confidence.

## Data differences between staging and production

Whatever is stored in our database in staging is going to differ from what is stored in production. It could be the sheer amount of data. Production contains all the data of our users. Staging might only contain some sample data. What might look like a well performing application in non-production might slow down to a crawl in production.

In staging there might be some experimental tables, or collections, laying around that were created to test a feature. At multiple times maybe the a whole table, or all, was dropped and recreated to start from a clean slate. Maybe years ago someone, not longer on your did, made manual changes to production no one knows anything about.

As a result we might only try database migrations on non-production before attempting to use them on production

## Third party services differs

If we consume internal services, e.g. microservices of other teams, these might have their own staging environments too.


We already do this when consuming services from google. There is no staging Google Maps API. At least not one that is open to the public.


## Infrastructure differences

Using cloud services there are bills to pay. If all stages are using the same resources we can multiply the cost by about the amount of stages. As an example, say case production costs us 10.000€ a month then having development and staging too might amount to 30.000€. Maybe less because the load on development and staging is lower than on production.

As a result there might be cost saving ideas. Using cheaper resources and plans. Shutting down the development and staging environment during non working hours, such as during the night or on the weekend.

Aside from costing us more it is also hard to find any issues that might arise from having the service run longer than a day. After all the services in staging are shut down every night while production keeps running.

## Added complexity

Our pipelines need to be adapted. Maybe master is deployed straight onto development but

Cons that is not used but has to be migrated.

Stakeholders are asking what is currently deployed
