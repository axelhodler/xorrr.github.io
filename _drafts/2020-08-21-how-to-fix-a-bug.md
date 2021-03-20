---
layout: post
title: "How to fix a bug"
date:   2020-08-21 17:21:22
description: ""
tags: []
---

During software development there are lots of things that can go wrong. One of these things are software bugs. Things are not working as they are supposed to. In some cases the application does not work or crashes. Some say bugs should be called mistakes. Programmer mistakes. Maybe the programmers were sloppy or not experienced enough. Mistakes were made. Mistakes will be made.

We consider bugs fine as long as they do not reach the end user. If they reach the end user the effect will range from a slight bother to being painful. One could consider it an embarassment for the developer team.

How would one achieve that goal? There are techniques such as Test Driven Development and lots of ways to improve the correctness of software. As Dijkstra would say

> "Program testing can be used to show the presence of bugs, but never to show their absence!"

The earlier we are able to detect a bug the easier it is to eliminate it. Sometimes a bug is not caught by the development process itself.

It is very helpful if software is released to an internal group first. Maybe the group uses the app anyway. Maybe people in the organisation are just interested in what the others are working on.

Make it easy to report the bug. In a Slack or Teams channel. If a developer has found the bug they will almost always provide the necessary steps to reproduce the bug. Having a video is great.

The first step would be prioritizing the app. We believe in fixing it as soon as possible. Both to avoid the trap of discouraging the people writing the bug report


Then the goal is to reproduce the bug. After that some run straight into understanding and fixing the bug. The experiences with UI Tests differ between developers. Everyone has an opinion. Depending on how you write them UI tests might be slow or brittle. One of the benefits though is


In our case it was a performance issue of iOS.

Now we can drill down with unit level tests. What's a unit test? It is a lot faster and lot more focussed.
