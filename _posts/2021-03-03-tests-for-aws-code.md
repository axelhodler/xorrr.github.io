---
layout: post
title: "Tests for AWS CDK code"
date:   2021-03-03 19:22:22
description: ""
tags: []
---

The [AWS Cloud Development Kit](https://aws.amazon.com/cdk/) (CDK) allows us to __define our cloud infrastructure using code__. Instead of clicking through the web UI, or AWS console as it is called, we use code. Code that can be checked into version control for collaboration, annotated with documentation and certainly for testing.

The following post focusses on the testing aspect.

If we choose TypeScript we can initialise a CDK project with the following command after having installed `aws-cdk`.

```
cdk init app --language typescript
```

The project ends up with a generated empty stack. As the comment states we have not defined anything yet.

<script src="https://gist.github.com/axelhodler/eecbba79cf27327b0b0924b2d100ca95.js"></script>

As an addition an initial test case has been generated in the `test` directory of our project. It makes sure to test the fact __our stack is empty__. As defined above. Having the test directory and an initial test already tells us the CDK code can be tested and the intention of the framework authors to have tests.

<script src="https://gist.github.com/axelhodler/6acb2c90889e8c59a672a632768e5799.js"></script>

We can run the tests via `npm run test` and get the following results:

![result](https://www.dropbox.com/s/dv87q8t7umywp3s/testresult_cdk.png?raw=1)

How does this work? We have an assertion checking whether our template has resources. The template used for comparison is our [Cloud Formation](https://aws.amazon.com/cloudformation/) template. It is the output of `cdk synth`.

Without changing the generated `CdkTestsStack`, which has no resources defined yet, it looks as follows:

<script src="https://gist.github.com/axelhodler/11cb6112157d5ebcf78c543db5d50489.js"></script>

`Resources` only have the `CDKMetadata` entry. This is about to change.

### Building our stack

Let’s create a private [ECR repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Repositories.html) for our docker image. The image will later be used to run our application with [AWS Fargate](https://aws.amazon.com/fargate/). The image is uploaded to the ECR repository and AWS Fargate will pick it up and run it.

We run `npm i @aws-cdk/aws-ecr` to get the required dependencies and extend the stack.

<script src="https://gist.github.com/axelhodler/fe3c2eb1c757ac0b259518a41606760e.js"></script>

Running `cdk synth` again provides the following output:

<script src="https://gist.github.com/axelhodler/af276c12fabdbcc776c314dbfb1018fd.js"></script>

Now our repository is present in the resources. Our first resource! On to writing the test.

<script src="https://gist.github.com/axelhodler/b72b76d62604b3d3a52ff64b9945db0e.js"></script>

We assert our stack has the `AWS::ECR::Repository` resource with the `RepositoryName` property value `test`. The test passes.

Time to set up our `ApplicationLoadBalancedFargateService`. The construct is part of the `@aws-cdk/aws-ecs-patterns`. We reference our ECR repository in the `taskImageOptions`. Making sure the `latest` image is picked up by Fargate.

<script src="https://gist.github.com/axelhodler/4948573416264ee1919988712dadf382.js"></script>

Aside from the `image` property we did not provide other configuration. Thus the default values are used. We could write tests to be sure which values are used.

This both increases visibility of which values are used and makes sure we would learn about changes to the construct even when updating `aws-ecs-patterns` to a newer version. It allows us to learn if, for example, the `DesiredCount` default would ever change to two instances.

The next test asserts some of the default values.

We should only do this with properties that are important to us.

<script src="https://gist.github.com/axelhodler/7bad33e660f458317f0574a1ca4a6583.js"></script>

The test passes. Let’s try to test if the `ApplicationLoadBalancedFargateService` is using the latest image in our repository.

Here we can use `haveResourceLike` instead of `haveResource`. Else we would need to specify the values for `Essential`, `LogConfiguration`, `Name` and `PortMappings` too to allow for a proper comparison of `ContainerDefinitions`.

<script src="https://gist.github.com/axelhodler/491245ab19f5af10dbb5b27869c572f4.js"></script>

Using the CloudFormation JSON syntax makes the test pretty much unreadable. It has to be copied from the `cdk synth` output and pasted into the test code. It also gives us a glimpse of what writing CloudFormation templates would look like. In the future this cold be moved into a helper function.

### Test driving changes

Say we are building a [Spring Boot](https://spring.io/projects/spring-boot) application. Thus the url to conduct the health check of our services via load balancer is `/actuator/health`. We need a way to configure it. It is a possible property in our `AWS::ElasticLoadBalancingV2::TargetGroup` resource.

<script src="https://gist.github.com/axelhodler/e1be32c625118d59085c9cc1b50afff6.js"></script>

We write the following test

<script src="https://gist.github.com/axelhodler/329b46711c68fa944e12285263901d64.js"></script>

The new test fails. We can fix it by extending our stack and configuring the health check.

<script src="https://gist.github.com/axelhodler/8d4d139d77521019f9a2c6e4712c734b.js"></script>

As a result `cdk synth` provides the new Cloud Formation template

<script src="https://gist.github.com/axelhodler/b079e839aeb17a1143a21c1c47a4b7cb.js"></script>

And all of our four tests pass

![result](https://www.dropbox.com/s/1u48z4b6fzfchz5/cdk_last.png?raw=1)

Sure, the health check will only work if we provide the default Spring Boot port of `8080` to the `containerPort` of `taskImageOptions`. But that is for another time.

### Why would we want to test the CDK code?

There are plenty of projects around that don’t test their CDK code. They might be fully productive without it. The [maintainers of the CDK project ](https://github.com/aws/aws-cdk) have tested the constructs to make sure they work. Thus we won’t have to. Still, there are some benefits to having them under test.

The tests can act as a __sanity check__. Are the constructs doing what I’m expecting them to do? As an __early warning__ system if someone inadvertently removes something important. What if someone removes some critical configuration deemed unnecessary from their perspective? They can be used as __documentation and specification__ for future developers or our own future selves. And of course as part of the __test driven development__ workflow. We might not get as much design feedback on the constructs used from libraries but it will help to structure the way in which we work. Step by step.



The post has been cross-posted to [Medium](https://axelhodler.medium.com/tests-for-aws-cdk-code-9a4bce9fec0e)
