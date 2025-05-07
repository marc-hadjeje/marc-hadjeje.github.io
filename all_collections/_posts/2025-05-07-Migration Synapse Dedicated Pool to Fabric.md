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
Microsoft provides a comprehensive list of [prerequisites](https://learn.microsoft.com/en-us/fabric/data-warehouse/migrate-with-migration-assistant#prerequisites){:target="_blank"} on its official documentation site. However, in this guide, I’ll focus on the more complex aspects that may require special attention when using the assistant.

##### Extract DACPAC (data-tier application package) file from Synapse Analytics Dedicated SQL Pool

The first step is to extract the metadata from your Synapse Analytics Dedicated SQL Pool. This includes the schema definitions for tables, views, stored procedures, functions, and other database objects.
For my migration tests, I used a Synapse database model provided as part of a Microsoft [hands-on lab](https://github.com/solliancenet/MCW-Azure-Synapse-Analytics/blob/master/Hands-on%20lab/HOL%20step-by%20step%20-%20Azure%20Synapse%20Analytics%20end-to-end%20solution.md){:target="_blank"}. This database includes several tables, which I’ve listed below using SQL Server Management Studio

![Tables_list](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/Listoftables.jpg?raw=true)

The first challenge I encountered was that, unlike an on-premises MS SQL database, I couldn’t generate the DACPAC of The SQL dedicated Pool Database directly from the SSMS interface. Instead, I had to use the command line SqlPackage CLI to perform the extraction.

1.	Download and install SqlPackage

```
# Commandline inside Powershell
dotnet tool install -g microsoft.sqlpackage
```

![installsqlpackage](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/installsqlpackage.png?raw=true)

2.	Execute SQL Package command to extrat DACPAC File.

When using SQLPackage, you can choose to extract the DACPAC file either to [Azure Blob Storage](https://learn.microsoft.com/en-us/sql/tools/sqlpackage/sqlpackage-for-azure-synapse-analytics?view=sql-server-ver16#example){:target="_blank"} or directly to your local machine. In my case, I opted for local extraction, as I will need to access the file locally when using the migration assistant in Microsoft Fabric.
```
# Commandline inside Powershell
SqlPackage /Action:Extract /SourceFile:databaseschema.dacpac /TargetServerName:yourserver.sql.azuresynapse.net /TargetDatabaseName:databasename /TargetUser:sqladmin /TargetPassword:{your_password} 
```
![extractdacpac](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/extractdacsql.png?raw=true)

Here we are — we can now move on to using the migration assistant, which is directly accessible within Microsoft Fabric.

![extractdacpac](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/migrateinfabric.jpg?raw=true)

##### Extract DACPAC (data-tier application package) file from Synapse Analytics Dedicated SQL Pool


