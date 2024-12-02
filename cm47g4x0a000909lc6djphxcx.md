---
title: "AWS Integration & Messaging - Part 2: Understanding Amazon SNS"
seoTitle: "Understanding Amazon SNS: AWS Messaging Guide"
seoDescription: "Explore Amazon SNS in AWS integration, including how it works, fan-out architecture with SQS, message filtering, and essential SNS features"
datePublished: Mon Dec 02 2024 19:51:45 GMT+0000 (Coordinated Universal Time)
cuid: cm47g4x0a000909lc6djphxcx
slug: amazon-sns
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733169001229/ec502bc9-0714-4f76-88e8-e270a97d1ef5.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1733169083064/5bb15487-f0e9-432b-8c95-b36bc707fb08.jpeg
tags: blogswithcc

---

In this blog, we will move on to the second part of AWS Integration & Messaging, focusing on Amazon SNS (Simple Notification Service). We will explore how SNS works, why and how SQS is used with SNS in a fan-out architecture, as well as SNS Message Filtering and other key points about SNS.

## Amazon SNS

Amazon Simple Notification Service (SNS) is a handy messaging service from AWS. It helps you easily manage and grow your microservices, distributed systems, and serverless apps. With SNS, you can send messages to multiple subscribers, like AWS services, mobile devices, email addresses, and HTTP/S endpoints.

## How Amazon SNS Works?

The working mechanism of Amazon SNS can be broadly categorized into three steps.

* **Create a Topic:**
    
    A topic is a communication channel where publishers send messages. Each topic has a unique name and an Amazon Resource Name (ARN).
    
    Example: `arn:aws:sns:region:account-id:topic-name`
    
* **Add Subscribers:**
    
    Subscribers are entities that want to receive messages from a topic. Some types of subscribers can be, HTTP/HTTPS Endpoints, Lamda, SMS, SQS Queues, Mobile Push Notifications etc.
    
* **Publish Messages:**
    
    Publishers send messages to the topic, and SNS distributes them to all subscribers. Messages can be simple text or JSON data and can be published using the SDK.
    

![](https://images.ctfassets.net/ee3ypdtck0rk/25F39M5q2UZTSeD5xSD568/ac6f635f12c27b7d16e052cd7d9ddc1f/AWS-SNS-overview_2x.png?w=1840&h=1382&q=80&fm=png align="center")

## Integrating SQS with SNS — Fan Out

So far, we have seen a one-to-one connection between publishers and subscribers. But what if we want to send a message once and have it received by all subscribers? In this case, we need to integrate an **SQS Queue** with SNS. This setup is called **Fan-Out Architecture.**

In AWS, **fan-out** is achieved by integrating **Amazon SNS** with **Amazon SQS** or other subscriber endpoints. SNS acts as the broadcaster that sends messages to multiple targets (e.g., queues, email, HTTP endpoints) at the same time. **Fan-Out** is a messaging pattern in which a single message or event is distributed to multiple subscribers simultaneously.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733167011570/f25ceb77-bb76-47a2-b51f-95a0d7fbae1a.png align="center")

* **Buying Service** generates an event or message and publishes it to an **SNS Topic**.
    
* **SNS** same message to multiple **SQS Queues**
    
* Each **SQS Queue** receives a copy of the message independently.
    
* Each queue is connected to a subscriber service like Fraud Service or Shipping Service or any other service.
    
* The subscriber services poll their respective queues, retrieve messages, process them, and then delete them from the queue to avoid reprocessing.
    

### Why SQS is necessary?

* Using **SQS with SNS** in a fan-out architecture is essential for enhancing **reliability, scalability, and decoupling**.
    
* By integrating SQS, messages are stored in the queue until they are successfully processed, ensuring **no loss of data**.
    
* SNS does not need to know or manage the state of the subscriber services.
    
* Allows **Cross-Region Delivery** as it works with SQS Queues in other regions
    
* SQS enables **asynchronous workflows**. That is, publisher doesn’t need to wait for a subscriber to process the message.
    
* Messages in SQS can be stored for a set retention period of up to 14 days. If a subscriber service temporarily fails, the messages stay in the queue and can be processed later.
    

## SNS – Message Filtering

As we have known that SNS broadcasts messages to multiple subscribers. However, there can be situations where subscribers only need a subset of messages from a topic, delivering all messages to every subscriber is unnecessary. **Message Filtering** solves this by allowing subscribers to define **filter policies**. These policies make sure that each subscriber gets only the messages they need, based on specific attributes in the message. This handy feature cuts down on unnecessary processing, saves costs, and makes workflows more efficient.

> JSON policy is used to filter messages sent to SNS topic subscriptions. If a subscription doesn't have a filter policy, it receives every message.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733167676852/5af9e7cf-c31a-4b1c-900e-1d1f4d7e4053.png align="center")

* When a publisher sends a message to an SNS topic, it can include **attributes** (key-value pairs) alongside the main message body.
    
* Each subscriber can define a **filter policy** (JSON data) when subscribing to the SNS topic.
    
* SNS evaluates the filter policy and If the message matches the filter policy, it is delivered to the subscriber; otherwise, it is skipped.
    

## Some Key Points to Know About SNS

* SNS enables you to **push data** i.e. send a single message to multiple subscribers simultaneously
    
* Each SNS topic can support up to **12.5 million subscriptions**
    
* **Data** is **not** **persisted** (lost if not delivered)
    
* SNS operates on the publisher-subscriber **(Pub/Sub)** model
    
* You can create up to **100,000 SNS topics** in your AWS account
    
* SNS is fully managed and automatically scales, so it **doesn't need provision throughput**
    
* Integrates with **SQS for fan-out** architecture pattern
    
* FIFO capability for SQS FIFO
    

![](https://miro.medium.com/v2/resize:fit:1200/0*dJoNK4qUYWUmZ_rY.png align="center")

To summarize the whole Amazon SNS service, the following diagram illustrates the whole process perfectly. Starting from publisher to finally reaching to subscribers, Amazon SNS allows to have a fully managed messaging service to decouple applications.