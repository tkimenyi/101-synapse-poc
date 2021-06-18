# Azure Synapse Proof-of-Concept

![Synapse Analytics](https://raw.githubusercontent.com/osamaemumba/101-synapse-poc/main/images/synapse1.png)

## Table Of Contents

1. Introduction
2. Purpose
3. Prerequisites
4. Deployment
5. Post Deployment

## Introduction

This template deploys necessary resources to run an Azure Synapse Proof-of-Concept. 
Following resources are deployed with this template along with some RBAC role assignments:

- An Azure Synapse Workspace with batch data pipeline and other required resources
- An Azure Synapse SQL Pool
- An optional Apache Spark Pool
- Azure Data Lake Storage Gen2 account
- A new File System inside the Storage Account to be used by Azure Synapse
- A Logic App to Pause the SQL Pool at defined schedule
- A Logic App to Resume the SQL Pool at defined schedule
- A key vault to store the secrets

The data pipeline inside the Synapse Workspace gets Newyork Taxi trip and fare data, joins them and perform aggregations on them to give the final aggregated results. Other resources include datasets, linked services and dataflows. All resources are completely parameterized and all the secrets are stored in the key vault. These secrets are fetched inside the linked services using key vault linked service. The Logic App will check for Active Queries. If there are active queries, it will wait 5 minutes and check again until there are none before pausing

## Purpose

This template allows the Administrator to deploy a Proof-of-Concept environment of Azure Synapse Analytics with some pre-set parameters. This allows more time to focus on the Proof-of-Concept at hand and test the service.

## Prerequisites

- Owner role (oOtherwise Contributor + User Access Administrator roles) for the Azure Subscription the template being deployed in. This is for creation of a separate Proof-of-Concept Resource Group and to delegate roles necessary for this proof of concept.
- Fork out [this github repository](https://github.com/osamaemumba/101-synapse-poc) into your github account.

## Deployment

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fosamaemumba%2F101-synapse-poc%2Fmain%2Fazuredeploy.json)


## Post Deployment

Because the Synapse Workspace is using a Managed Virtual Network, the Storage Account requires a Managed Private Endpoint to be created into the Managed Virtual Network.

You can create a Managed private endpoint to your data source from Azure Synapse Studio. Select the Overview tab in Azure portal and select Launch Synapse Studio.

![Step 1](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-synapse-poc/images/9.png)

In Azure Synapse Studio, select the Manage tab from the left navigation. Select Managed Virtual Networks and then select + New.

![Step 2](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-synapse-poc/images/10.png)

Select the data source type. In this case, the target data source is an ADLS Gen2 account. Select Continue.

![Step 3](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-synapse-poc/images/11.png)

In the next window, enter information about the data source. In this example, we're creating a Managed private endpoint to an ADLS Gen2 account. Enter a Name for the Managed private endpoint. Provide an Azure subscription and a Storage account name. Select Create.

![Step 4](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-synapse-poc/images/12.png)

After submitting the request, you'll see its status. To verify the successful creation of your Managed private endpoint was created, check its Provisioning State. You may need to wait 1 minute and select Refresh to update the provisioning state. You can see that the Managed private endpoint to the ADLS Gen2 account was successfully created.

You can also see that the Approval State is Pending. The owner of the target resource can approve or deny the private endpoint connection request. If the owner approves the private endpoint connection request, then a private link is established. If denied, then a private link isn't established.

![Step 5](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-synapse-poc/images/13.png)

Further information can be found:

[Create a Managed private endpoint to your data source](https://docs.microsoft.com/en-us/azure/synapse-analytics/security/how-to-create-managed-private-endpoints)
