--- 

title: "Veeva Vault - PromoMats Graph connector for Microsoft Search and Copilot" 
ms.author: dannyyao
author: ms-dannyyao
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
description: "Set up the Veeva Vault - PromoMats Graph connector for Microsoft Search and Copilot" 
ms.date: 12/02/2024
---

# Veeva Vault - PromoMats Graph Connector (Preview)

With the Microsoft Graph connector, your organization in M365 can index documents that are managed by Veeva Vault - PromoMats, using Microsoft Copilot and Search. The connector respects permission rules to ensure only authorized users can interact with documents within the M365 ecosystem.

This article is for Microsoft 365 administrators or anyone who configures, runs, and monitors the Veeva Vault - PromoMats connector.

## Capabilities

### Access
- Users can access documents and metadata stored in Veeva Vault through Microsoft 365 applications like Microsoft Search and Copilot.
- Access is controlled by Veeva Vault's ACLs and integrated with Microsoft Entra ID for seamless permission mapping.

### Data Syncing
- The connector supports periodic full crawls and incremental syncs, configurable by the admin.
- Data crawled includes document metadata, lifecycle stages, and ACLs.

## Limitations
### Indexing
- Only the latest document versions are indexed.
- Supported file types include text-based and Microsoft Office documents, PDFs, and more.
- Files larger than 4MB are indexed partially.

## Prerequisites
Before you create a Veeva Vault connector, ensure the following steps are completed:

### Obtain Veeva Vault Administrator Credentials
- You must have a Veeva Vault account with administrative privileges.
- Credentials include a valid username, password, and API access enabled.

### Set up API Access
- Enable REST API access for your Veeva Vault instance. For more information, see [Veeva Vault API documentation](https://developer.veevavault.com/docs/).

### Configure Access Permissions
- Ensure user roles in Veeva Vault are correctly assigned. Only users with viewing permissions can access content via the connector.

### Verify the Veeva Vault URL
- Obtain the correct URL of your Veeva Vault instance. The typical format is `https://<your-vault-domain>.veevavault.com`.

### Decide the Scope of Data to be Indexed
- Plan whether to crawl specific applications like PromoMats, QualityDocs, or RIM. Each application may require a separate connector depending on its data model.

## Set Up

### Display Name
Provide a display name for your connector in the Microsoft 365 Admin Center. This name helps identify the connection in your workspace.

### Veeva Vault URL
Enter the URL of your Veeva Vault instance. For example: `https://<your-vault-domain>.veevavault.com`.

### Authentication Details
- **Basic Authentication**: Enter your Veeva Vault username and password.
- **OAuth 2.0 Authentication**: Configure the OAuth client in your Veeva Vault admin portal and use the generated credentials.

### Rollout to Limited Audience
Deploy this connection to a limited group of users to validate indexing and access control functionality before a full rollout.

### Customize Sync Schedules
- Set up periodic incremental crawls (default: 15 minutes) and full crawls (default: daily).

## Default Settings

| Section | Setting               | Default Value |
|---------|-----------------------|---------------|
| Users   | Access Permissions   | Data indexed respects Veeva Vault permissions; only viewable documents are accessible. |
| Content | Index Metadata       | Key metadata, such as document name, owner, and lifecycle stage, are indexed by default. |
| Content | Manage Properties    | Metadata properties such as title, created by, and last modified by are enabled by default. |
| Sync    | Incremental Crawl Frequency | Every 15 minutes. |
| Sync    | Full Crawl Frequency | Every day. |

To change any of these defaults, choose the **Custom Setup** option during configuration.

## Custom Setup

### Users
**Access Permissions**  
The connector adheres to the ACLs defined in Veeva Vault. Only users with view permissions in Veeva Vault can see the indexed content in Microsoft 365. Admins can optionally allow all users access to all indexed content, though this is not recommended.

### Sync
**Schedule Adjustments**  
Adjust crawl frequency to fit your organization's requirements:
- **Incremental Crawl**: Default is 15 minutes.
- **Full Crawl**: Default is daily.

## Troubleshooting

### Common Errors

| Error                   | Description                               | Resolution                                                                 |
|-------------------------|-------------------------------------------|---------------------------------------------------------------------------|
| `INVALID_SESSION_ID`    | Authentication session expired or invalid.| Reauthenticate with valid credentials.                                   |
| `INSUFFICIENT_ACCESS`   | User lacks permissions to access files.   | Verify user roles and ACLs in Veeva Vault.                               |
| `API_LIMIT_EXCEEDED`    | Too many API requests made in a short period. | Adjust crawl frequency or retry after some time.                        |
| **Missing Properties or Documents** | Required metadata properties are not enabled. | Ensure metadata properties are enabled in Veeva Vault and test retrieval.|

## What's Next

After publishing your connection, review the status under the **Data Sources** tab in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md).

If you have additional issues or feedback, contact us at [Microsoft Graph | Support](https://developer.microsoft.com/en-us/graph/support).
