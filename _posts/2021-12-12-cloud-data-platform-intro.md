---
title: "Cloud Data Platfom - part 1 - overview!"
date: 2021-08-22T21:10:00-02:00
categories:
  - blog
---

### Cloud Data Platform - Intro

Cloud-based data platfoms are getting increasingly popular and lots of companies are investing in this area these days.

1. Some companies are migrating their existing legacy data warehouse / BI solutions to the cloud as a part of their "digital transformation" efforts.
2. Some companies start new initiatives in the area of data analytics.
3. Some companies are embracing advanced analytics, data science & ML.

Modern cloud based data platforms can address all of these needs and can bring tremendous value to the business. 

### Idea

In this series of blog posts I'd like to reflect on some aspects of a modern data platfom solution. I am not planning to disucuss business or organizational matters. Instead, I want to cover following architectural and technical topics:
1. Context (enterprise IT landscape, use cases)
2. Data platform architecture (layers)
3. Data ingestion layer
4. Storage and processing layer
5. Consuption layer
6. Underlying cloud infrastructure (technologies, resources, networking, security)
7. Observability, reconciliation, monitoring, alerting
8. DevOps practices (Infra as code, CI/CD)
9. Many many more ... :)

Given my background, I'll focus mostly on Microsoft Azure stack, but most architectures, patterns & practcies apply to other cloud platforms as well.

### Context

A typical enterprise has a number of IT systems covering various aspects of the business (sales, accounting, manufacturing, supply chain etc.). Depending on company size it can be either a few, tens or even hundreds. Some of these systems might be homegrown (ususally with the help of external IT vendors), some might be taken "off-the-shelf" (ERP, CRM, Office, etc.) and some of them might be legacy after mergers / acquisitions / reorgs. Moreover, some of them might run on premise and some of them might live in the cloud. 

In order to get some holistic view on the overall state of the enterprise and levarage synergies, companies usually implemented enterprise data warehouses that brough the data from all/most of these systems together, integrated it and presented for BI / reporting. 

![Traditional on-prem DW/BI solution](/images/2021-12-12-cdp-intro/traditional-bi.svg)
<p style="text-align: center;">Image 1: Traditional on-prem DW / BI solution</p>

Modern cloud-based data platforms do pretty much the same (integrating data from various sources and delivering value from that data), but the scope and use cases are usually much broader and capabities bigger. Apart from typical DW / BI workloads, they also enable other scenarios such as AI (data sciance & ML) or [data hub](https://towardsdatascience.com/what-is-a-data-hub-41d2ac34c270).

The central part of the data platform is usually a [data lake](https://databricks.com/discover/data-lakes/introduction), which acts a primary storage location for various representations of the enterprise data and a foundation for particular use cases. 

The high level overview of this concept is presented on a diagram below:

![Modern cloud based data platform](/images/2021-12-12-cdp-intro/use-cases.svg)
<p style="text-align: center;">Image 2: Modern cloud-based data platform</p>

Of course the aforementioned scenarios were and still are also doable in traditional on-prem setup. But now its much easier to build and run them in the cloud thanks to its elasticity, scalability and rich ecosystem of managed tools and resouces. You don't need heavy investments in on-prem infrastructure and skills to run it - the cloud vendor (usually) does that for you!

### Use cases

Depending on business needs, a typical data platform can focus on one primary use case or can bring a few of them together. It is quite likely to have a data warehouse workload and some AI/ML elements, or a data hub that has some also partially consists of a data warehouse (among other components) and has some embedded AI features. 


#### Analytics

Apparently, data warehousing [is still a thing](https://www.linkedin.com/pulse/data-lake-architecture-bill-inmon/). However, since we live in the cloud era, it often makes sense to migrate a legacy DW as a part of some digital transformation or cloud migration endevour. Or build a new one from scratch using modern, cloud native technologies such as Snowflake or Azure Synapse Analytics.

![Cloud based data analytics solution](/images/2021-12-12-cdp-intro/analytics.svg)
<p style="text-align: center;">Image 3: Cloud-based data analytics solution</p>

In such solution, the data is usually first ingested from the source systems into the data lake. Then it is processed (deduplicated, cleaned, trasformed) using some ETL tools and efficently loaded ([MPP](https://www.sisense.com/glossary/mpp-database/)) into the data warehouse. On top of the data warehouse there are ususally some data marts which are slices of ceratin business areas and act as semantic models easy to understand to the business users.

Both data warehouse and data marts can be then queried using some reporting solution like Power BI, Tableau or QlikView. This repoting tool is often referred to as a "frontend" or "BI" part and technically its a 3<sup>rd</sup> party software, but logically it is also belongs to the data platform.

There are also some other emerging approaches like [data lakehouse](https://databricks.com/blog/2020/01/30/what-is-a-data-lakehouse.html) but this is the most common design as of today.

#### AI

AI (data science & machine learning) is another popular workload that are a good fit for the cloud. Availability of certain, managed storage (HDFS compatible data lake), compute (large VM instances/clusters, Kubernetes, big data tools like Spark) technologies and enablers (Azure ML, Azure DevOps, API hosting options like App Services, Azure Functions) make it relatively easy to build and run scalable Data & AI solution in the cloud.

![Cloud based data & AI solution](/images/2021-12-12-cdp-intro/ai.svg)
<p style="text-align: center;">Image 4: Cloud-based data & AI solution</p>

In typical setup, the solution usually consists of a data lake, some ETL / data processing components, MLOps environment and model hosting resources. 

Data scientists conduct data exploration & analysis looking for some usefull insights. Based on that, thy design and evaluate various use-case oriented ML models. 

These models are later oprationalized using machine learning pipelines for automated training and inference (getting predictions for actual input data). Some of the models can be also shared with external downstream systems via APIs.

#### Data Hub

The last use case I wanted to mention is focused on conducting data integration and mediation between various IT systems acrosss the enterprise. 

![Cloud based data hub solution](/images/2021-12-12-cdp-intro/data-hub.svg)
<p style="text-align: center;">Image 5: Cloud-based data hub solution</p>

In this scenario, the data is ingested from various sources in real-time or batch mode. It is cleaned, integrated, transformed, sometimes stored using certain data models inside data lake. 

Then, the prepared data is utilized by some use-case oriented components that process and store the data accoring to specific use case needs. Sometimes the data lake is bypassed and the incoming data is directly transformed and stored in use case oriented databases or models (like from XYZ system on the diagram above).

Lastly, the data is then served using various techniques (like APIs, message queues, files, SFTPs etc.) with downstream systems.

**Small disclaimer**: The "data hub" term is often used as a marketing name for off-the-shelf products from various software vendors. Each vendor interprets and and applies this name independently based on their own interpretation of data hub. So it is totally likely (and OK) to find a diffrent architcture for it. However the basic idea is always the same - integrating data from various sources, processing it and making available for various consumers.

<p align="center">
  <img src="/images/2021-12-12-cdp-intro/data-hub-abstract.svg" />
</p>
<p style="text-align: center;">Image 5: An abstract representation the data hub concept</p>

#### Follow up

In the next post I am going to discuss a bit more detailed architecture of a modern, cloud-based data plaftorm.