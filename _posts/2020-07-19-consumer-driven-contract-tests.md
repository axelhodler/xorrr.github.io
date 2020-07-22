---
layout: post
title: "Consumer Driven Contract Testing"
date:   2020-07-19 17:21:22
description: ""
tags: []
---

Describing the practice in simple terms

Say we provide a service. Getting a user by its id would return the following infos

{% highlight text %}
curl https://someserver.com/api/users/1
{
  "id": 1,
  "name": "Jane Doe",
  "age": 23,
  "address": "Somestreet 123 in City"
  "premium": true
}
{% endhighlight %}

The id, the name, the age, the address and a field whether the user is a premium user. In reality the service might provide a lot more fields.

The consumer of the service could be an app on iOS or Android. A web app. Or any other backend service.

The provider provides, controls and owns the service. A consumer of the service has an interest in getting some data or using some functionality. Plus the interest for the service not to break.

Let’s think about an iOS app which would display each users age in a list. Maybe for customer relationship management.

There are two common ways to break the service and thus to impair the functionality of the app.

First if the provider decides `users` is not really a great descriptor and should be `reader`. As a result the url would change to `api/reader/1` . A client who does not know of that change could have an application in production which now fails to retrieve the user.

Second by the provider to state the field `age` should not be provided. And instead to provide a `birthdate` field with a timestamp. After all the clients could calculate the age by themselves. Our iOS client consuming the `age` field would break because the service does not provide the `age` anymore.

In both cases some data will not be displayed. In the worst case the app might crash in production. Until a fix is provided by either side.

The expectations between provider and consumer went out of sync.

## Contracts

To resolve the issues above a consumer can provide a contract to the provider. The contract states:

{% highlight text %}
We are consuming the "name" and "age" field of your /users service. We expect "name" to be a String and "age" to be an Integer.
{% endhighlight %}

The provider accepts the contract. As a result the provider knows a client is using the `"name"` and the `"age"` field.

Another service could provide a contract too:

{% highlight text %}
We are consuming the "premium" field of your /users service. We expect "premium" to be a Boolean.
{% endhighlight %}

If every consumer provides a contract the provider will know which fields can be removed because no one is consuming them. The provider will also know who to talk to when changes to parts which are consumed are planned.

The contracts are written in code. No signatures or lawyers required.

## Consumer Driven

Ideally the service would not simply be created and hoped to be consumed somehow. This approach usually leads to a lot of rework and waste. Both because the consumers are not happy with it or are not interested in some of the data or want it in a different format.

The service could be _Consumer Driven_ using _Contracts_

The consumer states

{% highlight text %}
We will be consuming the "name" and "age" field of your /users service. We expect "name" to be a String and "age" to be an Integer.
{% endhighlight %}

The result will be the following service.

{% highlight text %}
curl https://someserver.com/api/users/1
{
  "name": "Jane Doe",
  "age": 23
}
{% endhighlight %}

Anything more than that could be considered waste. Until more contracts are provided or existing contracts are extended.

A side benefit is people talking to each other. The producer might state

> We cannot really provide that, because X. How about the alternative Y?

Some might argue the above is enough of a reason to adopt Consumer Driven Contract Tests.

## Tests

Because no lawyers or signatures are involved the contracts are not as legally binding as some might like. What we can do is test if the contracts are upheld.

If the provider would break a contract an automated test should be failing as a result. Telling the provider to fix it before the break reaches production and thus the consumer.

Although it would often be too late it is helpful if a test fails on the consumer side too if the provider disregards the failing test on his side and pushes the changes.

There we have it. The _contracts_ are _consumer driven_ and covered by _tests_. Consumer Driven Contract Tests (CDCT). Keeping expectations between provider and consumer in sync.

As with everything there are some issues that need to be understood.

### The service could still be unavailable or crash

CDCT makes sure the requests and responses of the service have the correct structure. If the credentials to the database have changed or the database is unavailable the consumer will probably not find the `user`. Even if we have CDCT. Exception handling is still required. CDCT is not an alternative to integration tests.

### Tooling setup and knowledge required

On both the consumer and producer side its necessary to set up some tooling and define a way to work with each other using CDCT. An effort not every team might be willing to spend time on.

### Discussions about the design of the service

Some might not like the fact that the consumers can drive the service specification. Hopefully the team has a healthy way to resolve arguments around these issues.

### Some contracts might be duplicated or overlap with each other

If two consumers use some of the same fields it is helpful if both provide their separate contracts. We don’t want to save time by stating:

> The stuff consumer F wants is already covered in contracts of consumer B and E

Just to have the service consumer F requires to be broken because consumer B stated the contract is not required anymore. We should be explicit on who uses what.

### Contract Management

Depending on the amount of consumers. Plus the amount of version each consumer has out in the wild. Remember not everyone updates the iOS apps all the time. There are a lot of contracts to keep track of. The consumers need to tell the producers which contracts are not required anymore.

### We still need service documentation

It is still helpful to provide service documentation. To learn some intricacies or to simply get an overview of what is available. We can’t know what the service has to offer by looking at a single contract. While looking through all contracts might be tedious.

All of the above is solvable, but should be understood before going forward with CDCT.

Common tools are [Pact](https://pact.io/) or [Spring Cloud Contract](https://spring.io/projects/spring-cloud-contract).

To sum it up the benefits outweigh the potential issues by far. Using CDCT makes sense to have the producer and consumer play nice with each other. Even if the project is small or we have only a single consumer at first.

Article has been cross-posted on [Medium](https://medium.com/@axelhodler/consumer-driven-contract-tests-b483aad9cbdf).
