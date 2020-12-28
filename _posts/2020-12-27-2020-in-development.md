---
layout: post
title: "2020 in development"
date:   2020-12-27 10:21:22
description: ""
tags: []
---

An overview of technologies and services I had the pleasure to use in 2020.

One project on the job with Vue.js for the client and Google Cloud (Firestore, Functions, Authentication) for the backend. The other, still ongoing, using SwiftUI on iOS and a Spring Boot application, written in Kotlin, running on AWS Fargate.

For side projects I was using SwiftUI and Vue.js on the client and vercel.com, with mongoDB and SendGrid, or Firestore on the backend.

## Used from toolbox

* Spring-Boot, Kotlin
* Vue.js
* SwiftUI
* Docker
* AWS
* Google Cloud Firestore
* vercel.com
* mongoDB
* Jekyll

## Used in 2019 but not in 2020

* React
* AWS Lambda & S3
* CloudFoundry
* Shopify
* New Relic
* Bitbucket Pipelines
* CircleCI

## Added to toolbox

* Cypress
* Automated Testing in iOS
* AWS: CDK, Fargate, ECR, SES, SQS, SNS, DocumentDB
* Google Cloud: Functions, Authentication
* Datadog
* Concourse CI
* Github actions
* Bitrise
* SendGrid
* Nuxt.js
* Stripe

## Summary

SwiftUI is great, depending on how demanding the UX team is one still has to dive deeply into UIKit to create heavily customized elements. Diving into UIKit, even for people with previous knowledge, is tedious and in my opinion the cost is not worth the benefit. In private projects I would only use what SwiftUI has to offer. If something is not (yet) supported it seems simpler to find a different approach to solve it with SwiftUI before diving into UIKit.

Having had experiences with Angular, React and Ember working with Vue.js is still a blast. With Nuxt Content it was easy to migrate a static page using Jekyll to static content plus webapp in Vue.js.

Spring Boot, together with IntelliJ, is a great tool and can show iOS developers what could be possible with Xcode and might be in the future.

Firestore is still great if all you want is a database which can offer offline functionality. Working with AWS Fargate is a lot simpler than with plain EC2 if all you are doing is running an application via `java -jar app.jar` or doing the same within Docker. Combined with CDK you can get a zero downtime construct by using [ApplicationLoadBalancedFargateService](https://docs.aws.amazon.com/cdk/api/latest/docs/@aws-cdk_aws-ecs-patterns.ApplicationLoadBalancedFargateService.html).

I enjoyed working with any of these technologies. Pick what is already there or what the team is most comfortable and happy working with. In the end it is all about increasing revenue or decreasing costs for your employer or your company.

