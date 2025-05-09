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


![Fabric Snowflake](/assets/images/blog_post_image.jpg)

##### Introduction to lakehouse, data files formats
Before discussing the features in detail, let's recall the benefits of a lakehouse and the different formats used in the data analytics world.

Lakehouse represents a significant advance in data management, combining flexibility, performance and governance to meet current and future business challenges. But when it comes to storage formats, well, vendors (for the most part) have storage formats that are certainly open, but different.

While the fundamentals of table formats are generally similar, each format includes a unique metadata layer on top of Parquet files:

![Fabric Architecture](/assets/images/format.jpg)


The question is, if their fundamentals aren't all that different, which one should you choose?

##### Apache Xtable project

Well, wouldn't it be better not to have to choose? And so X Table was born, for seamless interoperability between Apache Hudi, Delta Lake and Apache Iceberg tables. 
Apache XTable provides abstraction interfaces that allow omni-directional interoperability across these 3 differents formats.

![Fabric Architecture](/assets/images/xtable.jpg)

##### Write an Iceberg table to OneLake using Snowflake

In Snowflake , Apache Iceberg™ tables for Snowflake combine the performance and query semantics of typical Snowflake tables with external cloud storage that you manage. They are ideal for existing data lakes that you cannot, or choose not to, store in Snowflake.

![Fabric Architecture](/assets/images/icebergsnow.jpg)

Make sure your Fabric capacity is in the same Azure location as your Snowflake instance.
In Snowflake, set up your EXTERNAL VOLUME using the path to the Files folder in your lakehouse


```
# Sql code inside snowflake
#CREATE OR REPLACE EXTERNAL VOLUME onelake_exvol
STORAGE_LOCATIONS =
(
    (
        NAME = 'onelake_exvol'
        STORAGE_PROVIDER = 'AZURE'
        STORAGE_BASE_URL = 'azure://<path_to_Files>/icebergtables'
        AZURE_TENANT_ID = '<Tenant_ID>'
    )
);
```

Now the external volume is created Open the consent URL from the previous step in a new browser tab and proceed, consent to the required application permissions, if prompted.

Run the following command to retrieve the consent URL and name of the application that Snowflake uses to write to OneLake

```
# Sql code inside snowflake

DESC EXTERNAL VOLUME onelake_exvol;
#check if you are abble to have the right access to Onelake 
SELECT SYSTEM$VERIFY_EXTERNAL_VOLUME('onelake_nts');

# use your new external volume to create an Iceberg table.
CREATE OR REPLACE ICEBERG TABLE MYDATABASE.PUBLIC.Inventory (
    InventoryId int,
    ItemName STRING
)
EXTERNAL_VOLUME = 'onelake_exvol'
CATALOG = 'SNOWFLAKE'
BASE_LOCATION = 'Inventory/';

```
##### Create a table shortcut to an Iceberg table in Fabric

In Microsoft OneLake,create shortcuts to your Apache Iceberg tables from Snowflake, making them accessible across various Fabric workloads.
Open your workspace and select Manage access, then Add people or groups. Grant the application used by your Snowflake external volume the permissions needed to write data to lakehouses in your workspace. We recommend granting the Contributor role.
When you set up a shortcut to an Iceberg table folder, OneLake automatically generates the necessary Delta Lake metadata (the Delta log) for that table, ensuring the Delta Lake metadata is available through the shortcut.

![Fabric Architecture](/assets/images/iceberg-shortcut-diagram.jpeg)


##### End to End Scenario with the solution

In this reporting-oriented scenario, this is an architecture where the Snowflake exposition layer writes its tables into Onelake storage in Iceberg format via external volumes. Subsequently, Fabric uses a shortcut to mount the Iceberg file into a table and allow data reading for PowerBI in direct lake mode. This solution enables access to Snowflake data without copying or loading data, offering an alternative to the direct query or import mode for PowerBI. It reduces costs by avoiding intensive use of Snowflake compute and Fabric.

![scenario Architecture](/assets/images/archi-end.jpg)


##### Soonest Benchmarking the solution through power bi performance

For our benchmark, we will use the TPC-H, established by the Transaction Processing Performance Council (TPC) . It is a standard benchmark for decision support systems in retail business scenarios. 