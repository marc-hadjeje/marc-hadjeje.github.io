---
layout: post
title: Interoperability Fabric with Snowflake
date: "2023-08-07"
categories: ["Snowflake", "Fabric", "Iceberg"]
---
In May 2024, Snowflake and Microsoft [annouced](https://www.snowflake.com/en/blog/microsoft-partnership-enhancing-interoperability/){:target="_blank"} the extension of their partnership through the Iceberg file format, establishing an intelligible exchange format between their technologies. This article explores technically how this announcement optimizes the use of products without compromising your data and analytics strategy.

![Fabric shortcut](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcut.jpg?raw=true)

##### Introduction to Iceberg

OneLake comes automatically with every Microsoft Fabric tenant and is designed to be the single place for all your analytics data.OneLake is a single, unified, logical data lake for the whole organization.Like OneDrive, OneLake brings customers:

- One data lake for the entire organization
- One copy of data for use with multiple analytical engines

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/fabric_schema.jpg?raw=true)

##### OneLake shortcuts

When you start with Onelake in Fabric, you can create a shortcut enabling you to use your Datalake on Azure (ADLS G2), GCP (Google Cloud Storage) soonest or AWS (Bucket S3).Shortcuts can be created both in Lakehouses and KQL Databases.
![Fabric shortcut](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcut.jpg?raw=true)
To tackle this scenario, I'll explain how to create this shortcut in a lakehouse from within Fabric.
Before you start, you need to create a Fabric capacity in trial version using the [following procedure](https://learn.microsoft.com/en-us/fabric/get-started/fabric-trial){:target="_blank"}.
1.	Create or Open your [lakehouse](https://learn.microsoft.com/en-us/fabric/data-engineering/tutorial-lakehouse-introduction)
2.	Right click on a directory within the Lake view of the lakehouse
3.	Select New shortcut with 2 possible scenarios : 
- You already have Delta files in your datalake, create the shortcut from the Tables folder
- You have parquet files, create the shortcut from the Files folder
![Creation shortcut](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcut_creation.jpg?raw=true)
4.	Select External sources (here will be ADLS G2)
![New shortcut](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/new_shortcut.jpg?raw=true)
5.  The External ADLS G2 need to be located in the same region than your lakehouse / the link must be the DFS endpoint for the storage account
![New shortcut adlsg2](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcutadlsg.jpg?raw=true)
This is the tree structure of your datalake :
![New shortcut adlsg2 list](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcutlists.jpg?raw=true)
Note that if you already have your data in Delta format you can use the managed tables, otherwise you will need to mount your files and generate the Delta format files from the parquet files in your datalake.
![New shortcut mount table](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/MountTable.jpg?raw=true)
6. Notebooks can be used to script mount all your parquet files in delta format and make them available on the table
![New shortcut notebook](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/notebook.jpg?raw=true)

7. To create this notebook, the code is written in Pyspark,we are going to use two features ([Verti-Parquet](https://learn.microsoft.com/en-us/fabric/data-engineering/delta-optimization-and-v-order?tabs=sparksql){:target="_blank"} and Optimize Write) of Fabric for optimized data writing and for subsequent better reading performance
   
```
# Python code 
#This cell sets Spark session settings to enable Verti-Parquet and Optimize on Write.
spark.conf.set("sprk.sql.parquet.vorder.enabled", "true")
spark.conf.set("spark.microsoft.delta.optimizeWrite.enabled", "true")
spark.conf.set("spark.microsoft.delta.optimizeWrite.binSize", "1073741824")
```
Save the Dataframe from parquet file to delta table

```
# Python code 
table_name = 'fact_sale'
#This cell save the dataframe to delta file
df = spark.read.format("parquet").load('Files/AdlsG2-Marc/sale2023.parquet')
df.write.mode("overwrite").format("delta").partitionBy("Year").save("Tables/" + table_name)
```

Bellow is the parquet file in table format including the structure with available columns.
![New shortcut table structure](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/sales_table.jpg?raw=true)
We can then make the most of your data, including the option of using [Direct Lake](https://learn.microsoft.com/en-us/power-bi/enterprise/directlake-overview) with PowerBI and delta tables.
![New shortcut table structure](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/directlake-diagram.jpeg?raw=true)
