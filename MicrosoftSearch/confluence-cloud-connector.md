---
ms.date: 11/16/2024
title: "Confluence Cloud Microsoft Graph connector"
ms.author: mansipakhale
author: mansipakhale
manager: harshkum
audience: Admin
ms.audience: Admin
ms.topic: article
ms.service: mssearch
ms.localizationpriority: Medium
search.appverid:
- BFB160
- MET150
- MOE150
description: "Set up the Confluence Cloud Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot"
---

# Confluence Cloud Microsoft Graph connector

The Confluence Cloud Microsoft Graph connector allows your organization to index Confluence content. After you configure the connector and index data from the Confluence site, end users can search for those contents in Microsoft Search and Microsoft 365 Copilot.

This article is intended for Microsoft 365 administrators who are responsible for configuring, running, and monitoring the Confluence Cloud Microsoft Graph connector. It supplements the general instructions provided in setting up Microsoft Graph connectors in the Microsoft 365 admin center.

## Benefits
- **Enhanced search capabilities**: Users can ask natural language questions about Wiki content in Copilot, such as:
   - Summarize the architecture document </br>
   - How to get access to a portal </br> 
- **Semantic search support**: Users can perform natural language queries for accurate responses. </br>

## Prerequisites
1. You must be the admin for your organization's Microsoft 365 tenant and the admin for your organization's Confluence site.
2. **Authentication**: Ensure that you have authentication credentials with the right access. 

## Limitations
- Doesn't index attachment files or comments.

