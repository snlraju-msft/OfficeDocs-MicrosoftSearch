--- 
title: "Azure File Share Graph connector for Microsoft Search and Copilot" 
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
description: "Set up the Azure File Share Graph connector for Microsoft Search and Copilot" 
ms.date: 12/02/2024
---

# Azure File Share Graph Connector (Preview)

The Azure File Share Graph Connector integrates Azure File Share data into the Microsoft 365 ecosystem, enabling advanced search and interaction capabilities via Microsoft Search and Copilot. It respects NTFS permissions, ensuring that only authorized users can access indexed content within Microsoft 365.

This guide is intended for Microsoft 365 administrators responsible for configuring, managing, and monitoring the Azure File Share Graph Connector.

## Capabilities

### Access
- Enables access to Azure File Share data via Microsoft Search and Copilot.
- Integrates Azure Active Directory (AAD) and NTFS permissions for secure data interaction.

### Data Syncing
- Supports periodic full crawl.
- Indexes text-based file content, metadata, directory structures, and ACLs.

## Limitations

### Indexing
- Indexes text-based files up to 4MB (3.8MB content + 0.2MB metadata).
- Supports formats like Microsoft Office files, PDFs, text files, JSON files, and other text-based files. Non-text files are excluded by default.

## Prerequisites

Before setting up the Azure File Share Connector, ensure the following:

### Azure File Share Configuration
- Your Azure File Share must be mounted on a device.
- Install and register a Graph Connector Agent (GCA) on the device.

### User Credentials
Use the same credentials for:
- Mounting the Azure File Share.
- Running the Graph Connector Agent.
- Configuring the connector in the Microsoft 365 Admin Center.

## Set Up

### Display Name
Provide a descriptive name, e.g., "Azure File Share - Marketing Team."

### Add Source Folder Paths
Enter the UNC path for the Azure File Share, e.g.,  
`\\testpath.file.core.windows.net\test_folder\test_folder2`

### Configure Authentication
- Select the GCA for your tenant.
- Use Windows authentication with valid admin credentials.

### Rollout to a Limited Audience
Deploy the connector to a small group to validate indexing and access controls before a broader rollout.

### Customize Sync Schedules
- Set periodic full crawls (default: daily).
- Configure incremental crawls (default: every 15 minutes).

## Default Settings

| Section  | Setting            | Default Value                                                      |
|----------|--------------------|--------------------------------------------------------------------|
| Users    | Access Permissions | Respects NTFS permissions; only viewable files are indexed and accessible. |
| Content  | Index Metadata     | Includes properties like file name, created by, last modified, and file path. |
| Sync     | Incremental Crawl  | Every 15 minutes.                                                 |
| Sync     | Full Crawl         | Every day.                                                        |

To adjust these defaults, use the **Custom Setup** option during configuration.

## Custom Setup

### Users

**Access Permissions**  
The connector respects ACLs defined in NTFS setups. Admins can optionally make all indexed content visible to all Microsoft 365 users, though this is not recommended.

### Content

#### Custom Properties

You can enrich indexed data by creating custom properties based on the connector's default properties.

##### To Add a Custom Property:

1. **Enter a Property Name**  
   Provide a name for the property that will appear in search results.

2. **Select a Value Type**  
   Choose between **Static** or **String/Regex Mapping**:
   - **Static Value**: Appears in all search results.
   - **String/Regex Value**: Varies based on added rules.

3. **Edit the Value**  
   - **For Static Value**: Enter the desired string.
   - **For String/Regex Value**:
     - In **Add Expressions**, select a default property.
     - Enter a **Sample Value** for preview purposes.
     - Add up to three **Expressions** using regex.  
       [Learn about regex expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).
     - Combine the extracted values in the **Create Formula** section.

#### Assign Property Labels
Follow the general [setup instructions](./configure-connector.md).

#### Manage Schema  
Follow the general [setup instructions](./configure-connector.md).

### Sync

#### Schedule Adjustments
- **Full Crawl**: Default is daily.

## Troubleshooting

Find steps for resolving common issues [here](troubleshoot-azure-file-share-connector.md).

## What's Next

After publishing your connection, monitor the status under **Data Sources** in the [admin center](https://admin.microsoft.com). To update or delete connections, see [Manage your connector](manage-connector.md).

For additional issues or feedback, contact [Microsoft Graph Support](https://developer.microsoft.com/en-us/graph/support).
