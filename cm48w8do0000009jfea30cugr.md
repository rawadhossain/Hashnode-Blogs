---
title: "AWS Integration & Messaging - Part 3: Amazon Kinesis"
datePublished: Tue Dec 03 2024 20:10:07 GMT+0000 (Coordinated Universal Time)
cuid: cm48w8do0000009jfea30cugr
slug: amazon-kinesis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733169321471/f59148a4-23f6-48e7-b5d1-476a9d782cc4.jpeg

---

## Amazon Kinesis

Amazon Kinesis is a handy service from AWS that lets developers gather, process, and analyze data in real-time. It's built to manage data from all sorts of sources, helping businesses make quick decisions or create real-time apps. Kinesis is perfect for things like real-time analytics, analyzing logs and events, and creating live dashboards.

### Some Core Components of Amazon Kinesis

* **Kinesis Data Streams:**
    
    Real-time streaming data service that captures, process, and store data streams.
    
* **Kinesis Data Firehose:**
    
    Loads streaming data into destinations like Amazon AWS data stores. No need to write custom applications for processing or loading data.
    
* **Kinesis Data Analytics:**
    
    Enables running SQL queries or Apache Flink on streaming data in real-time.
    
* **Kinesis Video Streams:**
    
    capture, process, and store video streams for streaming video. Useful for real-time video analytics or archiving video for later playback and processing.
    

## Understanding Kinesis Data Streams Workflow

As we've seen in previous articles, **producers** are the sources of data, such as applications, clients, SDK/KPL, and the Kinesis Agent. They send data to a Kinesis Data Stream.

A data stream is made up of several **Shards**. Each shard represents a portion of the stream's throughput capacity. It can handle **1 MB/sec** for writes, **2 MB/sec** for reads, and support up to **1,000 records/second** for ingestion. The producer assigns a **partition key** to each record, which decides which shard the record is written to. Records with the same partition key are sent to the same shard.

Each **record** in the stream contains, Partition Key, Data Blob, Sequence Number. **Consumers** like **Apps, Lamda, Data Firebase, Data Analytics** reads and process data from the stream in real-time.

By using this, we get handle traffic according to need, like

* Increase shards for higher throughput.
    
* Decrease shards to reduce costs during low traffic.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733255764041/9456270a-d918-4b32-8e56-de576377fd31.png align="center")

## Kinesis Client Library (KCL)

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