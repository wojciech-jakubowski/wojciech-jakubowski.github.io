---
title: "Cloud Data Platfom part 2: architecture!"
date: 2022-01-08T21:10:00-02:00
header:
    overlay_image: /assets/images/header/header_image.jpg
    show_overlay_excerpt: false
    overlay_filter: rgba(0, 0, 0, 0.20)
---

As discussed previously, the main purpose of a modern, cloud-based data platform is to gather data from various source systems, process and serve it to the downstream consumers. 

Let's now look into what happens inside and revisit some of the common architecture patterns for modern, data platforms - especially the concept of layers and data flows.

#### High level architecture

A data platform typically consists of a set of layers that play specific roles in data processing, as well as some cross-cutting concerns that span across all the layers.

![High level architecture](/assets/images/cdp-arch/high-level.svg)
<p style="text-align: center;">Image 1: High level architecture</p>

Typically, the layers are:

- *Sources* - all potential source systems regardless of the underlying technology (files, SQL DBs, message queues, streams), data ingestion method (batch or stream), or load model (pull or push).

- *Ingestion layer* - represents all potential components and technologies that can be used to reliably and efficiently move data from the left (sources) to the right (storage layer).

- *Storage and transformation layer* - usually implemented by a data lake and a set of components and technologies that can do massive data transformations (ETL) efficently.

- *Semantic layer* - a place where carefully prepared and modeled data is stored, ready to use by the end-users. Depending on the use case, this might be a data warehouse with accompanying data marts/models, a ML Ops environment (ML training data sets and pipelines), or any other data-hub-like use-case oriented databases or storage in general.

- *Consumption layer* - also called *serving layer*, represents all potential technologies that can be used to make actual use of the data. This typically means BI/reporting tools, dashboards, APIs, outbound data streams or message queues, and other data destinations (SFTP, email, 3rd party storage, custom data exporters, etc).

- *Consumers* - all potential users of the curated prepared data as well as destination systems (internal and external).

Cross-cutting concerns:

- *Orchestration* - all potential components responsible for scheduling and coordinating all the processes that take place on the platform. So, things like data ingestion (loading data from source systems), data transformation and storage, ML Ops, maintenance tasks, etc are orchestrated from here - which is why it spans through all the layers.

- *Security, Identity & access control* - all the components responsible for securing and controlling access to the platform. Typically these are things like IAM solutions, secret vaults, virtual networks, and all kinds of advanced threat protection tools.

- *Monitoring & Alerting* - represents tools and technologies used for monitoring (collecting, storing, and analyzing logs and metrics) and alerting based on predefined rules and conditions.

- *DevOps* - tools and processes responsible for delivering CI/CI, infrastructure as code, and automation capabilities.


#### Data pipelines

Usually, data is moved between layers, changing its representation (formats), quality, structure (schema, joins), quantity (filters, aggregations), etc. An example of such process can be:

1. Data is ingested from the source system by some components from the ingestion layer and stored in the storage layer. There are multiple ways this can be done: data can be either pulled or pushed in batches, or processed as an infinite, continuous stream of messages.

