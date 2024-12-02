---
title: "AWS Integration & Messaging - Part 3: Amazon Kinesis"
slug: aws-integration-messaging-part-3-amazon-kinesis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733169321471/f59148a4-23f6-48e7-b5d1-476a9d782cc4.jpeg

---

## **Some Key Points About Kinesis**

* **Standard pull data**: Data consumers poll (pull) records from shards at a maximum rate of **2 MB per shard per second**.
    
* Enhanced fan-out, Kinesis pushes data records directly to each consumer at **2 MB per shard per second**, per consumer, reducing latency.
    
* Possibility to replay data, allowing consumers to reprocess data.
    
* Meant for real-time big data, analytics, and ETL
    
* Data within a shard is processed in the order it was written
    
* Data expires after X days (maximum retention being 365 days)
    
* Provisioned mode or on-demand capacity mode:
    
    * **Provisioned mode**: Specify the number of shards to handle predictable workloads.
        
    * **On-demand mode**: Automatically scales based on traffic, suitable for unpredictable workloads.