---
title: "Amazon CloudFront: The Key to Faster Web Content Delivery"
seoTitle: "Accelerate Web Content with Amazon CloudFront"
seoDescription: "Amazon CloudFront accelerates web content with caching at edge locations for faster global access, involving origins and caching processes"
datePublished: Sat Nov 23 2024 15:02:04 GMT+0000 (Coordinated Universal Time)
cuid: cm3uatppd000i09ld8j3kcgxm
slug: cloudfront
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732374067397/a731b164-1a93-427d-a602-e5a1ae624a55.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1732374094836/e46f0c1d-9e25-450a-80b7-dae054de28f2.jpeg
tags: blogwithcc

---

In this blog, we will explore Amazon CloudFront in AWS. We'll learn how CloudFront works, what the caching process is, and understand its key components. We'll also look at cache policies in CloudFront and discuss some use cases and best practices for these policies.

## Amazon CloudFront

Let's understand by a relatable example, suppose you want to use a streaming platform like Netflix. Let’s say the main server is somewhere in the US and the viewers from Asia, Europe and Australia want to watch the content. Now, if every viewer has to stream the contents directly from US, they will have to experience some suffering and delays. But in reality, do we actually face any buffer while streaming? — No, because the cloud architecture has been designed in such a way that it passes all the edge cases of failure.

To address these issues, AWS offers a service called **Amazon CloudFront**. It creates copies of video content and stores them at **edge locations** around the world. This allows users to stream content quickly from a nearby location.

In simple terms, it is a **Content Delivery Network (CDN)** that stores copies of your website's content in multiple locations worldwide. This allows users to access it faster, regardless their location.

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20220306134139/Group-111.jpg align="center")

Some terms to know for understand CloudFront:

### Origin Server

The website's digital assets, such as files, images, and videos, are stored on the origin server, which serves as the primary content source. It is like a root repository for the website's content. The origin server can be an Amazon S3, EC2, or a custom HTTP server.

### Edge Locations

Edge locations are the data centers to cache or store the content. Whenever any request is sent by user, CloudFront delivers it from nearest Edge locations instead of origin server.

## How CloudFormation Works

First, a structure or blueprint called a **Template** is created, written in either **JSON** or **YAML** format. This template outlines the infrastructure (like EC2 instances, S3 buckets, and databases) and their configurations. AWS CloudFormation uses these templates to automate the creation and management of AWS resources. The process begins by uploading the template to an Amazon S3 bucket. Once uploaded, AWS CloudFormation references the template and initiates a **stack**. A stack is a collection of resources defined in the template. CloudFormation then processes the template to set up and **configure resources**, such as EC2 instances, S3 buckets, or databases. This method ensures consistent and efficient deployment and also allows for updates or rollbacks as needed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734012960321/7b0052bb-c5a1-4702-ba10-b09c5b65ed34.png align="center")

## Understanding Caching Process in CloudFront

**Caching** in CloudFront is a process of storing copies of content at different **Edge Locations** to make it faster for the users to access. What CloudFront does is that it determines whether it can respond to the users request from its cache or it needs to fetch it from the origin server.

![](https://kloud-blogwpsite-ause-prd-web.azurewebsites.net/wp-content/uploads/2016/11/drawing11.png align="center")

Here's how the whole process works, for better understanding an image is given above.

* **Step—1:** Client sends requests, which is routed to nearest Edge location based on user's location using CloudFront’s global network.
    
* **Step—2:** CloudFront checks if the requested file is already cached at the Edge Location. CloudFront identifies each object in the cache using the **Cache Key**. If it is cached, the client's request is served immediately. If not, CloudFront forwards the request to the **Origin Server** to fetch the file.
    
* **Step—3:** When the requested content is not found, which is called a **Cache Miss**, CloudFront retrieves the content file from the origin server. Once the file is retrieved, it is cached at the Edge Location.
    
* **Step—4:** Therefore, when a second request is made for that file, it results in a **Cache Hit**. This means the file can be served directly from the Edge Location until it expires or is invalidated, allowing for faster access with low latency.
    

## Cache Key

A **cache key** is a unique identifier that CloudFront uses to determine if a cached version of the requested content is available. If the cache key matches an existing cache entry, CloudFront serves the cached content. If it doesn't match, CloudFront retrieves the content from the origin server.

The cache key includes:

* **URL path**: Example: `/images/cover.jpg`
    
* **Query strings**: Example: `/images/cover.jpg?size=large`
    
* **Headers**: Example: `Accept-Language: en-US`
    
* **Cookies**: Example: `user_session=aeff778`
    

### Cache Behavior

Let’s understand some cache behavior in real life aspect. For example, how it works for singing in a web page.

**Cache Behaviors – Sign in Page**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732372407090/a5762ec8-a155-4782-afac-d11159d082cb.png align="center")

