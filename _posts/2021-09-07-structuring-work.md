---
layout: post
title: "Structuring work for continuous integration"
date:   2021-09-07 09:30:22
description: ""
tags: []
---

Consider our product has a bunch of included services. Warranty, insurance, additional drivers and more. The services are displayed in a grid. Our users might, or might not, know about them yet.

What do these included services mean exactly? All we get is an icon and a title?

Maybe the feedback made it clear people had further questions when looking at the services - Maybe adding additional context is a legal requirement.

We decide to add more context. One approach is the common "info" ( __i__ ) icon or, in our case, the "Show details" (_Details anzeigen_) button.

The button on the bottom left leads to the detail view on the right.

<p class="image__pair">
<img src="https://www.dropbox.com/s/i5wzoc8vl15xffo/details.PNG?raw=1" class="image__ios image__ios-left"/>
<img src="https://www.dropbox.com/s/qydqa5u79fu7qro/context.PNG?raw=1" class="image__ios image__ios-right"/>
</p>
All we need to do is to display some text in a view and implement a way to get there.

Not too hard.

The common approach developers take is

1. Create the button
2. Have the button open a modal (a sheet in iOS terminology) - Pressing Done (_Fertig_) closes the sheet - The view on the left is displayed again.
3. Layout the content of the sheet

This is fine. It gets the job done.

<p class="image__pair">
<img src="https://www.dropbox.com/s/uec3l2llz8yyssq/step_1.PNG?raw=1" class="image__ios image__ios-left"/>
<img src="https://www.dropbox.com/s/uwsm128xg1e7lq8/step_23.PNG?raw=1" class="image__ios image__ios-right"/>
</p>

Can we improve upon it?

Yes.

Consider continuous integration. We merge to master as often as possible. Multiple times a day - even multiple times per hour.

Using the approach above we cannot merge to master directly after creating the button. Yes, technically we can, with the result of having a master state
that cannot be released __right now__. Consider someone in the team finds a bug which needs a bugfix release ASAP. Only possible after we remove the added button. The button without functionality.

If a user discovers the button he will press it and nothing would happen. Step 2 (opening the sheet) is not yet implemented.

Even if it was, without Step 3 (layout the sheet) the user would see a blank page.

What if we were to change the order of the steps in our approach?

We start with the content of the sheet. No need to commit to the sheet element yet. Maybe the designers will show up a few minutes later with the decision
to have the view use the full screen?

If we layout the content first we are able to merge. Due to SwiftUI we also do not need to have a button to allow us to see our view. That is what previews are for.

The current state is mergeable to master. Even if there is some pending release it would not matter. After all, the view does not have an entry point yet. We did not implement the button. No user will be able to see the view. We can collect feedback from other developers or designers on the contents of the sheet without the necessity to think on how the user gets there.

As a result the approach changes to

1. Layout the content of the sheet
2. Create the button
3. Have the button open the sheet

<p class="image__pair">
<img src="https://www.dropbox.com/s/f1r1lze8s5urgrr/step_3a.png?raw=1" class="image__ios image__ios-left"/>
<img src="https://www.dropbox.com/s/75jklgiwh72zqtz/step_23a.png?raw=1" class="image__ios image__ios-right"/>
</p>

We have split a single unit of work, which has to be merged as one, into two units of work, mergeable one by one.

Is the change in question too trivial to warrant a change of our "usual" order? The statement _"We can do this in an hour"_ comes to mind. Fair enough.

Consider we had to implement some logic that fetched the content of the sheet from a server. Suddenly we have to deal with http requests, API design, possible errors due to being offline or an unreachable server.

What if, due to the possibility of collecting feedback earlier, we had to change the design of the sheet multiple times?

A feature once considered _trivial_ before is not _trivial_ anymore.

