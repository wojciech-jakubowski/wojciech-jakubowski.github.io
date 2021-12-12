---
title: "Cloud Data Platfom - part 1 - overview!"
date: 2021-08-22T21:10:00-02:00
categories:
  - blog
---

### Intro

Cloud based data platfoms are getting increasingly popular and lots of companies are investing in this area these days. Main reasons:

1. Some companies are migrating their existing legacy data warehouse/BI solutions to the cloud as a part of their "digital transformation" efforts.
2. Some companies start new initiatives in the area of analytics/BI.
3. Some companies are embracing advanced analytics, data science & ML.

Modern cloud based data platforms can address all of these needs and can bring tremendous value for the IT landscape and the whole business overall. 

In this series of blog posts I'd like to reflect various technical aspects modern data platfom solution. I am not going to disucuss business or organizational aspects. Instead, I want to cover following more architectural and technical topics:
1. Enterprise IT landscape, use cases (this post)
2. High level architecture, layers
3. Data ingestion layer
4. Storage and processing layer
5. Consuption layer
6. Underlying cloud infrastructure (technologies, resources, networking, security)
7. Observability, reconciliation, monitoring, alerting
8. DevOps practices (Infra as code, CI/CD)
9. Many more :)

Given my background, I'll focus on data platform running in Microsoft Azure, but most architectures, patterns & practcies apply to other cloud platforms as well.

### Some context - enterprise IT landscape.

A typical enterprise has a number of IT systems covering various aspects of the business (sales, accounting, manufacturing, supply chain etc.). Depending on company size it can be either a few, tens or hundreds. Some of these systems might be homegrown (ususally with the help of IT vendors), some might be taken "from the shelf" (ERP, CRM, Office, etc.) and some of them might be legacy after mergers/acquisitions/reorgs. Moreover, some of them might run on premise and some of them might live in the cloud. 

In order to get some holistic view on the overall state of the enterprise and levarage synergies, companies usually implemented enterprise data warehouses that brough the data from all these systems together, integrated it and presented for BI/reporting. 

![Traditional on-prem DW/BI solution](/images/2021-12-12-cdp-intro/traditional-bi.png)
<p style="text-align: center;">Image 1: Traditional on-prem DW/BI solution</p>

Modern cloud based data platforms do prety much the same, but the scope and use cases are usually much broader and capabities are bigger. Apart from traditional DW/BI, they also enable other scenarios such as Data/AI, Data Hub or some custom data solutions.

![Modern cloud based data platform](/images/2021-12-12-cdp-intro/cloud-dp.png)
<p style="text-align: center;">Image 2: Modern cloud based data platform</p>

Of course these scenarios were and are also doable in traditional on-prem setup. But now its much easier to build and run them in the cloud thanks to its elasticity, scalability and rich ecosystem of managed tools and resouces. You don't need heavy investments in on-prem infrastructure and skills required to run it - cloud vendor (usually) does that for you!

### Use cases

As mentioned above the typical use cases and scenarios for modern cloud-based data platforms are:

#### Analytics

BI & data warehousing is still a thing [inmon link]. However, since we live in the cloud era, it often makes sense to migrate a legacy DW as well as a part of some digital transformation or cloud migration endevour. Or build a new one from scratch using modern, cloud native technologies such as Snowflake or Azure Synapse Analytics.

![Cloud based data analytics solution](/images/2021-12-12-cdp-intro/cloud-dw.png)
<p style="text-align: center;">Image 3: Cloud based data analytics solution</p>

In such solution, the data is first ingested from the source systems into the [data lake](https://databricks.com/discover/data-lakes/introduction), which is a primary storage location and a key component. Then it is processed (deduplicated, cleaned, trasformed) using some ETL tools and loaded in efficent way ([MPP](https://www.sisense.com/glossary/mpp-database/)) into the data warehouse. On top of the data warehouse there are ususally some data marts which are slices of ceratin business areas and act as semantic models that are easy to understand to the business users.

Both data warehouse and data marts can be then queried using some reporting solution like Power BI, Tableau or QlikView. This repoting tool is often referred to as a "frontend" or "BI" part and technically its a 3<sup>rd</sup> party software, but logically it is also belongs to the data platform.

There are also some other approaches emerging like [data lakehouse](https://databricks.com/blog/2020/01/30/what-is-a-data-lakehouse.html) but this is the most common design as of now.

#### AI, Data Science & ML

Data science and machine learning are another popular workload that is a good fit for the cloud. Availability of certain storage (HDFS compatible data lake), compute (large VM instances/clusters, Kubernetes, big data tools like Spark) resources and MLOps enablers (Azure ML, multiple API hosting tools like AppServices, Azure Functions, Azure DevOps) make it relatively easy to build and run scalable Data & AI solution.

![Cloud based data & AI solution](/images/2021-12-12-cdp-intro/cloud-ai.png)
<p style="text-align: center;">Image 4: Cloud based data & AI solution</p>

A typical setup of such solution usally consists of a data lake (the same datlake used by Analytics workload!), some ETL / data processing tools, MLOps environment and Model hosting resources.

#### Use case specfic solutions

#### Data Hub