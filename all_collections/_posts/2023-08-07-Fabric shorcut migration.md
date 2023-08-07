---
layout: post
title: Leverage your data with Microsoft Fabric
date: "2023-08-07"
categories: ["Data", "Fabric", "Shortcut"]
---
For my first post about [Microsoft Fabric](https://www.microsoft.com/en-gb/microsoft-fabric/){:target="_blank"} the unified analytics solution of Microsoft in preview,I'd like to raise an initial topic of migration path for customers.
The migration scenario to study involves taking over the existing services of customers who currently use managed data services on Azure to use Spark such as [Microsoft HDInsight](https://azure.microsoft.com/en-gb/products/hdinsight/){:target="_blank"} or [Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/overview-what-is){:target="_blank"}, using the Spark engine to process their data and obviously associating the storage layer via Azure Data Lake Storage Gen2 for their datalake.

To store the data, [Parquet file](http://parquet.incubator.apache.org){:target="_blank"} is often the preferred format for storing data because is a columnar format that is supported by many other data processing systems for reading and writing.
Microsoft's Fabric promise is to enable its customers to leverage the data assets they already have in place in order to migrate to new fabric platform offering new services.

##### Introduction to Onelake

OneLake is a single, unified, logical data lake for the whole organization. Like OneDrive, OneLake comes automatically with every Microsoft Fabric tenant and is designed to be the single place for all your analytics data. OneLake brings customers:

- One data lake for the entire organization
- One copy of data for use with multiple analytical engines

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/fabric_schema.jpg?raw=true)
When you start up Onelake in Fabric, you can create a shortcut enabling you to use your Datalake on Azure (Adls G2) , GCP (Google Store Storage) soonest or AWS (S3) in your lakehouse.
![Fabric shortcut](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcut.jpg?raw=true)

To tackle this scenario, We'll explain how to create this shortcut from within Fabric.
Before you start, you need to create a Fabric capacity in trial version using the [following procedure](https://learn.microsoft.com/en-us/fabric/get-started/fabric-trial){:target="_blank"}.

1.	Create or Open your lakehouse
2.	Right click on a directory within the Lake view of the lakehouse.
3.	Select New shortcut.

