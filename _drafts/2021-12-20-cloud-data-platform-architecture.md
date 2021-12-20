---
title: "Cloud Data Platfom architecture"
date: 2021-12-20T21:10:00-02:00
header:
    overlay_image: /assets/images/header/header_image.jpg
    show_overlay_excerpt: false
    overlay_filter: rgba(0, 0, 0, 0.20)
---

In this post I'd like to focus on the internal architecure of cloud based data platform - especially on the concept of layers and data flows.  

The previous provides some high level concepts related to the IT environment surrouding and interacting with the platform. In essence, the platform gathers data from various source systems, processes it and serves to the downstream systems in many different ways. 

Let's look into what actually happens inside.

#### High level architecture

The the modern cloud data platfom typically consists of a few layers that play certain roles. There are also some cross-cutting concenrs that span through all the layers. 

![Traditional on-prem DW/BI solution](/assets/images/2021-12-20-cdp-arch/high-level.svg)
<p style="text-align: center;">Image 1: High level architecture</p>

For now lets keep things quite abstract and let's not focus on particular architectures (like lambda or kappa) nor on concrete Azure technologies - no worries we will get there. 

We can see following elements in the diagram above:

Layers

- Sources - represents any potential source system regardeles of the underlying technology (files, SQL DBs, message queues, streams) data ingestion method (batch, stream) or ingestion technologies used

- Data Ingestion Layer - represents all potential components and technologies that can be used reliably and efficently move data from left (sources) to right (storage layer) 

- Storage and tansfomration layer - usually implemented by data lake and a set of components and technologies that allow conducting massive data transformations (ETL) in efficent manner

- Semantic Layer - a place where carefully prepared and modelled data is stored ready for use by end users. Depending on use case this might be a data warehouse with accompanying data marts / models, an ML ops environment (ML trainig data sets and pipelines) or any other use case orieted databases or storage in general.

- Data Consumption level - represents all potential technologies that can be used to make an acutual use of the data handled by data platform. This typically means BI / reporting tools / dashboards, APIs, outbound data streams and other data destinations (SFTP, email, other storage, custom data exporters).

Cross cutting concerns:

- Orchestration - all potential components responsible for orchestrating  
