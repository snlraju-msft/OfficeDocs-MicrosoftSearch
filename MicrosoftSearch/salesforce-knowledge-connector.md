--- 
title: "Salesforce Knowledge connector for Microsoft Search and Microsoft 365 Copilot" 
ms.author: rerabo
author: vivg
manager: igala
audience: Admin
ms.audience: Admin 
ms.topic: article 
ms.service: mssearch 
ms.localizationpriority: medium 
search.appverid: 
- BFB160 
- MET150 
- MOE150 
description: "Set up the Salesforce Knowledge Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot" 
ms.date: 12/05/2024
---

# Salesforce Knowledge Microsoft Graph connector

The Salesforce Knowledge Microsoft Graph connector allows your organization to index articles from Salesforce Knowledge. After you configure the connector, end users can search for Knowledge articles from Salesforce in Microsoft Copilot and from any Microsoft Search client. 

This documentation is for Microsoft 365 administrators or anyone who configures, runs, and monitors a Salesforce Knowledge Microsoft Graph connector. 

>[!NOTE]
>The Salesforce Knowledge Microsoft Graph connector is in preview. If you wish to get early access to try it, sign up using [this form](https://forms.office.com/r/JniPmK5bzm).

## Capabilities
- Index Salesforce Knowledge articles.
- Enable users within the company to ask questions in natural language using Copilot and receive answers based on articles in Salesforce. Examples:
   - What are the steps for processing a refund?
   - What is the escalation procedure for high-priority cases?
   - What are the latest updates to our product warranty policy?
- Use [Semantic search in Copilot](semantic-index-for-copilot.md) to enable users to find relevant content based on keywords, personal preferences, and social connections.

## Limitations
- The connector doesn't support ACLs (access control lists). All the data indexed using the Salesforce Knowledge Microsoft Graph connector is visible to all Microsoft 365 users in your tenant, accessible through Microsoft Search or Microsoft 365 Copilot.

## Prerequisites

>[!NOTE]
>Make sure that the Salesforce account used to log in for the Salesforce Knowledge Microsoft Graph connector is the same as the account already logged into Salesforce.

To connect to your Salesforce instance, you need your Salesforce instance URL, the client ID, and the client secret for OAuth 2.0 authentication. The following steps explain how you or your Salesforce administrator can get this information from your Salesforce account:

- Log in to your Salesforce instance and go to **Setup**.
- Navigate to Apps -> App Manager.
- Select **New connected app**.
- Complete the API section as follows:
    - Select the checkbox for **Enable Oauth settings**.
    - Specify the Callback URL as: For **M365 Enterprise**: `https://gcs.office.com/v1.0/admin/oauth/callback`, for **M365 Government**: `https://gcsgcc.office.com/v1.0/admin/oauth/callback`
    - Select these required OAuth scopes.
        - Access and manage your data (API).
        - Perform requests on your behalf at any time (refresh_token, offline_access).
    - Select the checkbox for **Require secret for web server flow**.
    - Save the app.
      > [!div class="mx-imgBorder"]
      > [![Screenshot that shows API section in Salesforce instance after admin has entered all required configurations listed above.](media/salesforce-connector/sf1.png)](media/salesforce-connector/sf1.png#lightbox)

- Copy the consumer key and the consumer secret. This information is used as the client ID and the client secret when you configure the connection settings for your Salesforce Knowledge Microsoft Graph connector in the Microsoft 365 admin portal.
  > [!div class="mx-imgBorder"]
  > [![Screenshot that shows results returned by API section in Salesforce instance after admin has submitted all required configurations. Consumer Key is at top of left column and Consumer Secret is at top of right column.](media/salesforce-connector/clientsecret.png)](media/salesforce-connector/clientsecret.png#lightbox)
- Before closing your Salesforce instance, follow these steps to ensure that refresh tokens don't expire:
    - Go to Apps -> App Manager.
    - Find the app you created and select the drop-down on the right. Select **Manage**.
    - Select **edit policies**.
    - For the refresh token policy, select **Refresh token is valid until revoked**.
       > [!div class="mx-imgBorder"]
       > [![Screenshot that shows select the Refresh Token Policy named "Refresh token is valid until revoked ".](media/salesforce-connector/oauthpolicies.png)](media/salesforce-connector/oauthpolicies.png#lightbox)

You can now use the [Microsoft 365 Admin Center](https://admin.microsoft.com/) to complete the rest of the setup process for your Salesforce Knowledge Microsoft Graph connector.
     
## Get Started

### 1. Display name 
A display name is used to identify each citation in Copilot, helping users easily recognize the associated file or item. Display name also signifies trusted content. Display name is also used as a content source filter. A default value is present for this field, but you can customize it to a name that users in your organization recognize.

### 2. Salesforce Knowledge URL
Use your organization’s Salesforce Knowledge Instance URL. This URL is the specific web address used to access and interact with Salesforce Knowledge's API services for data retrieval, which typically looks like `https://[COMPANY_NAME].my.salesforce.com`

### 3. Authentication Type
For Salesforce Knowledge Microsoft Graph connector, use OAuth 2.0 for authentication. 

To authenticate, enter the Client ID and Client Secret. The Client ID is a unique identifier assigned to your application for making requests to the Salesforce Knowledge API. The Client Secret is a confidential key used alongside the Client ID to securely authenticate your application with the Salesforce Knowledge API. 
 
### 4. Roll out to limited audience
Deploy this connection to a limited user base if you want to validate it in Copilot and other Search surfaces before expanding the rollout to a broader audience. To know more about limited rollout, see [staged rollout](staged-rollout-for-graph-connectors.md).

At this point, you're ready to create the connection for Salesforce Knowledge. You can click on the "Create" button to publish your connection and index posts from your Salesforce Knowledge account.

## Custom Setup

Custom setup is for admins who want to edit the default values for settings. Once you click on the 'Custom Setup' option, you see three other tabs: Users, Content, and Sync.

### Users
Currently, articles from your organization’s Salesforce Knowledge instance are indexed. All the data indexed using the Salesforce Knowledge Microsoft Graph connector is visible to all Microsoft 365 users in your tenant, accessible through Microsoft Search or Copilot. 
 
### Content

**Manage properties**

Here, you can add or remove available properties from your Salesforce Knowledge data source, assign a schema to the property (define whether a property is searchable, queryable, retrievable, or refinable), change the semantic label and add an alias to the property. Properties that are selected by default are listed below:

**Source Property** | **Semantic Label** |**Description**| **Schema**
--- | ---- | --- | ---
ArticleId | | | Query, Retrieve 
ArticleNumber | | The unique number automatically assigned to the article when it's created. | Query, Retrieve 
ArticleType | | The type or category of the knowledge article (for example, FAQ, Support Article, How-To). | Retrieve 
CondensedBody | | Includes the Full content or main body of the article | Retrieve, Search 
CreatedById | | | Query, Retrieve, Search 
CreatedByName | createdBy | The user who initially created the article. | Query, Retrieve, Search 
CreatedDate | createdDateTime | Timestamp of when the article was initially created. | Query, Retrieve 
IconUrl | iconUrl | | Retrieve 
Language | | The language in which the article is written. | Retrieve 
LastModifiedById | | | Query, Retrieve, Search 
LastModifiedByName | lastModifiedBy | The user who last updated the article. | Query, Retrieve, Search 
LastModifiedDate | lastModifiedDateTime | Timestamp of the most recent update to the article. | Query, Retrieve 
LastPublishedDate | | The date when the article was last published. | Query, Retrieve 
LastPublishedVersionId | | | Query, Retrieve 
Summary | | A brief overview or abstract of the article's content. | Search 
Title | title | The main headline or title of the article. | Query, Retrieve, Search 
Url | url | Link to the article in Salesforce Knowledge. | Retrieve 
UrlName | | A unique URL-friendly name generated for the article. | Query, Retrieve 

### Sync

The refresh interval determines how often your data is synced between the data source and the Salesforce Knowledge Microsoft Graph connector index. There are two types of refresh intervals - full crawl and incremental crawl. For more information, see [refresh settings](configure-connector.md#guidelines-for-sync-settings).

## Troubleshooting
After publishing your connection, you can review the status under the **Data Sources** tab in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md). 

If you have issues or want to provide feedback, contact [Microsoft Graph | Support](https://developer.microsoft.com/en-us/graph/support).
