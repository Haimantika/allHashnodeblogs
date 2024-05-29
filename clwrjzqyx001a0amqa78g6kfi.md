---
title: "Understanding observability and building an observability pipeline"
seoTitle: "Building an Effective Observability Pipeline"
seoDescription: "Learn the basics of observability and how to build an observability pipeline using Fluent Bit for efficient system monitoring"
datePublished: Wed May 29 2024 08:17:28 GMT+0000 (Coordinated Universal Time)
cuid: clwrjzqyx001a0amqa78g6kfi
slug: understanding-observability-and-building-an-observability-pipeline
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716310300392/670a8828-d988-4fae-9b80-efd0a6cdb943.png
tags: devops, observability

---

With complexity of systems growing, Observability is becoming more important. To understand the need, let us take an example of an e-commerce company using microservices. Core functions like user management, product showcase, order processing, and payment are separate services. With services spread across environments, identifying issues like slowdowns, failures, or errors can be challenging. Companies need a way to quickly detect, diagnose, and fix these problems. This is where observability helps.

In this article, we will understand the basics of observability and how we can build an observability pipeline using Fluent Bit.

## What is observability?

Observability helps us understand how a system works by looking at its output. In control theory, it means how well we can figure out the internal state of a system from its external outputs. It helps engineers improve systems based on the data they produce.

Some key components in observability are:

* **Logs-** These are records that describe events that have occurred with the system. Logs provide details of what has happened along with timestamps, errors and other system events.
    
* **Metrics -** Metrics help in measuring various aspects of the system performance and health over time. It includes data such as CPU usage, memory consumption, etc.
    
* **Traces -** Traces track the lifecycle of a request as it moves through various services and components of a system. It helps in understanding the behavior of distributed systems.
    

