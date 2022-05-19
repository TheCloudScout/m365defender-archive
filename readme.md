# Introduction

**This repository is referred to by one of my articles on Medium**

> Once a security incident is raised, and triage begins, our security analysts often take very similar steps during their investigations.

> Automation might be able to help to shorten investigations, bring down ‘alert fatigue’ and in turn make sure your analysts have more time to make your organization even safer.

> In this article I’ll demonstrate how to leverage VirusTotal’s services to automatically scan all of your file hashes and IP addresses that come through Microsoft Sentinel as entities in their respective incidents. The scan results are then automatically attached to the security incidents so that your analysts see the reputation and other details in an instant, without having to go out and retrieve this information themselves.

https://medium.com/@koosg/automate-your-sentinel-incident-triage-a316d229f2fc

<br>

# Contents

### logicapp-enrich-incident-virustotal.template.json

This is the ARM template you can use to deploy the Logic App, together with all of the other required resources, as it was used as an example in the article.

The template will deploy the folowinf resources:

| Resource Type | Details |
| ---- | ---- |
| Microsoft.Operationalinsights/workspaces | The Log Analytics workspace where all logs from Microsoft 365 Defender will be archived |
| Microsoft.OperationalInsights/workspaces/tables | All custom table (17x) definitions where the schema is properly determined as well as the Basic tier |
| Microsoft.Insights/dataCollectionEndpoints | Data Collection Endpoint used for custom log ingestion. This is where the Logic App sends the data to. |
| Microsoft.Insights/dataCollectionRules | One Data Collection Rule per table name to work around maximum 'stream' limit of ten per DCR. Each DCR also contains the transformation query. |
| Microsoft.Storage/storageAccounts | Storage Account which will be used to temporarily store de logs from Microsoft 365 Defender via the streaming API. |
| Microsoft.Storage/storageAccounts/blobServices | Some blob storage configurations like network ACLs and data retention settings. |
| Microsoft.Logic/integrationAccounts | An Integration Account is required to be able to run Javascript tasks within Logic Apps. |
| Microsoft.Web/connections | Logic App Connector to communicate with the Storage Account via system-assigned Managed Identity. |
| Microsoft.Logic/workflows | The Logic App workflow where als retrieval, parsing, and sending of data will take place every day at 02:00 CET. |
| Microsoft.Authorization/roleAssignments | The system-assigned Managed Identity from the Logic App need to be granted two specific RBAC roles to retrieve data from the Storage Account and send data to the Data Collection Endpoint. |

To deploy this set of resources, click the button below.

<br>

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FTheCloudScout%2Fincident-enrich-virustotal%2Fmain%2Flogicapp-enrich-incident-virustotal.template.json)

<br>