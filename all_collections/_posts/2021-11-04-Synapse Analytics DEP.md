---
layout: post
title: Synapse Analytics Data Exfiltration Protection
date: "2023-05-09"
categories: ["Data", "Analytics", "Security"]
---
For my second post, I'm going to focus on a subject that is of major concern to many of my clients, data security inside their Data platform.

Beyond the network security aspects, what preoccupies companies are the aspects around data exfiltration which is a very considered threat by the security team.We will address this topic through the usage of [Microsoft Azure Synapse Analytics](https://azure.microsoft.com/en-us/products/synapse-analytics/){:target="_blank"} ,an enterprise analytics service.

Before briefly presenting the Synapse architecture and the possible protection capabilities, you will find below the definition of what is data exfiltration:
- Data exfiltration, aka data leakage, aka data extrusion, occurs when sensitive data is exfiltrated to locations outside of an 
In fact, there are two types of data exfiltration threats: **attacks from outside the organization**, and **malicious insiders stealing their organization**.
- In the public cloud business, data exfiltration usually stands for the second scenario: a malicious insider, who has legitimate access to sensitive data, is exfiltrating it to locations outside of the organization.
##### Presentation of Synapse Analytics 
![Synapse Architecture](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/Synapse_archi.jpg?raw=true)
Azure Synapse brings together the best of SQL technologies used in enterprise data warehousing, **Spark technologies** used for big data, **Data Explorer** for log and time series analytics, **Pipelines** for data integration and ETL/ELT, and deep integration with other Azure services such as **Power BI**, **CosmosDB**, and **AzureML**.
Historical ways to address the data leakage risk:
- Azure managed services VNet injection
- Using Azure Firewall in a hub & spoke architecture
- Azure Private Endpoints
- Azure Service Endpoint Policies		
				
New approach for data services is using Managed VNet:
- Power BI, Data factory, Synapse, Purview, Azure ML