![](https://www.splunk.com/content/dam/splunk2/images/data-insider/what-is-observability/benefits-of-observability-diagram.svg align="center")

For observability, we need to build a pipeline and in this article we will see how to do it using Fluent Bit.

---

## What is Fluent Bit?

[Fluent Bit](https://fluentbit.io/) is a fast and lightweight Telemetry agent for Logs, Metrics, and Traces for Linux, macOS, Windows, and BSD family operating systems. It allows to collect log events or metrics from different sources, process them and deliver them to different backends such as Fluentd, Elasticsearch, Splunk, DataDog, Kafka, New Relic, Azure services, AWS services, Google services, NATS, InfluxDB or any custom HTTP end-point.

![](https://fluentbit.io/images/overview-new.svg align="center")

Some benefits of using Fluent bit are:

* It can read from local files and network devices, and can scrape metrics in the Prometheus format from the server.
    
* It has built-in reliability, which means if we hit a network or server outage  
    we will be able to resume from where we left off without data loss.
    
* Fluent Bit can send data to a multitude of locations, including popular destinations like Splunk, Elasticsearch, OpenSearch, Kafka, and more.
    

---

## Understanding what observability pipeline is.

Let us go back to our ecommerce example, for days such as `end of season sale`, and `Flipkart's big billion days`, there is a huge spike in traffic, which can lead to performance issues. To tackle this, an observability pipeline is built. It is a system that collects, processes, and analyzes data from various sources, including logs, metrics, and traces, to provide insights into the performance and behavior of a distributed system. It helps the company to monitor their applications in real-time, detect anomalies, and troubleshoot issues faster.

---

## Building an observability pipeline.

Now that we know why an observability pipeline is needed and how it helps. Let us try and build one using Fluent Bit.

1. The first step is to identify what data to collect and determine output targets.
    
    * **Data collection:**  
        **Logs**: System logs, application logs, error logs.
        
        **Metrics**: CPU usage, memory usage, request count, error rate, response times.
        
    * **Output Targets:**
        
        **Monitoring Tools**: Prometheus (for metrics).
        
        **Analytics Platforms**: Elasticsearch (for logs).
        
2. The next step is to install Fluent Bit. Use this command below to install in Linux based systems.
    
    ```bash
    sudo apt-get install td-agent-bit
    ```
    
    There are multiple ways to install Fluent Bit, learn more [here](https://docs.fluentbit.io/manual/installation/getting-started-with-fluent-bit).
    
3. The next step is to configure Fluent Bit for data collection. To collect logs from system and applications, we need to configure inputs in the Fluent Bit configuration file (`fluent-bit.conf`) as shown below:
    
    ```yaml
    [INPUT]
        Name        tail
        Path        /var/log/syslog
        Parser      syslog
    
    [INPUT]
        Name        tail
        Path        /var/log/myapp.log
        Parser      json
    ```
    
    Fluent Bit can also be used to scrape metrics. For that we need to use a tool like [Node Exporter](https://github.com/prometheus/node_exporter), which can be scraped by Prometheus directly.
    
    Here’s how we configure Fluent Bit to collect metrics:
    
    ```yaml
    [INPUT]
        Name        dummy
        Tag         dummy.metrics
        Rate        1
    ```
    
    *Note: For demo purposes, we have used dummy metrics.*
    
4. The next step is to configure parsers and filters.
    
    **Parsers**: Parsers help in interpreting the log formats. For system logs, we might use the built-in `syslog` parser, and for JSON logs, the `json` parser. This helps in structuring the data properly for further processing.
    
    ```yaml
    [PARSER]
        Name        syslog
        Format      regex
        Regex       ^\<(?<pri>[0-9]+)\>(?<time>[^ ]+) (?<host>[^ ]+) (?<ident>[^:]+): (?<message>.+)$
    
    [PARSER]
        Name        json
        Format      json
    ```
    
    **Filters**: Filters add enrichment or transform data. This can update the logs with additional context, making them more useful for analysis.
    
    For example, here's how we can add hostname or modify message content:
    
    ```yaml
    [FILTER]
        Name        modify
        Match       *
        Add         hostname ${HOSTNAME}
    ```
    
5. Now that the input is setup, the next step is to integrate output. Will see how to do it with [Elasticsearch](https://www.elastic.co/) and [Prometheus](https://prometheus.io/).
    
    **InstallingElasticsearch:**
    
    Elasticsearch can be installed on local machine or using a managed service like Amazon Elasticsearch Service.
    
    To install locally, use the command:
    
    ```bash
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.10.1-amd64.deb
    sudo dpkg -i elasticsearch-7.10.1-amd64.deb
    ```
    
    **Elasticsearch setup:**
    
    ```yaml
    [OUTPUT]
        Name            es
        Match           *
        Host            localhost
        Port            9200
        Index           fluentbit
        Type            _doc
        Logstash_Format On
    ```
    
    **Installing Prometheus:**
    
    Prometheus can be [installed using precompiled binaries and docker images](https://prometheus.io/download/) or using [Docker](https://prometheus.io/download/).
    
    **Prometheus setup:**
    
    FluentBit does not natively support outputting metrics to Prometheus, but it can export metrics that Prometheus can scrape:
    
    ```yaml
    [SERVICE]
        HTTP_Server  On
        HTTP_Listen  0.0.0.0
        HTTP_Port    2020
    ```
    
6. Once the above steps are done, the final part is to perform tests. We can do that by injecting test logs and metrics into our inputs and ensuring they appear correctly in Elasticsearch and Prometheus. We can check the outputs in Elasticsearch Kibana and Prometheus dashboard to ensure data integrity and proper indexing.
    

---

## Optimizing the pipeline.

Now that we have learnt how to build an observability pipeline, it is important to know how to optimize performance and secure for maintaining efficiency and integrity.

To improve performance, we can optimize input and output plugin configurations, streamline data parsing, and manage memory and CPU resources through appropriate buffering and batching. To improve security, we can implement encryption for data in transit, and use secure authentication methods.

---

## Conclusion.

In this article, we have learnt all about observability, with real-life examples, use cases and how we can build an observability pipeline for our use case.

Here are some resources to learn more about observability:

* [Fluent Bit documentation](https://docs.fluentbit.io/manual)
    
* [About Observability](https://newrelic.com/blog/best-practices/what-is-observability)
    
* [Prometheus documentation](https://prometheus.io/docs/prometheus/latest/getting_started/)
    
* [Elasticsearch documentation](https://www.elastic.co/guide/en/observability/current/observability-get-started.html)