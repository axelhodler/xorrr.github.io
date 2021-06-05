---
layout: post
title: "Cross-account SQS access with Spring Boot"
date:   2021-06-04 8:22:21
description: ""
tags: []
---

Or how to get where we want to by following the error messages.

Say our CDK code looks as follows

{% highlight typescript %}
// Queue in our AWS account
const ourQueue = new sqs.Queue(this, `OurQueue`)

// Queue in another AWS account
const externalQueue = sqs.Queue.fromQueueArn(this, 'ExternalQueue',
    'arn:aws:sqs:eu-central-1:123456789101:ExternalQueueName')
{% endhighlight %}

To queues, one in our account one in another account. We need an IAM role allowed to access both queues.

If we are using ECS Fargate the role is the `taskRole` of our task definition.

We create the role. Assumable by ECS Fargate

{% highlight typescript %}
const role = new iam.Role(this, 'BackendServiceRole', {
  roleName: 'BackendServiceRole',
  assumedBy: new ServicePrincipal('ecs-tasks.amazonaws.com')
})
{% endhighlight %}

Then allow the role to consume both queues

{% highlight typescript %}
ourQueue.grantConsumeMessages(role)

externalQueue.grantConsumeMessages(role)
{% endhighlight %}

For `OurQueue` we either configure the queue to allow the IAM role to access __or__ we allow the role to access the queue. One side is enough.

As for `ExternalQueue`, using cross-account access, our IAM role must have the rights to access the queue __and__ the queue allows our role to access it.

The names of the queues need to be provided to our service. In Fargate we use the `environment` properties of our `taskImageOptions` to provide them as environment variables.

```
environment: {
  QUEUE_NAME: queue.queueName,
  EXTERNALQUEUE_NAME: externalQueue.queueName
}
```

### Spring Boot Setup

To our dependencies we add

```
implementation("org.springframework.cloud:spring-cloud-starter-aws-messaging")
```

We can also use [start.spring.io](https://start.spring.io/). We add the SQS dependency to get the desired dependency structure for our specific project layout. Whether we use Maven or Gradle and Java, Kotlin or Groovy.

The following is the setup in the code.

{% highlight kotlin %}
@Component
class OurQueueListener {
  private val logger = LoggerFactory.getLogger(javaClass)

  @SqsListener("\${ourqueue.name}")
  fun readMessage(message: String) {
    logger.info(message)
  }

}
{% endhighlight %}

We might want to parse the message and provide it to a service instead of only logging it. Out of scope for this article.

{% highlight kotlin %}
@Component
class ExternalQueueListener {
  private val logger = LoggerFactory.getLogger(javaClass)

  @SqsListener("\${externalqueue.name}")
  fun readMessage(message: String) {
    logger.info(message)
  }

}
{% endhighlight %}

The listener for `OurQueue` will work right away if configured properly.

`ExternalQueue` will yield the following

```
2021-06-02 11:07:35.968  WARN 1 --- [main] s.c.a.m.l.SimpleMessageListenerContainer : Ignoring queue with name 'ExternalQueueName': The queue does not exist.; nested exception is com.amazonaws.services.sqs.model.QueueDoesNotExistException: The specified queue does not exist for this wsdl version. (Service: AmazonSQS; Status Code: 400; Error Code: AWS.SimpleQueueService.NonExistentQueue; Request ID: 34a08692-1234-4567-854a-dfa3c5f7f945; Proxy: null)
```

Ok... The queue does not exist?

Did the other team give us the wrong ARN of their queue?
Did it matter what we have provided to the `Queue` construct as `id`?
Does it need to be the same as the queue owner has used?
Did they forget to configure the queue to allow access to our role?
Are we not properly trying to access the queue?
wsdl version? Is there something we need to change in our implementation?

If we read the docs for `@SqsListener` we learn the passed `value` can be the physical name or the URL of the queue.

As soon as we switch to the URL it works

{% highlight kotlin %}
@SqsListener("\${externalqueue.url}")
{% endhighlight %}

We make sure to change the `environment` variables accordingly

```
environment: {
  QUEUE_NAME: queue.queueName,
  EXTERNALQUEUE_URL: externalQueue.queueUrl
}
```

We could use the `queueUrl` for both values to stay consistent.

The above is a showcase on the importance of error messages.

If the other side would not have allowed us the access we would get a different error message.

```
Caused by: com.amazonaws.services.sqs.model.AmazonSQSException: Access to the resource https://sqs.eu-central-1.amazonaws.com/123456789100/ExternalQueueName is denied. (Service: AmazonSQS; Status Code: 403; Error Code: AccessDenied; Request ID: 1245c0fe-1234-5678-8ec9-99cd22cf3576; Proxy: null)
```

### Adding server-side encryption

Maybe after changing it to using the SQS URL the setup still does not work. The log states

```
com.amazonaws.services.sqs.model.AmazonSQSException: The ciphertext refers to a customer master key that does not exist, does not exist in this region, or you are not allowed to access. (Service: AWSKMS; Status Code: 400; Error Code: AccessDeniedException; Request ID: 2a85873f-1234-5678-8ede-713ce5ea3a4a; Proxy: null) (Service: AmazonSQS; Status Code: 400; Error Code: KMS.AccessDeniedException; Request ID: 649c383c-1234-5678-be6e-610d7b5632b1; Proxy: null)
```

This means the queue was configured to use server-side encryption. For that, a customer master key (CMK) is used. To allow someone to decrypt the messages policy of the key needs to be adapted. We cannot change the key policies of AWS managed keys. The other account will have something like the following in their CDK code

{% highlight typescript %}
const sqsEncryptionMasterKey = new kms.Key(this, 'QueueKey')

const consumerRole = new iam.ArnPrincipal("arn:aws:iam::123456789101:role/BackendServiceRole");
sqsEncryptionMasterKey.grantEncryptDecrypt(consumerRole)

const encryptedQueue = new sqs.Queue(this, 'EncryptedQueue', {
  encryption: QueueEncryption.KMS,
  encryptionMasterKey: sqsEncryptionMasterKey
});

encryptedQueue.grantConsumeMessages(consumerRole)
{% endhighlight %}

They already have our iamRole ARN. The role needs both the rights to read from the SQS queue and to decrypt the messages.

On our side we need the ARN of the CMK to extend our CDK code. The policies need to be applied to both sides.

{% highlight typescript %}
const externalKey  = kms.Key.fromKeyArn(this, 'ExternalKey', 'arn:aws:kms:eu-central-1:123456789085:key/7a0d16dc-36c8-4cf9-a493-81bb2d772d74')
externalKey.grantDecrypt(role)
{% endhighlight %}

On the Spring Boot side, the code stays the same. Encryption and decryption will happen in the background.

### Summary

The external queue owner needs to

1. Allow our role to read from the queue
2. _With encryption_: Allow our role to access the key

We need to

1. Allow our role to read from the queue
2. _With encryption_: Allow our role to access the key

If any of the above is missing we will get the `AccessDenied` exception. It will be visible in the log during the startup since the `SqsListener` starts listening right away.

And don't forget to use the `queueUrl` instead of the queue for any queues not in our account.
