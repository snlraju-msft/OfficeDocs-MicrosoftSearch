--- 
ms.date: 06/11/2020
title: "Azure DevOps Work Items Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot" 
ms.author: vivg
author: vivg
manager: harshkum
audience: Admin
ms.audience: Admin 
ms.topic: article 
ms.service: mssearch 
ms.localizationpriority: medium 
description: "Set up the Azure DevOps Work Items Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot" 
---

# Azure DevOps Work Items Microsoft Graph connector

The Azure DevOps Work Items Microsoft Graph connector allows your organization to index work items in its instance of the Azure DevOps service. After you configure the connector, end users can search for work items from Azure DevOps in Microsoft Search and Microsoft 365 Copilot.

This article is for Microsoft 365 administrators or anyone who configures, runs, and monitors an Azure DevOps Work Items Microsoft Graph connector.

>[!IMPORTANT]
>The Azure DevOps Work Items Microsoft Graph connector supports only the Azure DevOps cloud service. Azure DevOps Server 2019, TFS 2018, TFS 2017, TFS 2015, and TFS 2013 are not supported by this connector.

## Capabilities
- Index Work Items from Azure DevOps
- Enable your end users to ask questions related to work items.
- Use [Semantic search in Copilot](semantic-index-for-copilot.md) to enable users to find relevant content based on keywords, personal preferences, and social connections.

## Limitations
- The connector only indexes one ADO organization per connection.

