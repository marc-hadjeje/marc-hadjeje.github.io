---
layout: post
title: Migrate your Synapse Dedicated pool to Fabric Datawarehouse
date: "2025-05-07"
categories: ["Synapse", "Fabric", "Datawarehouse","Migraton"]
---
In this article, we address a common concern among our Azure clients, particularly those who chose [Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/overview-what-is){:target="_blank"} their data platform several months ago. Since the launch of Microsoft Fabric in late 2023, many users have been questioning the long-term future of Synapse.
Rest assured: as of today, there are no plans to retire Synapse Analytics. The platform remains fully supported and maintained by [Microsoft](https://blog.fabric.microsoft.com/en-us/blog/microsoft-fabric-explained-for-existing-synapse-users/){:target="_blank"}.

However, the next generation of Microsoft’s big data analytics solutions is now a core part of Microsoft Fabric. For clients considering a migration and looking to transition from a PaaS to a SaaS experience, we will begin by mapping the key components and capabilities between the two platforms.

![Fabric Synapse Equivalent](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/synapse-fabric.jpg?raw=true)

##### Introduction to Fabric Migration Assistant for Data Warehouse

For clients primarily using Spark workloads or data pipelines within Synapse, the migration to Microsoft Fabric is straightforward and well-supported. Microsoft provides detailed documentation to guide this transition:

-	Overview of migrating Synapse to Fabric
-   Migrate Synapse Data Pipelines to Fabric

![MigrateScenario](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/migration-scenariosspark.png?raw=true)

But what about the Synapse Data Warehouse — specifically the Dedicated SQL Pool that relies on its proprietary storage?

##### Step-by-step guide to using the assistant
The Fabric Migration Assistant is a migration experience to copy SQL pools in Azure Synapse Analytics seamlessly into Microsoft Fabric Data Warehouse.
It allows copies metadata and data from the source database, automatically converting the source schema to Fabric Data Warehouse. AI-powered assistance provides quick solutions for migration incompatibility or errors.
Microsoft provides a comprehensive list of [prerequisites](https://learn.microsoft.com/en-us/azure/synapse-analytics/overview-what-is){:target="_blank"} on its official documentation site. However, in this guide, I’ll focus on the more complex aspects that may require special attention when using the assistant.

##### Extract DACPAC (data-tier application package) file from Synapse Analytics Dedicated SQL Pool

The first step is to extract the metadata from your Synapse Analytics Dedicated SQL Pool. This includes the schema definitions for tables, views, stored procedures, functions, and other database objects.
For my migration tests, I used a Synapse database model provided as part of a Microsoft [hands-on lab](https://learn.microsoft.com/en-us/azure/synapse-analytics/overview-what-is){:target="_blank"}. This database includes several tables, which I’ve listed below using SQL Server Management Studio

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/icebergsnow.jpg?raw=true)

The first challenge I encountered was that, unlike an on-premises MS SQL database, I couldn’t generate the DACPAC of The SQL dedicated Pool Database directly from the SSMS interface. Instead, I had to use the command line SqlPackage CLI to perform the extraction.

1.	Download and install SqlPackage

```
# Commandline inside Powershell
dotnet tool install -g microsoft.sqlpackage
```

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/icebergsnow.jpg?raw=true)

2.	Execute SQL Package command to extrat DACPAC File.

When using SQLPackage, you can choose to extract the DACPAC file either to Azure Blob Storage [hands-on lab](https://learn.microsoft.com/en-us/sql/tools/sqlpackage/sqlpackage-for-azure-synapse-analytics?view=sql-server-ver16#example){:target="_blank"} or directly to your local machine. In my case, I opted for local extraction, as I will need to access the file locally when using the migration assistant in Microsoft Fabric.
```
# Commandline inside Powershell
SqlPackage /Action:Publish /SourceFile:databaseschema.dacpac /TargetServerName:yourserver.sql.azuresynapse.net /TargetDatabaseName:databasename /TargetUser:sqladmin /TargetPassword:{your_password} 
```
![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/icebergsnow.jpg?raw=true)

##### Extract DACPAC (data-tier application package) file from Synapse Analytics Dedicated SQL Pool

In Microsoft OneLake,create shortcuts to your Apache Iceberg tables from Snowflake, making them accessible across various Fabric workloads.
Open your workspace and select Manage access, then Add people or groups. Grant the application used by your Snowflake external volume the permissions needed to write data to lakehouses in your workspace. We recommend granting the Contributor role.
When you set up a shortcut to an Iceberg table folder, OneLake automatically generates the necessary Delta Lake metadata (the Delta log) for that table, ensuring the Delta Lake metadata is available through the shortcut.

![Fabric Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/iceberg-shortcut-diagram.jpeg?raw=true)


##### End to End Scenario with the solution

In this reporting-oriented scenario, this is an architecture where the Snowflake exposition layer writes its tables into Onelake storage in Iceberg format via external volumes. Subsequently, Fabric uses a shortcut to mount the Iceberg file into a table and allow data reading for PowerBI in direct lake mode. This solution enables access to Snowflake data without copying or loading data, offering an alternative to the direct query or import mode for PowerBI. It reduces costs by avoiding intensive use of Snowflake compute and Fabric.

![scenario Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/archi-end.jpg?raw=true)