For a sign in feature in website, when the users need to authenticate, they navigate to the `/login` endpoint. Then all user requests are routed to CloudFront, which checks its cache behaviors to determine how to handle the request:

* For paths `/login`, a **custom cache behavior** and routes the request to the EC2 instance.
    
* For other paths like `/*`, it applies the **default cache behavior** and tries to fetch content from the S3 bucket.
    

To understand further, when a user hits `/login` endpoint. The request is forwarded by CloudFront to the **EC2 instance** where the user authenticates by generating **signed cookies** upon successful authentication. Signed cookies hold the encrypted data of the user and allows secure access to the resources.

After authentication, users may request files upon their requirement. Here, CloudFront checks if the content is **cached** at the nearest Edge Location. If cached, content is delivered immediately to the user. If not cached, it forwards the request to the **S3 bucket** to retrieve the file.

## CloudFront Policies – Cache Policy

Amazon CloudFront's **Cache Policy** lets you control how CloudFront caches content. It sets the rules for deciding if a cached object should be reused for a request or if the content should be fetched from the origin.

This caching is based on parts of the HTTP request, such as **headers**, **query strings**, and **cookies**, which CloudFront uses as cache keys.

### Key Components of a CloudFront Cache Policy:

* **Cache Key**: Combination of HTTP request attributes used to uniquely identify a cached object.
    
* **TTL (Time-to-Live):** It determines how long objects are cached before they expire. Can set Minimum, Maximum or Default TTL time for an object to cache.
    
* **Origin Forwarding:** Specifies which headers, query strings, or cookies are forwarded to the origin when a cache miss occurs.
    

## Use Cases for Cache Policies

1. **Static Content Delivery:**
    
    When it comes to delivering static assets—like images, CSS, or JavaScript files. These files don’t change frequently, so there’s no need to complicate the caching process by considering headers, query strings, or cookies.
    
    Setting up a **cache policy** ignores all additional request attributes, ensuring that CloudFront caches your static files as-is.
    
2. **Personalized Content:**
    
    Personalization is crucial in today's digital world. From receiving recommendations to having customized dashboards, modern users expect content that feels unique to them. With CloudFront Cache Policies, you can cache personalized responses by including query strings, cookies, or headers in the cache key.
    
3. **Geolocation-Based Caching:**
    
    Imagine visiting a website and seeing content tailored to your country or region without delays. With CloudFront, this is not only possible but also incredibly efficient. By including headers like `CloudFront-Viewer-Country` in the cache key, you can cache content based on the user’s geographic location.
    
4. **API Responses:**
    
    APIs are the backbone of modern applications, but frequent requests to the origin can cause delays. CloudFront Cache Policies allow you to cache specific API responses by including query strings or headers in the cache key. This is particularly helpful for APIs that return data that doesn't change often.
    

### **Best Practices for Cache Policies**

1. **Minimize Forwarded Attributes:**
    
    * Only forward request attributes that are absolutely necessary for your application.
        
    * Avoid forwarding sensitive or user-specific cookies unless personalization is required.
        
2. **Set Appropriate TTLs:**
    
    * Use **long TTLs** for static assets like images, CSS, and JavaScript files that rarely change.
        
    * Use **short TTLs** for dynamic or frequently updated content like news feeds or API responses.
        
    * Set a **default TTL** for situations where your origin doesn’t specify caching headers.
        
3. **Monitor Cache Metrics:**
    
    * Track the **cache hit ratio** to measure how often content is served from the cache versus the origin.
        
    * Identify low-performing behaviors and modify cache policies to improve efficiency.
        
    * Use **real-time logs** to pinpoint attributes causing excessive cache misses.
        
4. **Combine Cache and Origin Request Policies:**
    
    * Use Cache Policies to optimize caching (e.g., define cache keys, TTLs).
        
    * Use Origin Request Policies to limit what attributes (e.g., headers, cookies) are sent to the origin.
        
    * Use **Cache Policies** alongside **Origin Request Policies** to separately manage caching and forwarding behavior.
        

By using **Cache Policies**, CloudFront allows you to precisely control how your content is cached, optimizing both performance and cost for your web applications.