## Prerequisites
- You must be the **search admin** for your organization's Microsoft 365 tenant.
- To allow the connector to connect to your Azure DevOps organization, you must enable **Third-party application access via OAuth**. Refer Azure DevOps documentation to [manage security policies](/azure/devops/organizations/accounts/change-application-access-policies?view=azure-devops#manage-a-policy&preserve-view=true) to learn more.

    ![Screenshot that shows third-party application access via OAuth.](media/ado-workitems-connector-security-policies.png)

- **Service Account**: To connect to Azure DevOps and allow the Microsoft Graph Connector to update work items regularly, you need a service account with the following permissions granted to it.

    | Permission name | Permission type | Required for |
    | ------------ | ------------ | ------------ |
    | View project-level information | [Project permission](/azure/devops/organizations/security/permissions?view=azure-devops&tabs=preview-page#project-level-permissions&preserve-view=true) | Crawling Azure DevOps Work Items. This permission is **mandatory** for the projects that need to be indexed. |
    | View analytics| [Project permission](/azure/devops/organizations/security/permissions?view=azure-devops&tabs=preview-page#project-level-permissions&preserve-view=true) | Crawling Azure DevOps Work Items. This permission is **mandatory** for the projects that need to be indexed. |
    | View work items in this node| [Area path](/azure/devops/organizations/security/permissions?view=azure-devops&tabs=preview-page#area-path-object-level&preserve-view=true) | Crawling Work Items in an area path. This permission is **optional**. Only those area paths are crawled for which the user account has permission. |

    >[!IMPORTANT]
    >The service account must have **Basic** access level. To learn more about access levels in Azure DevOps, read [supported access levels](/azure/devops/organizations/security/access-levels).

## Get Started

[![Screenshot that shows connection creation screen for Microsoft Graph Connector for Azure DevOps Work Items.](media/ado-workitems-create-page.png)](media/ado-workitems-create-page.png#lightbox)

### 1. Display name 
A display name is used to identify each citation in Copilot, helping users easily recognize the associated file or item. Display name also signifies trusted content. Display name is also used as a [content source filter](/MicrosoftSearch/custom-filters#content-source-filters). A default value is present for this field, but you can customize it to a name that users in your organization recognize.

### 2. ADO Organization
The Azure DevOps connector allows indexing of one organization per connection. To connect to your Azure DevOps service, provide the name of the organization to be indexed.

### 3. Authentication type

To authenticate and sync work items from Azure DevOps, choose **one of the two** supported methods:<br>

> [!IMPORTANT]
> - [Microsoft Entra ID OAuth](/azure/devops/integrate/get-started/authentication/oauth?preserve-view=true&view=azure-devops) is the recommended OAuth mechanism.
> - [Azure DevOps OAuth](/azure/devops/integrate/get-started/authentication/oauth?preserve-view=true&view=azure-devops) is the legacy authentication mechanism, not being actively invested upon.

#### a. Microsoft Entra ID OAuth

**Ensure your ADO Organization is connected to Microsoft Entra**

The Azure DevOps Graph connector only indexes content from an ADO organization connected with Microsoft Entra of your tenant. To ensure that your ADO organization is connected with Microsoft Entra account, use the following steps. 

1. Navigate to [Azure DevOps](https://dev.azure.com/) and select the required organization.
2. Select `Organization settings`.
3. On the left navigation pane, select `Microsoft Entra` under the 'General' header.
4. Ensure that the organization is connected to your tenant's Microsoft Entra account.

**Create an app on Microsoft Entra ID**

1. Go to the [Azure portal](https://portal.azure.com) and sign in with admin credentials for the tenant.
2. Navigate to **Microsoft Entra ID** -> **Identity** -> **Applications** -> **App registrations** from the navigation pane and select **New registration**.
3. Provide a name for the app and select **Register**.
4. Make a note of the Application (client) ID. This ID is used to grant the Microsoft Entra app access to projects in the ADO organization.
5. Open **API permissions** from the navigation pane and select **Add a permission**.
6. Select **Azure DevOps** and then **Delegated permissions**.
7. Search for the following permissions under **vso** and select **Add permissions**. <br>
    a. **vso.analytics** - Analytics (read) <br>
    b. **vso.graph** - Graph (read) <br>
    c. **vso.identity** - Identity (read) <br>
    d. **vso.project** - Project and team (read) <br>
    e. **vso.variablegroups_read** - Variable Groups (read) <br>
    f. **vso.work** - Work items (read) <br>
9. Select **Grant admin consent for [TenantName]** and confirm by selecting **Yes**.
10. Check that the permissions are in the "**Granted**" state.
11. Open **Authentication** from the navigation pane. Select `Add a platform` and choose `Web`. Add one of the following URIs under "Redirect URIs":
    - For **M365 Enterprise**: https://<span>gcs.office.</span>com/v1.0/admin/oauth/callback
    - For **M365 Government**: https://<span>gcsgcc.office.<span>com/v1.0/admin/oauth/callback
12. Under **Implicit grant and hybrid flows**, check the option for `ID tokens (used for implicit and hybrid flows)` and click **Configure**.
13. From the navigation pane, select **Certificates and secrets** under **Manage**.
14. Select **New Client secret** and select an expiry period for the secret. Copy the generated secret (Value) and save it because it is not shown again.
15. Use this Client secret and the application ID to configure the connector.

**Grant the Microsoft Entra app access to projects in the ADO organization**

You need to provide the Microsoft Entra app the necessary access to the projects which need to be indexed using the following steps:

1. Navigate to [Azure DevOps](https://dev.azure.com/) and select the required organization.
2. Select `Organization settings`.
3. On the left navigation pane, select `Users` under the 'General' header.
4. Select `Add users`.
5. Copy the Application (client) ID obtained from the app to "Users or Service Principals".
6. Grant the `Basic` access level and select the projects to allow access to index. Also add to the `Project Reader` Azure DevOps group (or equivalent) to ensure access. De-select the option to send email invitation to users. Click add.

#### b. Azure DevOps OAuth

To connect to your Azure DevOps instance, you need your Azure DevOps [organization](/azure/devops/organizations/accounts/create-organization) name, its App ID, and client secret for OAuth authentication.

**Register an app**

Register an app in Azure DevOps so that the Microsoft Search app and Microsoft 365 Copilot can access the instance. To learn more, see Azure DevOps documentation on how to [register an app](/azure/devops/integrate/get-started/authentication/oauth?preserve-view=true&view=azure-devops#register-your-app).

The following table provides guidance on how to fill out the app registration form:

Mandatory Fields | Description | Recommended value
--- | --- | ---
| Company name         | The name of your company. | Use an appropriate value   |
| Application name     | A unique value that identifies the application that you're authorizing.    | Microsoft Search     |
| Application website  | The URL of the application that requests access to your Azure DevOps instance during connector setup. (Required).  | For **M365 Enterprise**: https://<span>gcs.office.</span>com/,</br> For **M365 Government**: https://<span>gcsgcc.<span>office.com/
| Authorization callback URL        | A required callback URL that the authorization server redirects to. | For **M365 Enterprise**: https://<span>gcs.office.</span>com/v1.0/admin/oauth/callback,</br> For **M365 Government**: https://<span>gcsgcc.office.<span>com/v1.0/admin/oauth/callback |
| Authorized scopes | The scope of access for the application | Select the following scopes: Identity (read), Work Items (read), Variable Groups (read), Project and Team (read), Graph (read), Analytics (read)|

>[!IMPORTANT]
>The authorized scopes that you select for the app should match the scopes exactly as listed above. If you omit one of the authorized scopes in the list or add another scope, the authorization will fail.

On registering the app, you get the **App ID** and **Client Secret** that is used to configure the connector.

>[!NOTE]
>To revoke access to any app registered in Azure DevOps, go to User settings at the right top of your Azure DevOps instance. Select **Profile** and then select **Authorizations** in the Security section of the side pane. Hover over an authorized OAuth app to see the **Revoke** button at the corner of the app details.

### 4. Roll out to limited audience
Deploy this connection to a limited user base if you want to validate it in Copilot and other Search surfaces before expanding the rollout to a broader audience. To know more about limited rollout, see [staged rollout](staged-rollout-for-graph-connectors.md).

At this point, you're ready to create the connection for Azure DevOps work items. You can click **Create** to publish your connection and index work items from your Azure DevOps organization.

For other settings, like **Access Permissions**, **Data Inclusion Rules**, **Schema**, **Crawl frequency**, etc., we have defaults based on what works best with ADO data. You can see the default values below:

| Users | Description |
|----|---|
| Access permissions | _Only people with access to content in Data source._ |
| Map Identities | _Data source identities mapped using Microsoft Entra IDs._ |

| Content | Description |
|---|---|
| Projects | _All projects are indexed._ |
| Manage Properties | _To check default properties and their schema, see [content](#content)_ |

| Sync | Description |
|---|---|
| Incremental Crawl | _Frequency: Every 15 mins_ |
| Full Crawl | _Frequency: Every Day_ |

If you want to edit any of these values, you need to choose the "Custom Setup" 

## Custom Setup

Custom setup is for those admins who want to edit the default values for settings listed in the above table. Once you click on the "Custom Setup" option, you see three more tabs - Users, Content, and Sync.

### Users

[![Screenshot that shows Users tab where you can configure access permissions.](media/ado-workitems-users-tab.png)](media/ado-workitems-users-tab.png#lightbox)

**Access Permissions**

The Azure DevOps Work Items connector supports search permissions visible to **Everyone** or **Only people with access to this data source**. If you choose **Everyone**, indexed data appears in the search results for all users. If you choose **Only people with access to this data source**, indexed data appears in the search results for users who have access to them.

>[!NOTE]
>
> Updates to groups governing access permissions are synced in full crawls only. Incremental crawls don't support processing of updates to permissions.

### Content

[![Screenshot that shows Content tab where you can configure projects and connection schema.](media/ado-workitems-content-tab.png)](media/ado-workitems-content-tab.png#lightbox)

**Choose projects**

In this step, you specify the scope of data that you want to index using the Azure DevOps Work Items Microsoft Graph connector. You can then choose for the connection to index either the entire organization or specific projects within the selected organization.

If you choose to index the entire organization, work items in all projects in the organization are indexed. New projects and work items are indexed during the next crawl after they're created.

If you choose to index individual projects, only work items in the selected projects are indexed.

> [!NOTE]
> Azure DevOps projects can be crawled after granting the _View project-level information_ and _View analytics_ permissions.

**Manage Properties**

Here, you can add or remove available properties from your Azure DevOps data source, assign a schema to the property (define whether a property is searchable, queryable, retrievable, or refinable), change the semantic label and add an alias to the property. Properties that are selected by default are listed below.

|Source Property|Label|Description|Schema|
|-----------|-----------|-----------|-----------|
| AreaPath | | The area path to the work item | Query, Retrieve, Search |
| AssignedTo | | Name of person the work item is assigned to | Query, Retrieve, Search |
| Authors | Authors | | Retrieve |
| ChangedBy | Last modified by | Person name who last modified the work item | Query, Retrieve |
| ChangedDate | Last modified date time | | Query, Retrieve |
| CreatedBy | Created by | Person name who created the work item | Query, Retrieve, Search |
| CreatedDate | Created date time | Timestamp when work item was created | Query, Retrieve |
| Description | Content | Description of work item | Search |
| IconUrl | IconUrl | | Retrieve |
| Id | | Work item ID | Query, Retrieve, Search |
| Priority | | Priority of work item | Query, Retrieve |
| ReproSteps | | Steps to reproduce a condition described in work item | | 
| State | | Current state of the work item | Query, Retrieve, Search |
| Tags | | | Query, Retrieve, Search |
| TeamProject | | | Retrieve |
| Title | Title | Title of the work item | Retrieve, Search |
| URL | url | URL of the work item | Retrieve |
| WorkItemType | | | Query, Retrieve, Search |

**Preview Data**

Use the preview results button to verify the sample values of the selected properties.

### Sync

[![Screenshot that shows Sync tab where you can configure crawl frequency.](media/ado-workitems-sync-tab.png)](media/ado-workitems-sync-tab.png#lightbox)

The refresh interval determines how often your data is synced between the data source and the Graph connector index. There are two types of refresh intervals - full crawl and incremental crawl. For more information, see [refresh settings](configure-connector.md#guidelines-for-sync-settings).

You can change the default values of refresh interval from here if you want to.

>[!TIP]
>**Default result type**
>* The Azure DevOps Work Items Microsoft Graph connector automatically registers a [result type](./customize-search-page.md#step-2-create-result-types) once the connector is published. The result type uses a dynamically generated [result layout](./customize-results-layout.md) based on the fields selected in step 3. 
>* You can manage the result type by navigating to [**Result types**](https://admin.microsoft.com/Adminportal/Home#/MicrosoftSearch/resulttypes) in the [Microsoft 365 admin center](https://admin.microsoft.com). The default result type is named "`ConnectionId`Default". For example, if your connection ID is `AzureDevOps`, your result layout is named: "AzureDevOpsDefault"
>* Also, you can choose to create your own result type if needed.

## Troubleshooting
After publishing your connection, you can review the status under the **Data sources** tab in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md).
You can find troubleshooting steps for commonly seen issues [here](troubleshoot-azure-devops-connector.md).

If you have issues or want to provide feedback, contact [Microsoft Graph | Support](https://developer.microsoft.com/en-us/graph/support).
