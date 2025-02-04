--- 

title: "Veeva Vault - PromoMats Graph connector for Microsoft Search and Copilot" 
ms.author: dannyyao
author: dannyyaou
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

The Veeva Vault - PromoMats Graph Connector allows organizations to index documents managed in Veeva Vault - PromoMats, making them accessible through Microsoft Search and Copilot. The connector respects Veeva Vault permission rules to ensure that only authorized users can interact with indexed documents in the Microsoft 365 ecosystem.

This guide is for Microsoft 365 administrators or anyone responsible for configuring, managing, and monitoring the Veeva Vault - PromoMats Graph Connector.

---

## Key Capabilities

### Access
- Provides seamless access to Veeva Vault documents and metadata via Microsoft Search and Copilot.
- Integrates Veeva Vault's ACLs with Microsoft Entra ID for secure permission mapping.

### Data Syncing
- Supports periodic full crawls, configurable based on organizational needs.
- Captures document metadata, lifecycle stages, and ACLs during crawls.

---

## Limitations

### Indexing
- Only the latest versions of documents are indexed.
- Supported file types include:
  - Microsoft Office documents
  - PDFs
  - Text-based files
- Files up to 100 MB in size are indexed, with a maximum of 4 MB of text content extracted per file.

---

## Prerequisites

### Administrator Credentials
- Ensure you have a Veeva Vault account with administrative privileges.
- Enable API access in your Veeva Vault instance.

### API Configuration
- Activate REST API access in your Veeva Vault instance. For details, refer to the [Veeva Vault API documentation](https://developer.veevavault.com/docs/).

### Veeva Vault URL
- Verify the URL for your Veeva Vault instance. The format typically looks like:  
  `https://<your-vault-domain>.veevavault.com`

---

## Set Up Guide

### Step 1: Configure Display Name
Provide a meaningful display name in the Microsoft 365 Admin Center to identify the connector.

### Step 2: Add Veeva Vault URL
Enter the verified URL of your Veeva Vault instance, e.g.,  
`https://<your-vault-domain>.veevavault.com`

### Step 3: Authentication Details

#### Microsoft Entra ID Authentication
To use Microsoft Entra ID authentication, ensure the following configurations are in place:
- **Vault Session ID URL**:  
  Example: `https://<your-vault-domain>.veevavault.com/api/v<version>/session`
- **Client ID**: The application ID of your Microsoft Entra ID app registered for Veeva Vault.
- **Client Secret**: The corresponding client secret. Securely store and restrict access to this value.

**Important:** Configure both Microsoft Entra ID and Veeva Vault admin settings to enable Microsoft Entra ID authentication.

### Step 4: Deploy to Limited Audience
Test the connector by rolling it out to a small user group to validate indexing and access control.

### Step 5: Customize Sync Schedules
- Full Crawls: Default is daily.

---

## Default Settings

| Section  | Setting               | Default Value |
|----------|-----------------------|---------------|
| **Users**   | Access Permissions   | Respects Veeva Vault permissions; only viewable documents are accessible. |
| **Content** | Index Metadata       | Indexes key metadata, such as document name, owner, and lifecycle stage. |
| **Content** | Manage Properties    | Enables metadata like title, created by, and last modified by. |
| **Sync**    | Full Crawls          | Every day. |

To modify these defaults, select the **Custom Setup** option during configuration.

---

## Custom Setup

### Users

**Access Permissions**  
The connector adheres to the ACLs defined in Veeva Vault. Only users with appropriate permissions in Veeva Vault can view indexed content in Microsoft 365. It is possible to override this and allow all users access to indexed content, though this is not recommended.

### Sync

**Adjust Sync Schedules**  
You can modify the frequency of full crawls to fit your organization's requirements:
- Full Crawls: Default is daily.

---

## Troubleshooting

For common issues and their resolutions, refer to the [Troubleshooting Guide](troubleshoot-veeva-vault-promomats-connector.md).

---

## Next Steps

Once the connector is configured and published, monitor its status under the **Data Sources** tab in the [Admin Center](https://admin.microsoft.com). To make updates or remove the connector, refer to the [Manage Your Connector](manage-connector.md) guide.

For additional support, contact [Microsoft Graph Support](https://developer.microsoft.com/en-us/graph/support).
