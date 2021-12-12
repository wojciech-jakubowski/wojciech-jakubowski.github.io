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

![EX](/images/2021-12-12-cdp-intro/traditional-bi.png)

Modern cloud based data platforms do prety much the same, but the scope and use cases are usually much broader and capabities are bigger. 

### Use cases

The typical use cases and scenarios for modern cloud based data platforms are:

#### Analytics

BI & data warehousing is still a thing [inmon link]. But since we live in cloud era and companies are intensively going to the cloud, it often makes sense to migrate a legacy DW as well. Or build a new one using modern, cloud native technologies such as Snowflake or Azure Synapse Analytics.

#### AI, Data Science & ML

#### Use case specfic solutions

#### Data Hub