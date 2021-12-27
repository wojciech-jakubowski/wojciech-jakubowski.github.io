---
title: "Cloud Data Platfom architecture"
date: 2021-12-20T21:10:00-02:00
header:
    overlay_image: /assets/images/header/header_image.jpg
    show_overlay_excerpt: false
    overlay_filter: rgba(0, 0, 0, 0.20)
---

In this post I'd like to focus on the internal architecure of a typical cloud data platform - especially on the concept of layers and data flows.  

The previous post discusses some general concepts related to the IT environment surrouding and interacting with the platform. Long-story-short: the platform gathers data from various source systems, processes and serves it to the downstream systems in various different ways. 

Let's look into what actually happens inside.

#### High level architecture

A modern cloud data platfom typically consists of a set of layers that play specific roles, as well as some cross-cutting concerns that span across all the layers.

![Traditional on-prem DW/BI solution](/assets/images/2021-12-20-cdp-arch/high-level.svg)
<p style="text-align: center;">Image 1: High level architecture</p>

Layers:

- **Sources** - represents all potential source systems regardless of the underlying technology (files, SQL DBs, message queues, streams), data ingestion method (batch, stream) or load model (pull or push).

- **Data Ingestion layer** - represents all potential components and technologies that can be used to reliably and efficently move data from the left (sources) to the right (storage layer).

- **Storage and transformation layer** - usually implemented by a data lake and a set of components and technologies that allow conducting massive data transformations (ETL) in efficent manner.

- **Semantic layer** - a place where carefully prepared and modelled data is stored ready for use by end users. Depending on the use case, this might be a data warehouse with accompanying data marts / models, a ML ops environment (ML trainig data sets and pipelines) or any other data-hub-like use case orieted databases or storage in general.

- **Data consumption layer** - represents all potential technologies that can be used to make an acutual use of the data handled by data platform. This typically means BI / reporting tools / dashboards, APIs, outbound data streams and other data destinations (SFTP, email, other storage, custom data exporters).

Cross-cutting concerns:

- **Orchestration** - all potential components responsible for cooridinating all the processes that take place on the platform. So, things like data ingestion (loading data from source systems), data transformation and storage, ML Ops, maintenance tasks, etc are orchestraged from here - which is why it spans through all the layers.

- **Security, Identity & access control** - all the components responsible for securing and controlling acces to the platform. Typically these are things like IAM solutions, secret vaults, virtual networks and all kinds of advanced threat procection tools.

- **Monitoring & Alerting** - reporesents tools and technologies used for monitoring (collecting, storing and analysing logs and metrics) and alerting based on predefined rules.

- **DevOps** - a set of tools responsible for delivering CI/CI, infrastructure as code and automation capabilitiessW.


The typical data flow is:
1. Data is either pulled or pushed from the sources using components from ingestion layer and stored in the storage layer. 
2. The storage layer also consists of a some internal layers sometimes called raw, cleaned, prepared (or bronze, silver, gold [as promoted by Databricks](https://databricks.com/blog/2019/08/14/productionizing-machine-learning-with-delta-lake.html)) - which represent different levels of data quality and reusability (more on that in future). Data is moved and transformed through these internal layers by some ETL/ELT tools. 
3. The data is then loaded into a sematinc layer which is carefully desinged, client facing representation/model of the specific business domains or specific use case oriented solution.

4. Finally, the data is exposed to the end users by components from consumption layer such as BI tools (PowerBI), APIs, Apps, message queues etc. 
 
#### Azure data platform - technology stack 

Data, Analytics & AI is an area where Azure really shines. The vairety of available offerings can be really is immensly rich and also overwhelminig. [PaaS](https://azure.microsoft.com/en-us/overview/what-is-paas/) or PaaS-like resourses usually bring most value as they are easy to use and reliable. On the other hand, [IaaS](https://azure.microsoft.com/en-us/overview/what-is-iaas/) - especailly networking - is a necessary foundation for the secure connectivity and storage.

The following diagram shows how specific Azure technologies map to the layered, high-level architecture discussed above.

![Traditional on-prem DW/BI solution](/assets/images/2021-12-20-cdp-arch/tech-stack.svg)
<p style="text-align: center;">Image 2: Azure Data Platform tech stack</p>

Over the series upcoming of posts I am planning to go through each layer and discuss in detail what is the potential role of these resources, and how can they be used together.

#### Follow up
In the next post, I am going to look into the Lambda & Kappa architecture patterns.