2. The storage layer is usually comprised of internal layers sometimes called raw, cleaned, prepared (or bronze, silver, gold [as promoted by Databricks](https://databricks.com/blog/2019/08/14/productionizing-machine-learning-with-delta-lake.html)). Those layers represent different levels of data quality and reusability (more on that in the future). Data is moved and transformed through these internal layers by some ETL/ELT data processing tools. 

3. The data is then loaded into a semantic layer which is carefully designed, client-facing representation (model) of the specific business domains or a use-case-oriented data solution.

4. Finally, the data is exposed to end-users by components from the consumption layer such as BI tools (PowerBI), APIs, apps, message queues, etc.

A process that implements the flow described above (steps 1-3) is called a *data pipeline*. It can be implemented using many different technologies, it is usually a combination of an orchestrator and ETL tool. 

#### Processing types

Different use cases may have different requirements regarding the amount & frequency of data ingested and accepted latency for getting results.

##### Batch

There are cases when it is ok to pull (or receive) the big portion of data from the source system, process it overnight, and prepare for the next morning reporting/consumption. This is called *batch processing*. 

![Batch processing](/assets/images/cdp-arch/batch.svg)
<p style="text-align: center;">Image 2: Batch processing</p>

The orchestrator plays the main role here. It triggers the load process based on some defined schedule and sequentially orchestrates ETL components to do their tasks (extraction, transformation, load). Data is usually processed in bigger chunks (hundreds of megabytes to terabytes) based on a defined schedule. 

This is simple and efficient, but there is always some lag in results between data loads. The cost is also relatively low because data processing components (which are usually the most expensive) only run during the data load and remain inactive the rest of the time.

##### Stream

But there are also cases where we have data incoming all the time as a stream of events and end-users want to get processing results as soon as possible. This is especially evident in IoT scenarios where devices are generating massive amounts of data each hour. These results need to be constantly processed and shared with consumers quickly (for instance for failure or anomaly detection). This approach is called *stream processing*.

![Stream processing](/assets/images/cdp-arch/stream.svg)
<p style="text-align: center;">Image 3: Stream processing</p>

In this approach, the data continuously and automatically flows through all the layers of the platform as an infinite stream of small events. The results of processing are almost immediately available.

This approach also has some drawbacks: 
- Not all data sources can emit data as a stream of events.
- There are challenges with storing millions (or billions) of small files as well as handling updates in the data lake efficiently, as HDFS storage systems mostly assume data immutability (although this is now solved by [delta lake](https://delta.io/) - more on that later).
 - It requires streaming processors to run all the time, which is usually expensive.

##### Lambda architecture

To accommodate these two approaches, an architecture pattern called [Lambda](https://databricks.com/glossary/lambda-architecture) is commonly used. It combines both processing modes in one solution by splitting data processing into two paths: cold - for batch processing and hot (also called hot) - for stream processing. 

![Lambda architecture](/assets/images/cdp-arch/lambda.svg)
<p style="text-align: center;">Image 4: Lambda architecture</p>

This pattern is robust but comes with a certain complexity as you need to maintain and operate both paths, usually with two different codebases.

##### Kappa architecture

An attempt to get rid of this duality is called Kappa architecture. In this pattern, the assumption is that all data is ingested, processed, and indefinitely stored as a data stream inside a streaming platform like Apache Kafka. Consumers can subscribe to these streams on demand. To get some historic information, all events need to be "replayed" from the streaming system. There is no data lake for long-term storage.

![Kappa architecture](/assets/images/cdp-arch/kappa.svg)
<p style="text-align: center;">Image 5: Kappa architecture</p>
 
##### Delta architecture

Delta architecture can be considered an mix of Kappa with a data lake long term storage. It become possible with introduction of delta technology by Databricks/Apache Spark. Instead of storing data inside streaming system (which is not really handy and optimized for long term storage), the data is stored inside delta tables inside data lake. Delta tables also support streaming, updates and ACID transactions.

![Lambda architecture](/assets/images/cdp-arch/delta.svg)
<p style="text-align: center;">Image 6: Delta architecture</p>

The limitation of this architecture can be only used for platforms that use Apache Spark since Delta writes are currently only possible with this technology.

In real life, you rarely end up with *extact* implementation of one of those architectures. Almost always there are some modifications to match the actual needs and objectives of the final design.

#### Azure Data Platform

Data, Analytics & AI tech stack is where Azure really shines. The variety of available offerings is very rich... and sometimes overwhelming. [PaaS](https://azure.microsoft.com/en-us/overview/what-is-paas/) or PaaS-like resources usually bring the most value as they are easy to use and reliable. On the other hand, [IaaS](https://azure.microsoft.com/en-us/overview/what-is-iaas/) - especially networking - is a necessary foundation for secure connectivity and storage.

The following diagram shows how the abstract architecture described above translates into Azure technology stack.

![Azure Tech stack](/assets/images/cdp-arch/tech-stack.svg)
<p style="text-align: center;">Image 7: Azure Data Platform tech stack</p>

#### Follow up
In the next few posts, I will go through each of these layers and discuss in detail when and how those Azure resources can be used.
