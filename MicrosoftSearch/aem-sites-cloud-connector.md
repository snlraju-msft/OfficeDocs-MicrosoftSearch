--- 

title: "Adobe Experience Manager(AEM) Sites Microsoft Graph connector" 
ms.author: rantang
author: ranran1998
manager: jecui
audience: Admin
ms.audience: Admin 
ms.topic: article 
ms.service: mssearch 
ms.localizationpriority: Medium 
search.appverid: 
- BFB160 
- MET150 
- MOE150 
description: "Set up the Adobe Experience Manager Sites Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot" 
ms.date: 01/16/2025
---

# Adobe Experience Manager Sites Microsoft Graph connector (preview)

With the Adobe Experience Manager Sites Microsoft Graph connector, your organization can index published webpages of your AEM Sites. After you configure the connector and index content from AEM Sites, end users can search for those published webpages in Microsoft Copilot and from any Microsoft Search client. 

This article is for Microsoft 365 administrators or anyone who configures, runs, and monitors an Adobe Experience Manager Sites Microsoft Graph connector. 

>[!NOTE]
>The Adobe Experience Manager Sites connector is in public preview. If you wish to get access to try it, you need to enable [Targeted Release](/microsoft-365/admin/manage/release-options-in-office-365?view=o365-worldwide#set-up-the-release-option-in-the-admin-center) ring for your Admin account.

## Capabilities
- Index published webpages of your AEM Sites.
- Supports ingestion filters based on page paths, allowing for exact matching and phrase matching using regular expressions.
- Customize your crawl frequency.
- Create workflows using this connection and plugins from Microsoft Copilot Studio.  
- Use [Semantic search in Copilot](semantic-index-for-copilot.md) to enable users to find relevant content.

## Limitations
- Does not index comments.
- Does not crawl user identities and access permissions. All published webpages indexed using the Adobe Experience Manager Sites Microsoft Graph connector are visible to all Microsoft 365 users in your tenant, from Microsoft Search or Copilot.   

## Prerequisites
- You must be the **search admin** for your organization's Microsoft 365 tenant.
- **Adobe Experience Cloud Instance URL**: To connect to your Adobe Experience Manager Sites data, you need your organization's Adobe Experience Cloud instance author environment URL and publish environment URL.
  Your organization's Adobe Experience Cloud instance author environment URL typically looks like: `https://author-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.<REGION>.adobeaemcloud.com`.
  Your organization's Adobe Experience Cloud instance publish environment URL typically looks like: `https://publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.<REGION>.adobeaemcloud.com`. 
- **Adobe Experience Cloud Account**: To connect to Adobe Experience Cloud and allow the Adobe Experience Manager Sites Microsoft Graph connector to update published webpages and metadata regularly, you need a technical account of your Adobe Experience Manager Sites with the credentials to access published webpages and metadata. The technical account is the secure, service-based account for external access to Adobe Experience Manager Sites. Find more details [here](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis#generate-a-jwt-token-and-exchange-it-for-an-access-token).

## Get started

### 1. Display name 
A display name is used to identify each citation in Copilot, helping users easily recognize the associated file or item. Display name also signifies trusted content. The display name is also used as a [content source filter](/MicrosoftSearch/custom-filters#content-source-filters). A default value is present for this field, but you can customize it to a name that users in your organization recognize.

### 2. Adobe Experience Cloud Instance URL
To correctly access and update data from the Adobe Experience Manager Sites, both the author and publish environment URLs are essential.   

### 3. Authentication Type
Authentication Type - We support the technical account for Adobe Experience Cloud. To enable and configure the technical account for Adobe Experience Manager Sites, please find more details [here](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis#generate-a-jwt-token-and-exchange-it-for-an-access-token).

### 4. Staged rollout to a limited audience
Deploy this connection to a limited user base if you want to validate it in Copilot and other Search surfaces before expanding the rollout to a broader audience.

At this point, you are ready to create the connection for AEM Sites. You can click the **Create** button to publish your connection and index published web pages from your AEM Sites. 

For other settings, like Access Permissions, Data inclusion rules, Schema, Crawl frequency, etc., we set defaults based on what works best with AEM Sites data. You can see the default values below: 

**Page** | **Settings** | **Default values**
--- | ---- | ---
Users | Access permissions | All published pages or posts indexed using the Adobe Experience Manager Sites Microsoft Graph connector are visible to all M365 users in your tenant, from Microsoft Search or Copilot.
Content | Index content | All published pages are selected by default. 
Content | Manage properties | To check default properties and their schema, [click here](#content).
Sync | Incremental crawl | Frequency: Every 15 mins
Sync | Full crawl | Frequency: Every day

If you want to edit any of these values, you need to choose the **Custom setup** option. 

## Custom setup 

Custom setup is for those admins who want to edit the default values for settings. Once you click the **Custom setup** option, you should see three other tabs – **Users**, **Content**, and **Sync**. 

### Users 

**Access permissions**

Currently only published webpages from your AEM Sites are indexed. All data indexed using the Adobe Experience Manager Sites Microsoft Graph connector is visible to all Microsoft 365 users in your tenant, from Microsoft Search or Copilot.

### Content 

**Content ingestion filters**   

You can choose to include or exclude certain content paths.  

- Content paths that should be fetched: Only support input exact paths. A valid content path must contain at least two levels, with '/content' as the root path. 

- Content paths that should not be fetched: Only support input Java regular expression for paths. For information about writing regular expressions, see Regular Expression Language Quick Reference. The priority of excluding content paths is higher than that of including content paths. 

Use the preview results button to verify the sample values of the selected properties and filters. 

**Manage properties**

Here, you can check available properties from your Adobe Experience Manager Sites. Assign a schema to the property (define whether a property is searchable, queryable, retrievable, or refinable), change the semantic label, and add an alias to the property. Properties that are selected by default are listed below. 

| **Source property** | **Semantic label**       | **Description**                                                                 | **Schema**                  |
|----------------------|--------------------------|---------------------------------------------------------------------------------|-----------------------------|
| CreatedBy           | Created by              | Date and time that the item was created in the data source                      | Query, Retrieve, Search     |
| CreatedTime         | Created date time       | Date and time that the item was created in the data source                      | Query, Retrieve             |
| Description         | Description             | A brief summary of the page's content                                           | Query, Retrieve             |
| HtmlContent         | Content                 | The content of static webpages, not available for dynamic webpages              | Search                      |
| Title            |                          |  The title of the items                                                                               | Query, Retrieve             |
| LastModifiedBy      | Last modified by        | Name of the person who most recently edited the item in the data source         | Search, Query, Retrieve     |
| Link                | URL                     | The target URL of the item in the data source                                   | Query, Retrieve             |
| ModifiedTime        | Last modified date time | Date and time the item was last modified in the data source                     | Query, Retrieve             |
| Navigation Title    | Navigation title        | Navigation title is the title displayed in site navigation menus                | Query, Retrieve             |
| PublishedBy         | Published by            | Name of the person who published the item in the data source                    | Query, Retrieve             |
| PublishedTime       | Published date time     | Date and time the item was published in the data source                         | Query, Retrieve             |
| Subtitle            | Subtitle                | The subtitle of the items                                                      | Query, Retrieve             |
| PageTitle               | Title               | The pagetitle of the webpages                                                          | Query, Retrieve             |
| Tags                | Tags                    | Tags defined in AEM Sites metadata. In AEM, tags are organized hierarchically   | Query, Retrieve, Search     |


### Sync 

You can configure full and incremental crawls based on the scheduling options present here. By default, incremental crawl is set for every 15 minutes, and full crawl is set for every day. If needed, you can adjust these schedules to fit your data refresh needs.

## Troubleshooting
After publishing your connection, you can review the status under the **Data sources** tab in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md).

If you have issues or want to provide feedback, contact [Microsoft Graph | Support](https://developer.microsoft.com/en-us/graph/support).
