---
layout: post
title: "Threat Modeling"
date:   2021-08-29 09:30:22
description: ""
tags: []
---

Threat Modeling allows us to identify potential threats and vulnerabilities in our software.

One way is to apply the STRIDE methodology for finding attacks. STRIDE is an acronym and stands for
Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of privilege.

Spoofing could be an attacker acting as someone else, this could be the attacker taking the role of the CEO and asking an employee for confidential information.

Tampering could be a way to ignore any validation to spend less on an item than it costs.

Repudiation is a way to allow the attacker to state "I did not do that", "I did not buy that Porsche".

__Information disclosure__ is finding out "Who else bought a Porsche?"

_Denial_ of Service allows the attacker to make sure no one is able to reach the application.

_Elevation_ of Privilege allows the attacker to get access to things he should not be able to have access to. Such as admin panels.

There are two requirements

Get a model of your software and add trust boundaries. If the system uses [arc42](https://arc42.org/) to document the software architecture we can use the business context diagram to draw our trust boundaries.

Trust boundaries should only cross data flows. They delineate the attack surface between principals (Application, Platfrom, Third party services the application communicates with).

There is a cardgame, the repository is available on [Github](https://github.com/dehydr8/elevation-of-privilege) a deployed version is available [online](https://elevation-of-privilege.herokuapp.com/)

It is important to fixate on the attacks. The goal is to get an overview of potential attacks and issues where we are not sure yet if we are protected against it.

How we fix the issue is handeled in another workshop. Plus, spending time on how to fix the issues makes it impossible to finish the workshop in a timely manner.

When being strict with the time, but also allowing people to discuss the workshop should take from 90 to 120 minutes. Less if people are already experienced.

----


Afterwards we go through each threat and decide on a strategy and tactic for addressing them. We could create a bug per threat. Putting them in the same macheinery we already use to assure quality.





