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

The Azure File Share Graph Connector integrates Azure File Share data into the Microsoft 365 ecosystem, enabling advanced search and interaction capabilities via Microsoft Search and Copilot. By respecting NTFS permissions, it ensures that only authorized users can access indexed content within Microsoft 365.

This guide is intended for Microsoft 365 administrators responsible for configuring, managing, and monitoring the Azure File Share Graph Connector.

---

## Key Capabilities

### Access
- Provides secure access to Azure File Share data via Microsoft Search and Copilot.
- Integrates Azure Active Directory (AAD) and NTFS permissions for seamless security.

### Data Syncing
- Supports periodic full crawls.
- Indexes text-based file content, metadata, directory structures, and ACLs.

---

## Limitations

### Indexing
- Files up to 4MB (3.8MB content + 0.2MB metadata) are indexed.
- Supported formats include:
  - Microsoft Office files
  - PDFs
  - Text files
  - JSON files
- Non-text files are excluded by default.

---

## Prerequisites

Before setting up the Azure File Share Connector, ensure the following requirements are met:

### Azure File Share Configuration
- Mount your Azure File Share on a device.
- Install and register the Graph Connector Agent (GCA) on the device.

### User Credentials
Use the same credentials for:
- Mounting the Azure File Share.
- Running the Graph Connector Agent.
- Configuring the connector in the Microsoft 365 Admin Center.

---

## Setup Guide

### Step 1: Configure Display Name
Provide a descriptive name for the connector, such as "Azure File Share - Marketing Team."

### Step 2: Add Source Folder Paths
Specify the UNC path for the Azure File Share, e.g.,  
`\\testpath.file.core.windows.net\test_folder\test_folder2`

### Step 3: Configure Authentication
- Select the GCA for your tenant.
- Use Windows authentication with valid admin credentials.

### Step 4: Deploy to Limited Audience
Deploy the connector to a small group to validate indexing and access controls before a broader rollout.

### Step 5: Schedule Full Crawls
Set up periodic full crawls to update indexed data (default: daily).

---

## Default Settings

| Section  | Setting               | Default Value                                                      |
|----------|-----------------------|--------------------------------------------------------------------|
| Users    | Access Permissions    | Respects NTFS permissions; only authorized files are indexed.      |
| Content  | Index Metadata        | Includes properties such as file name, created by, last modified, and file path. |
| Sync     | Full Crawl Frequency  | Every day.                                                        |

To adjust these defaults, use the **Custom Setup** option during configuration.

---

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

---

## Troubleshooting

For common issues and resolutions, refer to the [Troubleshooting Guide](troubleshoot-azure-file-share-connector.md).

---

## What's Next

Once the connector is configured and published, monitor its status under **Data Sources** in the [Admin Center](https://admin.microsoft.com). To make updates or delete the connection, see [Manage Your Connector](manage-connector.md).

For additional support or feedback, contact [Microsoft Graph Support](https://developer.microsoft.com/en-us/graph/support).
