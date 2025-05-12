---
layout: post
title: Migrate your Synapse Dedicated pool to Fabric Datawarehouse
date: "2025-05-07"
categories: ["Synapse", "Fabric", "Datawarehouse","Migraton"]
---
In this article, we address a common concern among our Azure clients, particularly those who chose [Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/overview-what-is){:target="_blank"} their data platform several months ago. Indeed since the launch of Microsoft Fabric in late 2023, many users have been questioning the long-term future of Synapse.  
Rest assured: as of today, **there are no plans to retire Synapse Analytics**, which continues to be our PaaS analytics service platform.The platform remains fully supported and maintained by Microsoft as announced in this [post](https://blog.fabric.microsoft.com/en-us/blog/microsoft-fabric-explained-for-existing-synapse-users/){:target="_blank"} by one of our leaders.

However, the next generation of Microsoft’s big data analytics solutions is now a **core part of Microsoft Fabric**. For clients considering a migration and looking to transition from a PaaS to a SaaS experience.  
First we will begin by mapping the key components and capabilities between the two platforms.

![Fabric Synapse Equivalent](/assets/images/synapse-fabric.jpg)

##### Introduction to Fabric Migration Assistant for Data Warehouse

For clients primarily using Spark workloads or data pipelines within Synapse, the migration to Microsoft Fabric is straightforward and well-supported. Microsoft provides detailed [documentation](https://learn.microsoft.com/en-us/fabric/data-engineering/migrate-synapse-overview){:target="_blank"} to guide this transition:

-	Overview of migrating Synapse to Fabric
-   Migrate Synapse Data Pipelines to Fabric

![MigrateScenario](/assets/images/migration-scenariosspark.png)

But what about the Synapse Data Warehouse — **specifically the Dedicated SQL Pool** that relies on its proprietary storage? It’s also important to note that Fabric Data Warehouse still has some limitations. However, the product is evolving rapidly, with new features being added regularly.  You can follow the latest updates and planned improvements on the official Microsoft Fabric [roadmap](https://learn.microsoft.com/en-us/fabric/release-plan/data-warehouse){:target="_blank"} and review the current list of [limitations](https://learn.microsoft.com/en-us/fabric/data-warehouse/limitations){:target="_blank"}

##### Step-by-step guide to using the assistant

**The Fabric Migration Assistant is a migration experience** to copy SQL pools in Azure Synapse Analytics seamlessly into Microsoft Fabric Data Warehouse.
It allows copies metadata and data from the source database, automatically converting the source schema to Fabric Data Warehouse.
**AI-powered assistance** provides quick solutions for migration incompatibility or errors.

Microsoft provides a comprehensive list of [prerequisites](https://learn.microsoft.com/en-us/fabric/data-warehouse/migrate-with-migration-assistant#prerequisites){:target="_blank"} on its official documentation site.
However, in this guide, I’ll focus on **the more complex aspects** that may require special attention when using the assistant.

**Extract DACPAC (data-tier application package) file from Synapse Analytics Dedicated SQL Pool**

The first step is to extract the metadata from your Synapse Analytics Dedicated SQL Pool. This includes the schema definitions for tables, views, stored procedures, functions, and other database objects.  
For my migration tests, I used a Synapse database model provided as part of a Microsoft [hands-on lab](https://github.com/solliancenet/MCW-Azure-Synapse-Analytics/blob/master/Hands-on%20lab/HOL%20step-by%20step%20-%20Azure%20Synapse%20Analytics%20end-to-end%20solution.md){:target="_blank"}.  
This database includes several tables, which I’ve listed below using SQL Server Management Studio.I've also added some SQL Views or stored procedures that customers like to use for data transformations in their datawarehouses.

![Tables_list](/assets/images/Listoftables.jpg)

The first challenge I encountered was that, unlike an on-premises MS SQL database, I couldn’t generate the DACPAC of The SQL dedicated Pool Database directly from the [SSMS](https://learn.microsoft.com/en-us/ssms/download-sql-server-management-studio-ssms){:target="_blank"} (Sql Server Management Studio) interface. Instead, I had to use the command line [SqlPackage](https://learn.microsoft.com/en-us/sql/tools/sqlpackage/sqlpackage-download?view=sql-server-ver16){:target="_blank"} CLI to perform the extraction.

1) Download and install SqlPackage

```
# Commandline inside Powershell
dotnet tool install -g microsoft.sqlpackage
```

![installsqlpackage](/assets/images/installsqlpackage.png)

2) Execute SQL Package command to extract DACPAC File

When using SQLPackage, you can choose to extract the DACPAC file either to Azure Blob Storage [here](https://learn.microsoft.com/en-us/sql/tools/sqlpackage/sqlpackage-for-azure-synapse-analytics?view=sql-server-ver16#example){:target="_blank"} or directly to your local machine. In my case, I opted for local extraction, as I will need to access the file locally when using the migration assistant in Microsoft Fabric.
```
# Commandline inside Powershell
SqlPackage /Action:Extract /SourceFile:databaseschema.dacpac /TargetServerName:yourserver.sql.azuresynapse.net /TargetDatabaseName:databasename /TargetUser:sqladmin /TargetPassword:{your_password} 
```
![extractdacpac](/assets/images/extractdacsql.png)

Here we are — we can now move on to using the migration assistant, which is directly accessible within Microsoft Fabric.

![migrateinfabric](/assets/images/migrateinfabric.jpg)

**Use the migration assistant in Microsoft Fabric**

1)Metadata migration

By selecting "Migrate" during the Review step, a new data warehouse is created, initiating the metadata migration process. During this phase, the T-SQL metadata is converted to formats supported by the Fabric data warehouse.  
The database objects that will be migrated include tables, views, functions, stored procedures, and security objects. Once the metadata migration is finished, the Migration Assistant opens and displays a summary of the migration.

![migrationobjectsummary](/assets/images/migratedobjectjpg.jpg)

2)Fix Problems with Copilot

Sometimes, certain objects don’t migrate successfully — either because their T-SQL metadata can’t be translated into a format supported by Fabric due to limitation that I mentionned earlier, or because the converted code doesn’t run properly. That’s where the “Fix problems” step in the migration assistant comes in handy: it helps you review and correct any scripts that didn’t make it through.  

Use Copilot for AI-powered assistance in fixing some errors, select Fix query errors in the Suggested action section. Copilot updates the script with suggestions. Mistakes can happen as Copilot uses AI, so verify code suggestions and make any adjustments you need.

![migrationobjectsummary](/assets/images/fixthepb.png)

3)Copy Synapse Dedicated pool data by using Fabric


Copy data helps with migrating data used by the objects you migrate. You can use the copy job [documentation](https://learn.microsoft.com/en-us/fabric/data-factory/what-is-copy-job){:target="_blank"} to do it manually or follow the steps in the copy job wizard integrated into the Migration assistant.  Start by selecting your Synapse Dedicated pool data source, then choose the tables and columns to map to your new Fabric data warehouse tables.
Not all errors can be resolved automatically with Copilot — some will require manual rework. However, Copilot can still significantly assist you in identifying and addressing these issues.

![migrationobjectsummary](/assets/images/mappingtable.png)

4)Reroute connections

This part is clearly missing for now, but it’s expected to be added as the migration assistant is still in preview. The idea is that any data loading or reporting platforms connected to your original source will need to be reconnected to your new Fabric warehouse.  

For example, in Azure Synapse Analytics dedicated SQL pools, you can find session information including source application by [sys.dm_pdw_exec_sessions](https://learn.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql?view=aps-pdw-2016-au7){:target="_blank"} , who is connected in, where the connection is coming from, and if its using Microsoft Entra ID or SQL authentication.

##### Conclusion

Migrating a **Synapse Dedicated SQL Pool** to **Microsoft Fabric Data Warehouse**  aligns with the evolution toward a unified, SaaS-oriented analytics platform. With this Datawarehouse migration Assistant, Microsoft offers a "semi-automated" process to transfer metadata, T-SQL objects, and data, while integrating AI-powered support via **Copilot** to resolve syntax or compatibility issues.  

However, this migration is not trivial (even with the assistance of Copilot) and It requires:
- A **preliminary dependency analysis** (unsupported objects, specific T-SQL functions, security),
- **Manual adaptation** of complex or non-convertible scripts,
- **Reconfiguration of data pipelines** and reporting tools connected to the original source.

**It’s important to note that this migration assistant is currently in preview within Microsoft Fabric at the time of writing. Many of the limitations highlighted by the migration assistant are expected to be addressed or resolved by the time the tool reaches General Availability (GA).**  

For organizations looking to adopt a SaaS-based approach, this migration is not only feasible but also strongly encouraged, as it enables seamless integration with the broader Microsoft Fabric ecosystem.  

Once migrated, the Fabric Data Warehouse can leverage [shortcuts](https://learn.microsoft.com/en-us/fabric/onelake/onelake-shortcuts){:target="_blank"} to access data across other Fabric engines—such as Lakehouse, [Real-Time Intelligence]( https://learn.microsoft.com/en-us/fabric/real-time-intelligence/overview){:target="_blank"}, and Power BI—unlocking a unified and highly interoperable analytics experience.