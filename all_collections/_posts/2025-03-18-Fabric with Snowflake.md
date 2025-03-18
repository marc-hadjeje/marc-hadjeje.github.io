---
layout: post
title: Interoperability between Fabric and Snowflake
date: "2025-03-18"
categories: ["Snowflake", "Fabric", "Iceberg"]
---
In May 2024, Snowflake and Microsoft [announced](https://www.snowflake.com/en/blog/microsoft-partnership-enhancing-interoperability/){:target="_blank"} the extension of their partnership through the Iceberg file format, establishing an intelligible exchange format between their technologies. This article explores technically how this announcement optimizes the use of products without compromising your data and analytics strategy.

The goal of this article is not to compare the two solutions, but to explain the interoperability of the two technologies, often examined in the context of consuming Power BI reports hosted on a Snowflake data warehouse.

In order to deliver these benefits, Snowflake and Fabric will add these capabilities for customers :

- Snowflake will be able to store data in Iceberg format in OneLake 
- Fabric will be able to store data in Iceberg format in OneLake via Apache XTable translation in OneLake.


![Fabric Snowflake](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/blog_post_image.jpg?raw=true)

##### Introduction to lakehouse, data files formats
Before discussing the features in detail, let's recall the benefits of a lakehouse and the different formats used in the data analytics world.

Lakehouse represents a significant advance in data management, combining flexibility, performance and governance to meet current and future business challenges. But when it comes to storage formats, well, vendors (for the most part) have storage formats that are certainly open, but different.

While the fundamentals of table formats are generally similar, each format includes a unique metadata layer on top of Parquet files:

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/format.jpg?raw=true)


The question is, if their fundamentals aren't all that different, which one should you choose?

##### Xtable

Well, wouldn't it be better not to have to choose? And so X Table was born, for seamless interoperability between Apache Hudi, Delta Lake and Apache Iceberg tables. 

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/xtable.jpg?raw=true)

##### End to End Scenario 

In this reporting-oriented scenario, we propose an architecture where the Snowflake exposition layer writes its tables into Onelake storage in Iceberg format via external volumes. Subsequently, Fabric uses a shortcut to mount the Iceberg file into a table and allow data reading for PowerBI in direct lake mode. This solution enables access to Snowflake data without copying or loading data, offering an alternative to the direct query or import mode for PowerBI. It reduces costs by avoiding intensive use of Snowflake compute and Fabric.

![scenario Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/archi-end.jpg?raw=true)

##### Deep dive in Snowflake 
In Snowflake , Apache Iceberg™ tables for Snowflake combine the performance and query semantics of typical Snowflake tables with external cloud storage that you manage. They are ideal for existing data lakes that you cannot, or choose not to, store in Snowflake.

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/fabric_schema.jpg?raw=true)

```
# Python code 
#This cell sets Spark session settings to enable Verti-Parquet and Optimize on Write.
spark.conf.set("sprk.sql.parquet.vorder.enabled", "true")
spark.conf.set("spark.microsoft.delta.optimizeWrite.enabled", "true")
spark.conf.set("spark.microsoft.delta.optimizeWrite.binSize", "1073741824")
```

##### Deep dive in Fabric 

In Microsoft OneLake, you can create shortcuts to your Apache Iceberg tables from Snowflake, making them accessible across various Fabric workloads. This is achieved through metadata virtualization ( X Table), which allows Iceberg tables to be viewed as Delta Lake tables via the shortcut. 
When you set up a shortcut to an Iceberg table folder, OneLake automatically generates the necessary Delta Lake metadata (the Delta log) for that table, ensuring the Delta Lake metadata is available through the shortcut.

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/fabric_schema.jpg?raw=true)

```
# Python code
#This cell sets Spark session settings to enable Verti-Parquet and Optimize on Write.
spark.conf.set("sprk.sql.parquet.vorder.enabled", "true")
spark.conf.set("spark.microsoft.delta.optimizeWrite.enabled", "true")
spark.conf.set("spark.microsoft.delta.optimizeWrite.binSize", "1073741824")
```


