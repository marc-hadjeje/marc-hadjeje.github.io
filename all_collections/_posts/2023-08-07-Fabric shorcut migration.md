---
layout: post
title: Leverage your data with Microsoft Fabric
date: "2023-08-07"
categories: ["Data", "Fabric", "Shortcut"]
---
For my first post about [Microsoft Fabric](https://www.microsoft.com/en-gb/microsoft-fabric/){:target="_blank"} the unified analytics solution of Microsoft,I'd like to raise an initial topic of migration path for customers.
The migration scenario to study involves taking over the existing services of customers who currently use Data PaaS services on Azure to use Spark such as [Microsoft HDInsight](https://azure.microsoft.com/en-gb/products/hdinsight/){:target="_blank"} or [Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/overview-what-is){:target="_blank"}, using the Spark engine to process their data and obviously associating the storage layer via Azure Data Lake Storage Gen2 for their datalake.

To store the data , [Parquet](http://parquet.incubator.apache.org){:target="_blank"} is often the preferred format for storing data because is a columnar format that is supported by many other data processing systems for reading and writing.
Microsoft's Fabric promise is to enable its customers to leverage the data assets they already have in place in order to migrate to new fabric platform offering new services.

##### Introduction to Onelake

OneLake is a single, unified, logical data lake for the whole organization. Like OneDrive, OneLake comes automatically with every Microsoft Fabric tenant and is designed to be the single place for all your analytics data. OneLake brings customers:

- One data lake for the entire organization
- One copy of data for use with multiple analytical engines

![Synapse Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/fabric_schema.jpg?raw=true)
To tackle this scenario, there is a feature [Data Exfiltration Protection](https://learn.microsoft.com/en-us/azure/synapse-analytics/security/workspace-data-exfiltration-protection){:target="_blank"} (DEP) that enables additional restrictions on the ability of Azure Synapse Analytics to connect to other services – enabling you to further secure your Azure Synapse Analytics deployment. There are a couple of key things to know about DEP:
- DEP can only be enabled at Azure Synapse Analytics workspace creation and cannot be disabled at a later point. If you want to disable DEP, you will have to create a new Azure Synapse Analytics workspace and migrate all artifacts.

- DEP enables you to limit the communication from Azure Synapse Analytics. By requiring connections to other services to use managed private endpoints and to approved Azure AD tenants.

- DEP applies to all services within an Azure workspace including dedicated SQL pools, serverless SQL pools, Apache Spark pools and Pipelines.

As mentioned below, via the DEP feature it will now be impossible for a user to exfiltrate data from the Enterprise datalake to an external storage account via the SQL Serverless pool.
![Synapse Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/Synapse-DEP.jpg?raw=true)