>[!IMPORTANT]
> * Atlassian is deprecating a set of Confluence cloud APIs (V1 version) and releasing new APIs (V2 version). You may read about their announcements [here](https://developer.atlassian.com/cloud/confluence/changelog/#CHANGE-864) or [here](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fcommunity.developer.atlassian.com%2Ft%2Frfc-19-deprecation-of-confluence-cloud-rest-api-v1-endpoints%2F71752&data=05%7C01%7Cvivg%40microsoft.com%7Cb8d049f07c3544de6b2c08dbe98b2a02%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C638360556187110970%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=DIw8xhEwulo59mAm8T0f0TTKvbtRr4tIMTMpQYgPDDQ%3D&reserved=0). Some of these deprecating v1 APIs are used by the connector for **OAuth connections** only. Hence, post this change your existing Confluence connections may stop working. This change is scheduled for Jan’24.
> * The change to migrate to new v2 APIs was released to all customers in **December 2023**. Post this release, your existing connections need to be reauthenticated. The new v2 APIs also require some more scopes (as compared to previous v1 APIs) which need to be provided during re-authentication. A new set of scopes required (complete list) – `read:group:confluence`, `read:user:confluence`, `read:content-details:confluence`, `Read:space:confluence`, `Read:permission:confluence`, `read:audit-log:confluence`, `read:content.metadata:confluence` and `read:page:confluence`.

## Get started
This video provides a step-by-step guide on adding the Confluence Cloud Microsoft Graph connector.
> [!VIDEO https://www.youtube-nocookie.com/embed/9WXQt1JykXo]

### 1. Display name

A Display name is used to identify each reference in Copilot, helping users easily recognize the associated file or item. Display name also signifies trusted content. Display name is also used as a [content source filter](./custom-filters.md#content-source-filters). A default value is present for this field, but you can customize it to a name that users in your organization recognize.

### 2. Confluence Cloud URL

To connect to your Confluence site, use your site URL. A Confluence cloud site URL typically looks like *https://<organization_name>.atlassian.net/*. 

### 3. Authentication type

To authenticate and synchronize content from Confluence On-prem, choose **one of two** supported methods:<br> 
>[!TIP]
>Make sure the service **account has view access** to the Confluence content you want to index.

   **a. Basic authentication** <br>
To authenticate using basic auth, enter your username (usually your email) and API token. To help generating an API token, see Atlassian's [guide](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/).

   **b. OAuth 2.0 (recommended)** <br> 
Register an app in Confluence Cloud so that the Microsoft Search app and Microsoft 365 Copilot can access the instance. To learn more, see Atlassian Support documentation on how to [Enable OAuth 2.0](https://developer.atlassian.com/cloud/confluence/oauth-2-3lo-apps/#enabling-oauth-2-0--3lo-).

The following steps provide guidance on how to register the app:

1. Sign in to [Atlassian Developer console](https://developer.atlassian.com/console/myapps/) with your Atlassian Confluence admin account.
2. Click on **Create** and select `OAuth 2.0 integration`.
3. Provide an appropriate name for the application and create the new app.
4. Navigate to `Permissions` from the navigation pane on the left. Click **Add** for `Confluence API`. Once added, click on **Configure**, and **Edit scopes** and select the following scopes.

   | **Scope name** | **Code** | **Description** |
   | ------------ | ------------ | ------------ |
   | View content details | `read:content-details:confluence` | Crawl content satisfying criteria.
   | View groups | `read:group:confluence` | To access group permissions of content.
   | View user details | `read:user:confluence` | To access individual user details to support permissions.

5. Click **Save**.
6. Navigate to `Authorization` from the navigation pane on the left. Add the callback URL, for **Microsoft 365 Enterprise**: `https://gcs.office.com/v1.0/admin/oauth/callback`, for **Microsoft 365 Government**: `https://gcsgcc.office.com/v1.0/admin/oauth/callback` and save the changes.
7. Navigate to **Settings** from the navigation pane on the left. You get the **Client ID** and **Secret** from this page.

   Complete the connection settings step using the **Client ID** and **Secret**.

### 6. Rollout to a limited audience

Deploy this connection to a limited user base if you want to validate it in Copilot and other Search surfaces before expanding the rollout to a broader audience. To know more about the limited rollout, click [here](./staged-rollout-for-graph-connectors.md).

At this point, you are ready to create the connection for ServiceNow Knowledge. You can click the "Create" button and the Confluence Cloud Microsoft Graph connector starts indexing page from your Confluence account.

For other settings, like Access Permissions, Data inclusion rules, Schema, Crawl frequency etc., We set defaults based on what works best with Confluence data. The default values are as follows:

|**Users** |&nbsp;|
|----|---|
|Access permissions|_Only people with access to content in Data source._|
|Map Identities|_Data source identities mapped using Microsoft Entra IDs._|

|**Content**|&nbsp;|
|---|---|
|Include/Exclude space|_All_|
|Manage Properties|_To check default properties and their schema, click here_|

|**Sync**|&nbsp;|
|---|---|
|Incremental Crawl|_Frequency: Every 15 mins_|
|Full Crawl|_Frequency: Every Day_|

If you want to edit any of these values, you need to choose the `Custom Setup` option.

## Custom setup

Custom setup is for those admins who want to edit the default values for settings listed in the default table. Once you click on the `Custom Setup` option, you see three more tabs – Users, Content, and Sync.

### Users

**Access permissions**

Confluence Cloud Microsoft Graph connector supports search permissions visible to **Everyone** or **Only people with access to this data source**. If you choose **Everyone**, indexed data appears in the search results for all users. If you choose **Only people with access to this data source**, indexed data appears in the search results for users who have access to them. 
In Confluence Cloud, security permissions for users and groups are defined using space permissions and page restrictions. Page-level restrictions, if present, take precedence over space permissions.

If there are no page restrictions, the connector checks for space-level permissions - 
* In case the space has 'anonymous users' access enabled, the content is visible to all users within your tenant.
* In case 'anonymous access' isn't enabled, the space-level permissions are not honored.
* In case space level permissions are not defined, the content is not visible to any user in your tenant.

>[!IMPORTANT]
>Permissions are managed at the space and page level only, and parent page permissions are not taken into consideration.

If you choose **Only people with access to this data source**, you need to further choose whether your Confluence site has Microsoft Entra ID provisioned users or non-AAD users.

To identify which option is suitable for your organization:

1. Choose the **Microsoft Entra ID** option if the email ID of Confluence users is **same** as the UserPrincipalName (UPN) of users in Microsoft Entra ID.
2. Choose the **non-AAD** option if the email ID of Confluence users is **different** from the UserPrincipalName (UPN) of users in Microsoft Entra ID.

>[!NOTE]
>
> * If you choose Microsoft Entra ID as the type of identity source, the connector maps the email IDs of users obtained from Confluence directly to UPN property from Microsoft Entra ID.
> * If you chose "Non-AAD" for the identity type see [Map your non-Azure AD Identities](map-non-aad.md) for instructions on mapping the identities. You can use this option to provide the mapping regular expression from email ID to UPN.
> * Updates to users or groups governing access permissions are synced in full crawls only. Incremental crawls do not currently support the processing of updates to permissions.


### Content

**Include or exclude data which you want to index**

With a Confluence Query Language (CQL) string, you can specify conditions for syncing pages. It's like a **Where** clause in a **SQL Select** statement. For example, you can choose to index only the pages that were modified in the last two years. To learn about creating your own query string, see [Advanced Searching using CQL](https://developer.atlassian.com/server/confluence/advanced-searching-using-cql/). The connector indexes all blogs and pages by default.

>[!TIP]
>You may use the CQL filter to index **content modified after a certain time** using, *lastModified >= "2018/12/31"*

Use the preview results button to verify the sample values of the selected properties and CQL string.

**Manage properties**

In this step, you can add or remove available properties from your Confluence data source. A few properties are selected by default.
*The list of properties that you select here, can impact how you can filter, search and view your results in Microsoft 365 Copilot.*


Source property | Label | Description
|:--- |:--- |:---|
Authors   | `authors` | Name of people who participated/collaborated on the item in the data source.|
CreatedByName  | `createdBy` | Name of the person who most recently edited the item in the data source.|
CreatedOn  | `createdDateTime` | Date and time that the item was created in the data source.|
IconUrl  | `iconUrl` | The associated icon URL of the item.|
Title   | `title` | The title of the item that you want to be shown in search and other experiences.|
UpdatedByName  | `lastModifiedBy` | Name of the person who most recently edited the item in the data source.|
UpdatedOn  | `lastModifiedDateTime` | Date and time the item was last modified in the data source.|
Url  | `url` | The target URL of the item in the data source.

**Preview data**

Use the preview results button to verify selected properties and filters.

### Sync

The refresh interval determines how often your data is synchronized between the data source and the Confluence Cloud Microsoft Graph connector index. There are two types of refresh intervals – full crawl and incremental crawl. For more details, click [here](configure-connector.md#guidelines-for-sync-settings).
You can change the default values of refresh interval from here if you want to.

### Review and test your connection

- For testing, you can choose [publish to limited audience](./staged-rollout-for-graph-connectors.md#modify-or-stop-staged-rollout)
- Search and validate your indexed content and permissions using [Index browser](./connectors-index-search.md)
- You may find answers to common questions in our [FAQ section](./frequently-asked-questions.md)

For MS Search, if you need to customize the search results page. To learn about customizing search results, see [Customize the search results page](customize-search-page.md).

If you have issues or want to provide feedback, contact [Microsoft Graph | Support](https://developer.microsoft.com/graph/support).
