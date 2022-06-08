---
layout: post
title: "Trunk based development"
date:   2022-03-27 06:30:22
description: ""
tags: []
---

At my first _real_ job, back in 2014, we were using the version control system svn for the backend. The mainline branch - think master or main in git terms - was called `trunk`.

Every developer pushed changes straight to mainline (`trunk`). There was no code review before adding changes to mainline. Me, having worked with git previously - in fact most projects in the company where using git already - thought this was subpar. Almost awful. Not the way things are done in modern software development.

Later someone was added to the team in charge of moving us to continuous delivery. When discussing branching strategies yours truly, being an avid reader of [hackernews](https://news.ycombinator.com/) and all things development related, was eager to push for Gitflow.

The response I got back was

> Yeah, but thats not really continuous delivery

Boy was I shaken. Isn't this the cool git branching model everyone is using?

Back then I did not get it. Mostly because we did what most folks do. Have our own definition of what _continuous_ is supposed to be.

In our case it was after every sprint. A release every two weeks - If, and only if, nothing goes wrong.

Of course every two weeks is better than every few months. And every week is better than every two weeks. But not what I would consider continuous today.

We ended up having feature branches, some short lived (days), some long lived (months). These changes were moved to mainline after one or more reviews.

The workflow had its issues but was, from my perspective, way better than to contributing straight into mainline.

Years later, when Microservices were all the rage, I watched a talk by [Sam Newman](https://www.youtube.com/watch?v=lqRQYEHAtpk&t=2425s)

Some of his comments on Gitflow in the talk

> Because if there is something worth doing, there is something worth doing really complicatedly

> I don't understand all this because I'm old and quite stupid

> Every single picture that shall explain to me how Gitflow works just makes we want to kill myself

Well the last one is an exaggeration. But taking the

> I don’t understand all this because I’m old and quite stupid

from a guy of his calibre is quite humbling.

Today I would agree.

Getting to _"short lived"_ _"feature branches"_ is hard. Both due to a definition what _"short lived"_ is exactly and the common question how the whole _"feature"_ - it's a _"feature branch"_ after all, can be implemented in a few days, or even hours.

If we get quick reviews, meaning colleagues get into the review ASAP instead of once or twice a day, we might get small branches and thus small pull requests (PR)

* Branch -> Fix a flaky test -> PR
* Branch -> Fix a typo in the docs -> PR
* Branch -> Refactor a method name -> PR
* Branch -> Create the DB migration for the feature -> PR
* Branch -> Add a failing, ignored, acceptance test for the feature -> PR
* Branch -> […] -> PR

And if we have feature toggles in place like

{% highlight python %}
if current_user_sees("our_awesome_future_feature"):
  show_new_feature()
{% endhighlight %}

and thus only specific users, or groups, will be able to see the new functionality in production. We can split the feature into

* Branch -> Add a button without functionality leading to the new feature -> PR
* Branch -> Create the scaffolding (UI) for the new feature -> PR
* Branch -> Create the REST API for the new feature -> PR
* Branch -> Integrate the REST API for the new feature -> PR
* Branch -> Use our corporate design for the new feature UI -> PR
* Branch -> Adapt feedback from the UX folks -> PR
* Branch -> Improve the performance -> PR
* Branch -> […] -> PR

See [Structuring work for continuous integration](https://www.hodler.co/2021/09/07/structuring-work/) for more examples.

Or as Kent Beck calls it

> First make the change easy, then make the easy change

If we don't get quick reviews the developers will bundle these changes into one big change. This automatically leads to longer lived feature branches.

To avoid the hassle of slow reviews we can try adding a second pair of eyes right away by pair programming or even multiple pair of eyes by mob programming.

Instead of having multiple branches we push small changes into main, or master, or trunk. Together as a team.

That is [trunk based development](https://trunkbaseddevelopment.com/).

Trunk can always be released. It could even be released on every push. That's the strategy.

Users will not care about unfinished features because they will not see them.

Only the developers. Or any other group configured to see them. When the feature is done the flag can be removed and everyone is able to see and use it.
