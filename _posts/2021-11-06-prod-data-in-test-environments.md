---
layout: post
title: "Production data in test environments"
date:   2021-11-06 06:30:22
description: ""
tags: []
---

The thoughtworks Technology Radar has added the technique [Production data in test environments](https://www.thoughtworks.com/de-de/radar/techniques/production-data-in-test-environments). It has the status "Hold". Short for "Don't do that".

I was asked what my thoughts are.

It seems people having issues with the technique probably do use production data in test environments. Having worked on such systems, all the listed drawbacks are true. Some are more of an issue than others.

Protection of private data is important and of legal relevance. Sending _incorrect alerts_ from test systems to real clients can happen on production too. There are measures to avoid this. Previews that show who, or even a number of how many, will get the message. Or sending it to an internal group first before sending it to everyone applicable for the notification.

Taking a step back.

Why do people put production data into test environments?

* To fill the databases with more than only a few bytes?
* To check if the queries are fast enough on larger datasets?
* To check if our database migrations are successful on real data?
* To actually display something in the app?
* To reproduce bugs?
* To make it look _real_ for demoing purposes?

Valid reasons.

Before we discuss using bogus or obfuscated data as an alternative let's take another step back.

Thinking from first principles - __Why is there a test environment at all?__

What if we were reproducing bugs in production?

What if we demo the production app instead of an app connected to a test environment?

What if we turn it around and add test data to our production environment?

A fake hotel that only testers can see and book, a fake car that only testers can see and buy, a fake article that only testers can read and share.

A new feature that only testers can see and use?

Using a simple [A/B Test](https://en.wikipedia.org/wiki/A/B_testing) where one of the groups, or a third group, is developers, testers, other stakeholders, project managers or anyone else that is not the end user.

I always try to add this concept to products and projects I'm working on.

### Examples

The roadtrips in the app below were not visible for everyone. They might just be there for testing. Instead of crossing the alps you can do a roadtrip from and to the office. Or check out how the content looks and feels before the customer sees it.

All in production.

<img src="https://www.dropbox.com/s/xdvpi1cshywgjjn/roadtrip.PNG?raw=1" class="image__ios image__center"/>

Another example.

I do not drive the car below. Aside from the name the car might not even exist. Still it is mine in the production app. It is test data in production.

<img src="https://www.dropbox.com/s/zrcs7hqhgjft1o9/pfsapp.PNG?raw=1" class="image__ios image__center"/>

Do we now have the solution for the whole production data in test environments?

Guess not. It's providing options.

Having production data in test environments is an option.

Having test data in production environments is an option.

