---
layout: post
title: Leverage your data assets with Microsoft Fabric
date: "2023-08-07"
categories: ["Data", "Fabric", "Shortcut"]
---
**Microsoft Fabric is currently in public preview**


For my first post about [Microsoft Fabric](https://www.microsoft.com/en-gb/microsoft-fabric/){:target="_blank"} the unified analytics solution of Microsoft,I'd like to raise an initial topic of migration path for customers.
The migration scenario to study involves taking over the existing services of customers who currently use managed data services on Azure to use Spark such as [Microsoft HDInsight](https://azure.microsoft.com/en-gb/products/hdinsight/){:target="_blank"} or [Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/overview-what-is){:target="_blank"}, using the Spark engine to process their data and obviously associating the storage layer via [Azure Data Lake Storage Gen2](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction){:target="_blank"} for their datalake.
It is also possible to take advantage of its data with Fabric in multi-cloud scenarios, where it is possible to "shortcut" data from GCP or AWS.

To store the data, [Parquet file](http://parquet.incubator.apache.org){:target="_blank"} is often the preferred format for storing data because is a columnar format that is supported by many other data processing systems for reading and writing.
Microsoft's Fabric promise is to enable its customers to leverage the data assets they already have in place in order to migrate to new fabric service offering new features.

##### Introduction to Onelake

OneLake comes automatically with every Microsoft Fabric tenant and is designed to be the single place for all your analytics data.OneLake is a single, unified, logical data lake for the whole organization. Like OneDrive, OneLake brings customers:

- One data lake for the entire organization
- One copy of data for use with multiple analytical engines

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/fabric_schema.jpg?raw=true)

##### OneLake shortcuts

When you start with Onelake in Fabric, you can create a shortcut enabling you to use your Datalake on Azure (ADLS G2) , GCP (Google Cloud Storage) soonest or AWS (Bucket S3).Shortcuts can be created both in Lakehouses and KQL Databases.
![Fabric shortcut](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcut.jpg?raw=true)
To tackle this scenario, We'll explain how to create this shortcut in a lakehouse from within Fabric.
Before you start, you need to create a Fabric capacity in trial version using the [following procedure](https://learn.microsoft.com/en-us/fabric/get-started/fabric-trial){:target="_blank"}.
1.	Create or Open your [lakehouse](https://learn.microsoft.com/en-us/fabric/data-engineering/tutorial-lakehouse-introduction)
2.	Right click on a directory within the Lake view of the lakehouse.
3.	Select New shortcut with 2 Possible scenarios : 
- You already have Delta files in your datalake, create the shortcut from the Tables folder
- You have parquet files, create the shortcut from the Files folder
![Creation shortcut](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcut_creation.jpg?raw=true)
4.	Select External sources (here will be ADLS G2)
![New shortcut](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/new_shortcut.jpg?raw=true)
5.  The External ADLS G2 need to be located in the same region than your lakehouse / the link must be the DFS endpoint for the storage account.
![New shortcut adlsg2](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcutadlsg.jpg?raw=true)
This is the tree structure of your datalake
![New shortcut adlsg2 list](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcutlists.jpg?raw=true)
Note that if you already have your data in Delta format you can use the managed tables, otherwise you will need to mount your files and generate the Delta format files from the parquet files in your datalake.
![New shortcut mount table](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/MountTable.jpg?raw=true)
6. Notebooks can be used to programmatically mount all your parquet files in delta format and make them available on the table.
![New shortcut notebook](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/notebook.jpg?raw=true)
```Python
# Python code with syntax highlighting
#This cell sets Spark session settings to enable Verti-Parquet and Optimize on Write.
spark.conf.set("sprk.sql.parquet.vorder.enabled", "true")
spark.conf.set("spark.microsoft.delta.optimizeWrite.enabled", "true")
spark.conf.set("spark.microsoft.delta.optimizeWrite.binSize", "1073741824")
```
```Python
table_name = 'fact_sale'
#This cell save the dataframe to delta file
df = spark.read.format("parquet").load('Files/AdlsG2-Marc/sale2023.parquet')
df.write.mode("overwrite").format("delta").partitionBy("Year").save("Tables/" + table_name)
```

Bellow is the parquet file in table format including the structure with available columns.
![New shortcut table structure](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/sales_table.jpg?raw=true)
We can then make the most of your data, including the option of using [Direct Lake](https://learn.microsoft.com/en-us/power-bi/enterprise/directlake-overview) with PowerBI and delta tables.
![New shortcut table structure](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/directlake-diagram.jpeg?raw=true)
