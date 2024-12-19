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
- API access enabled.

### Set up API Access
- Enable REST API access for your Veeva Vault instance. For more information, see [Veeva Vault API documentation](https://developer.veevavault.com/docs/).

### Verify the Veeva Vault URL
- Obtain the correct URL of your Veeva Vault instance. The typical format is `https://<your-vault-domain>.veevavault.com`.

## Set Up

### Display Name
Provide a display name for your connector in the Microsoft 365 Admin Center. This name helps identify the connection in your workspace.

### Veeva Vault URL
Enter the URL of your Veeva Vault instance. For example: `https://<your-vault-domain>.veevavault.com`.

### Authentication Details
To configure the Veeva Vault - PromoMats connector, you must provide authentication credentials. The connector supports the following authentication methods:

#### **1. Basic Authentication**
- **Username**: The username associated with your Veeva Vault account.
- **Password**: The password for the account. Ensure this credential is kept secure, as it will be used for authentication.

#### **2. Azure AD Authentication**
This method leverages Azure Active Directory (AAD) for secure and centralized identity management. Required fields include:
- **Vault Session ID URL**: The URL endpoint for retrieving session tokens. Typically formatted as: `https://<your-vault-domain>.veevavault.com/api/v<version>/session`.
- **Client ID**: The application ID for your Azure AD app registered for Veeva Vault.
- **Client Secret**: The client secret associated with the Azure AD app. Ensure this is securely stored and accessible only to authorized personnel.

**Important:** Ensure that the necessary configurations are made in both Azure AD and Veeva Vault admin settings to enable Azure AD authentication.

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

You can find troubleshooting steps for commonly seen issues [here](troubleshoot-veeva-vault-promomats-connector.md).

## What's Next

After publishing your connection, review the status under the **Data Sources** tab in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md).

If you have additional issues or feedback, contact us at [Microsoft Graph | Support](https://developer.microsoft.com/en-us/graph/support).
