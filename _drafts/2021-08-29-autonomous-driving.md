---
layout: post
title: "Getting into autonomous driving"
date:   2021-09-12 09:30:22
description: ""
tags: []
---

In the next weeks I'll start supporting the [Sounce]() team as a Software Architect. The goal of Sounce is support the shopfloor environment with AI supported accoustic tests.

My knowledge on AI is lets rather rudimentary.

At one point I have realized the best way to learn something is to try to solve a problem and to learn everything you need on the way of solving the problem. In the end you solve the problem and you learn something.

For quite some time I am following the development of [comma.ai](https://comma.ai/). They build hardware to improve the driver assistance capability of most new cars.

If we search through their Jobs page the Internship position contains a link to a calibration challenge. Reaching under 25% error on the challenge can be a way to get the internship.

I don't have an interest in the internship, but definitely in learning what it takes to solve. Especially with zero clues on how to approach self driving.

The goal of the calibration challenge is to predict the direction of travel using a provided dashcam video.

To achieve this we get 5 videos, labeled with their direction of travel using pitch and yaw angles in radians. The other 5 videos do not have labels. We need to create these labels.

Already the sentences contain words I know nothing about. Pitch and yaw is some kind of aeronautics lingo. After consulting wikipedia we learn

<img src="https://upload.wikimedia.org/wikipedia/commons/c/c1/Yaw_Axis_Corrected.svg" alt="pitch yaw" />

Let's check out the data

{% highlight bash %}
3.346066188150387949e-02 3.149205029088487234e-02
3.332004697594769665e-02 3.131719816086165481e-02
3.326381557788743448e-02 3.120279874728593833e-02
[...]
{% endhighlight %}

In the first line `3.346066188150387949e-02` is the pitch angle.
`3.149205029088487234e-02` is the yaw angle. π (Pi) radians is 180 degrees.

<img src="https://upload.wikimedia.org/wikipedia/commons/4/4e/Circle_radians.gif" alt="radians" />

If `yaw < pi` we drive to the right. Else we drive to the left.

If `pitch < pi` we drive up

Let's break out some code. In data science world they use jupyter notebooks.

{% highlight python %}

{% endhighlight %}

If we check out the labels of another 

{% highlight bash %}
6.239283637564226742e-02 1.159702385731047752e-02
6.228698778976032857e-02 1.173878080124349244e-02
6.242373445383316011e-02 1.185748319654455867e-02
[...]
{% endhighlight %}

Oh, a different yaw angle. Instead of about Pi radians it is about 1 radian

Ok let's have a break. Can't we just use ML to solve the issue? After all we have labeled data and unlabeled data.

Small datasets lead to overfitting. 

In [hints](https://github.com/commaai/calib_challenge#hints) they state

> The dataset is tiny, use caution if using ML.


> The devices that run openpilot are not mounted perfectly. The camera is not exactly aligned to the vehicle. There is some pitch and yaw angle between the camera of the device and the vehicle, which can vary between installations. Estimating these angles is essential for accurate control of the vehicle. The best way to start estimating these values is to predict the direction of motion in camera frame

Let's watch a video, i'll be using [mpv].

It will also provide us the resolution of the video `hevc 1164x874`