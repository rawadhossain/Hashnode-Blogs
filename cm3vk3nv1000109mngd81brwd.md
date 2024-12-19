---
title: "AWS Elastic Beanstalk: Exploring its Use Cases, Components, and Deployment Options"
seoTitle: "AWS Elastic Beanstalk features and deployment"
seoDescription: "Discover AWS Elastic Beanstalk's deployment options, components, features, and use cases for streamlined cloud application management"
datePublished: Sun Nov 24 2024 12:09:31 GMT+0000 (Coordinated Universal Time)
cuid: cm3vk3nv1000109mngd81brwd
slug: elastic-beanstalk
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732438727442/78d42821-5f4e-43d1-8ea6-b81734d8d792.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1732450007372/83890127-abed-4064-980b-ff538d028aa3.jpeg
tags: blogswithcc

---

In this blog, we will explore AWS Elastic Beanstalk and how it makes deploying applications in the cloud easier. We will also discuss the components of Beanstalk and the various deployment options available.

## Elastic Beanstalk

When an application is deployed on AWS, a developer must follow several steps. For example, they need to set up resources, choose the right EC2 instance type (size, CPU, memory, etc.), configure the instances, set security groups, install the necessary libraries and dependencies, manually transfer the application files to the EC2 instances, monitor traffic, and manually scale instances up or down. All these tasks can be quite a hassle for a developer. Developers want to manage all these tasks easily. This is where Elastic Beanstalk comes in.

**Elastic Beanstalk** is a service in AWS that simplifies the deployment and management of applications in the cloud. It uses the components and **automatically handles capacity provisioning, load balancing, auto-scaling, and application monitoring** for you, allowing developers to focus on writing code.

### Components of Elastic Beanstalk

* **Application:**
    
    collection of components such as environments, versions, and configuration settings.
    
* **Environment:**
    
    Collection of resources running an application version at a time. Can have multiple environments like development, testing, and production.
    
* **Environment Tier**:
    
    * **Web Server Tier**: Process HTTP requests for web applications.
        
    * **Worker Tier**: For background tasks or long-running processes.
        
* **Configuration Files**:
    
    Includes the files in `.ebextensions/` to customize your environment beyond default settings.
    

### Why is Beanstalk used?

Firstly, it saves time because developers can deploy applications quickly without worrying about the infrastructure.

Developers have the flexibility to automate the process and customize it when needed, making everything much simpler. This way, developers don't have to worry about deployment and can focus on building the application.

## Deployment Options in Beanstalk

Elastic Beanstalk has a bunch of deployment options to give developers flexibility in how updates are rolled out to applications. These different options are used based on different levels of availability, downtime, and risk management.

Some deployment options are explained below:

### All at Once Deployment

It is the **fastest deployment** option in Elastic Beanstalk where the update is rolled out simultaneously to all the instances.

Suppose you have four instances of version 1 (v1) of application. When it is deployed, Elastic Beanstalk stops all of the four instances, then it updates to version 2 (v2) based on whatever changes have been made and then restarts the application.

With this method, all the instances are now running on version v2. Although this approach is fast, it causes **downtime**, making it suitable for quick iterations in a development environment.

![](https://webmobilez.com/wp-content/uploads/2020/04/image-48.png align="center")

Here in stage—1, the instances having the initial version (v1) are stopped and then restarted in Stage —2 having the updated version (v2) of the application across all the instances.

### Rolling Deployment

In this deployment option, application runs below capacity, that is, updates are deployed to a batch of instances at a time which is updated sequentially.

Suppose you have four instances running with version v1, and a batch of two instances is configured. The first batch of two instances is stopped, updated to v2 and restarted. But the other two instances still run on v1. Once the instances with version v2 are updated, the other two instances with version v1 are stopped and updated to v2. Hence, all the four instances are finally running with version v2 of the application.

The image below illustrates the process.

![](https://webmobilez.com/wp-content/uploads/2020/04/2020-04-25__08-55-02-1.png align="center")

> This deployment option has minimum or no downtime with slightly inconsistent behavior during rollout. It is a long deployment and has no additional cost.

### Rolling with Additional Batch

This deployment option is almost similar to Rolling. However, in this option, application runs at capacity. This ensures that the number of operational instances remains constant during the deployment.

Rolling with Additional Batch has the Rolling Deployment strategy but an additional thing it does is, it temporarily creates additional instances. Suppose four instances running a v1 version application, now Beanstalk creates two more instances with version 2 (v2) to ensure the environment has enough capacity to handle traffic. Then the first two original instances of v1 are stopped, updated to v2 and restarts. The process continues for second batch of original instances. Once all instances are updated to version v2, the temporary instances are removed, hence all the original four instances are updated with new version of the application.

![](https://webmobilez.com/wp-content/uploads/2020/04/2020-04-25__09-04-48-1.png align="center")

> This longer deployment keeps the application’s performance unaffected, making it ideal for production with small cost and no downtime.

### Immutable Deployment

Immutable Deployment creates a new set of **instances** instead of modifying the existing ones.

Suppose you application has two instances running version v1. When you initiate an update to version 2 (v2), Beanstalk provisions a new set of 2 instances **(double capacity)** on a temporary ASG running v2 in parallel with the existing v1 instances. When the new updated instances are launched, all traffic from the current ASG switches to temporary ASG. Then the old v1 instances are terminated. By this, if there is any failure, original instances are unharmed.

![](https://webmobilez.com/wp-content/uploads/2020/04/2020-04-25__09-10-32.png align="center")

> Ensures zero downtime and is reliable. Although it is the longest deployment option, the ability to quickly roll back in case of failures makes it suitable for production.

### Blue/Green Deployment

This deployment option is not a direct feature but has the most control and security by suing two separate environments called **blue** and **green**. Sets a new stage having the updated version in which traffic is redirected using Route 53.

Let’s understand it, the **Blue Environment** is running an instance of version v1. Now, Beanstalk creates a new stage, **Green Environment** with version **v2** that mirrors the blue environment but with the updated application.

Now, Route 53 can be setup using weighted policies to redirect a little bit of traffic to the stage environment (green). Once done, Elastic Beanstalk **swaps traffic URL** between the blue and green environments.

![](https://miro.medium.com/v2/resize:fit:1108/1*sFK8chNIkAB0fkoDNHLDeQ.png align="center")

> This deployment strategy is perfect for large-scale, where if any issue arises, it can quickly revert traffic back to the blue environment. Ensures stability with zero downtime and release facility.

### Traffic Splitting Deployment

In this deployment strategy, a new application version is deployed to a temporary ASG with the same capacity.

Initially the application is running version v1 on all instances, maintained by **Main ASG** which handles 100% of the traffic routed by **ALB**. When version v2 is rolled out, Beanstalk creates a **Temporary ASG** that runs the updated version v2 of the application.

Now, the ALB starts routing a small percentage of the incoming traffic (~10%) to the v2 instances in the Temporary ASG while the majority (~90%) to the v1 instances in the Main ASG. During this Beanstalk checks the latency, error or any user feedback. If the v2 instances have any issue detected, the update is automatically rolled back and revert to v1 instances. If not then, new instances are migrated from Temp ASG to Main ASG, and old application version is then terminated.

![](https://miro.medium.com/v2/resize:fit:876/1*1QvWPa4jrwP_OMnbrNsxsg.png align="center")

> Ideal for Canary Testing, where a small group of users can test the new version before a final rollout. No downtime, as the traffic is split between old and updated instances during testing.

**This is the end of this blog.**