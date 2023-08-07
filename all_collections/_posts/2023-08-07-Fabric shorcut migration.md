---
layout: post
title: Leverage your data with Microsoft Fabric
date: "2023-08-07"
categories: ["Data", "Fabric", "Shortcut"]
---
For my second post, I'm going to focus on a subject that is of major concern to many of my customers, data security inside their Data platform.

Beyond the network security aspects, what preoccupies companies are the aspects around data exfiltration which is a very considered threat by the security team.We will address this topic through the usage of [Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/overview-what-is){:target="_blank"},an enterprise analytics service.

Before briefly presenting the Synapse architecture and the possible protection capabilities, you will find below the definition of what is data exfiltration:
- Data exfiltration, aka data leakage, aka data extrusion, occurs when sensitive data is exfiltrated to locations outside of an 
In fact, there are two types of data exfiltration threats: **attacks from outside the organization**, and **malicious insiders stealing their organization**.

- In the public cloud business, data exfiltration usually stands for the second scenario: a malicious insider, who has legitimate access to sensitive data, is exfiltrating it to locations outside of the organization.
##### Presentation of Synapse Analytics 
![Synapse Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/Synapse_archi.jpg?raw=true)
Azure Synapse brings together the best of SQL technologies used in enterprise data warehousing, **Spark technologies** used for big data, **Data Explorer** for log and time series analytics, **Pipelines** for data integration and ETL/ELT, and deep integration with other Azure services such as **Power BI**, **CosmosDB**, and **AzureML**.
##### Azure Synapse Analytics Managed Virtual Network	
Historical ways to address the data leakage risk in for Azure ressources:
- Azure managed services VNet injection
- Using Azure Firewall in a hub & spoke architecture
- Azure Private Endpoints
- Azure Service Endpoint Policies   

New approach for data services is using Managed VNet for most Azure Data products like **Power BI**, **Data factory**, **Purview**, **Azure ML**.
- When you create your Azure Synapse workspace, you can choose to associate it to a Microsoft Azure Virtual Network. The Virtual Network associated with your workspace is managed by Azure Synapse.
![Synapse Managed Vnet](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/azure-synapse-analytics-networking-managed-virtual-network-outbound-traffic.png?raw=true)
To provide more detail, the Synapse managed workspace will be hosted in Microsoft Azure subscriptions managed by MS engineering.
![Synapse Managed Vnet Option](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/managed_vnet.jpg?raw=true)
It is possible to connect other data sources to your Managed Synapse workspace through the managed private end point.
![Synapse Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/managed_vnet_integration.jpg?raw=true)
In order to secure your storage account, which must have public access so that Synapse engines can access it, in particular SQL Serverless, you need to configure the resource instances.
The resource instances need to be associated with your Synapse workspace in the "Security + networking" section of the storage account to Specify resource instances that will have access to your storage account based on their system-assigned managed identity.
![Synapse Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/Ressource_Instance.jpg?raw=true)
##### Data Exfiltration scenario
The diagrams below show possible scenario for data exfiltration, and we'll look at how we can respond next. 
![Synapse Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/DEP1.jpg?raw=true)
To tackle this scenario, there is a feature [Data Exfiltration Protection](https://learn.microsoft.com/en-us/azure/synapse-analytics/security/workspace-data-exfiltration-protection){:target="_blank"} (DEP) that enables additional restrictions on the ability of Azure Synapse Analytics to connect to other services – enabling you to further secure your Azure Synapse Analytics deployment. There are a couple of key things to know about DEP:
- DEP can only be enabled at Azure Synapse Analytics workspace creation and cannot be disabled at a later point. If you want to disable DEP, you will have to create a new Azure Synapse Analytics workspace and migrate all artifacts.

- DEP enables you to limit the communication from Azure Synapse Analytics. By requiring connections to other services to use managed private endpoints and to approved Azure AD tenants.

- DEP applies to all services within an Azure workspace including dedicated SQL pools, serverless SQL pools, Apache Spark pools and Pipelines.

As mentioned below, via the DEP feature it will now be impossible for a user to exfiltrate data from the Enterprise datalake to an external storage account via the SQL Serverless pool.
![Synapse Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/Synapse-DEP.jpg?raw=true)