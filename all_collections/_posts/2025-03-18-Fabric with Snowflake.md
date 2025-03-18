---
layout: post
title: Interoperability Fabric with Snowflake
date: "2023-08-07"
categories: ["Snowflake", "Fabric", "Iceberg"]
---
In May 2024, Snowflake and Microsoft [annouced](https://www.snowflake.com/en/blog/microsoft-partnership-enhancing-interoperability/){:target="_blank"} the extension of their partnership through the Iceberg file format, establishing an intelligible exchange format between their technologies. This article explores technically how this announcement optimizes the use of products without compromising your data and analytics strategy.
The goal of this article is not to compare the two solutions, but to explain the interoperability of the two technologies, often examined in the context of consuming Power BI reports hosted on a Snowflake data warehouse.

In order to deliver these benefits, Snowflake and Fabric will add these capabilities for customers now and later this year:

- Snowflake will be able to store data in Iceberg format in OneLake 
- Fabric will be able to store data in Iceberg format in OneLake via Apache XTable translation in OneLake.
- Data written by either platform, Snowflake or Fabric, will be accessible from both the platforms.
- Snowflake will be able to read Fabric data artifacts in OneLake, stored physically or virtually through shortcuts.
- Data from Snowflake and Fabric can be used by a variety of services, such as Microsoft 365, Copilot in Microsoft 365, Snowflake Cortex AI, Azure AI Studio, Snowflake Copilot and more.

![Fabric shortcut](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/shortcut.jpg?raw=true)

##### Introduction to lakehouse, formats

Lakehouse represents a significant advance in data management, combining flexibility, performance and governance to meet current and future business challenges. But when it comes to storage formats, well, vendors (for the most part) have storage formats that are certainly open, but different

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/fabric_schema.jpg?raw=true)

While the fundamentals of table formats are generally similar, each format includes a unique metadata layer on top of Parquet files:

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/fabric_schema.jpg?raw=true)

The question is, if their fundamentals aren't all that different, which one should you choose?

##### Xtable

Well, wouldn't it be better not to have to choose? And so X Table was born, for seamless interoperability between Apache Hudi, Delta Lake and Apache Iceberg tables. 

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/fabric_schema.jpg?raw=true)
   
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
