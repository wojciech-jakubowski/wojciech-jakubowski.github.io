---
title: "Cloud Data Platfom architecture"
date: 2021-12-20T21:10:00-02:00
header:
    overlay_image: /assets/images/header/header_image.jpg
    show_overlay_excerpt: false
    overlay_filter: rgba(0, 0, 0, 0.20)
---

As discussed in the previous post, the purpose of the modern, cloud-based data platform is to gather data from various source systems, process and serve it to the downstream systems in multiple ways. 

Let's now look into what happens inside and revisit some of the common architecture patterns for modern, cloud-based data platforms - especially the concept of layers and data flows.

#### Architecture

A data platform usually consists of a set of layers that play specific roles, as well as some cross-cutting concerns that span across all the layers.

![Traditional on-prem DW/BI solution](/assets/images/2021-12-20-cdp-arch/high-level.svg)
<p style="text-align: center;">Image 1: High level architecture</p>

Typically, the layers are:

- **Sources** - all potential source systems regardless of the underlying technology (files, SQL DBs, message queues, streams), data ingestion method (batch, stream), or load model (pull or push).

- **Data Ingestion layer** - represents all potential components and technologies that can be used to reliably and efficiently move data from the left (sources) to the right (storage layer).

- **Storage and transformation layer** - usually implemented by a data lake and a set of components and technologies that allow conducting massive data transformations (ETL) in an efficient manner.

- **Semantic layer** - a place where carefully prepared and modeled data is stored, ready to use by end-users. Depending on the use case, this might be a data warehouse with accompanying data marts/models, a ML Ops environment (ML training data sets and pipelines), or any other data-hub-like use-case oriented databases or storage in general.

- **Data consumption layer** - represents all potential technologies that can be used to make actual use of the data handled by the data platform. This typically means BI/reporting tools and dashboards, APIs, outbound data streams, and other data destinations (SFTP, email, other storage, custom data exporters).

- **Consumers** - all potential users of the curated prepared data as well as destination systems (internal and external)..

Cross-cutting concerns:

- **Orchestration** - all potential components responsible for coordinating all the processes that take place on the platform. So, things like data ingestion (loading data from source systems), data transformation and storage, ML Ops, maintenance tasks, etc are orchestrated from here - which is why it spans through all the layers.

- **Security, Identity & access control** - all the components responsible for securing and controlling access to the platform. Typically these are things like IAM solutions, secret vaults, virtual networks, and all kinds of advanced threat protection tools.

- **Monitoring & Alerting** - represents tools and technologies used for monitoring (collecting, storing, and analyzing logs and metrics) and alerting based on predefined rules and conditions.

- **DevOps** - tools responsible for delivering CI/CI, infrastructure as code, and automation capabilities.


#### Data flow

Usually, data is moved between the layers, changing its representation (formats), quality, structure (schema, joins, aggregations), etc as follows:

1. Data is ingested from the sources by components from the ingestion layer and then stored in the storage layer. There are multiple ways this can happen: data can be either pulled or pushed in batches, or processed as an infinite, continuous stream of messages.

2. The storage layer is usually comprised of internal layers sometimes called raw, cleaned, prepared (or bronze, silver, gold [as promoted by Databricks](https://databricks.com/blog/2019/08/14/productionizing-machine-learning-with-delta-lake.html)). Those layers represent different levels of data quality and reusability (more on that in future). Data is moved and transformed through these internal layers by some ETL/ELT tools. 

3. The data is then loaded into a semantic layer which is carefully designed, client-facing representation/model of the specific business domains or a use-case-oriented data solution.

4. Finally, the data is exposed to end-users by components from the consumption layer such as BI tools (PowerBI), APIs, apps, message queues, etc.

#### Lambda & Kappa

It is also worth mentioning two popular architecture styles for data platforms: lambda & kappa.  
 
#### Azure 

Data, Analytics & AI tech stack is where Azure really shines. The variety of available offerings is very rich... and sometimes overwhelming. [PaaS](https://azure.microsoft.com/en-us/overview/what-is-paas/) or PaaS-like resources usually bring the most value as they are easy to use and reliable. On the other hand, [IaaS](https://azure.microsoft.com/en-us/overview/what-is-iaas/) - especially networking - is a necessary foundation for secure connectivity and storage.

The following diagram shows how the abstract architecture described above translates into Azure technology stack.

![Traditional on-prem DW/BI solution](/assets/images/2021-12-20-cdp-arch/tech-stack.svg)
<p style="text-align: center;">Image 2: Azure Data Platform tech stack</p>

#### Follow up
In the next few posts I will go through each of these layers and discuss in detail how and when these Azure resources can be used.
