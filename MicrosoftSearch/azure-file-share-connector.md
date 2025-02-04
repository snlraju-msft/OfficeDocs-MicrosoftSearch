---
title: "Azure File Share Graph Connector for Microsoft Search and Copilot"
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
description: "Set up the Azure File Share Graph Connector for Microsoft Search and Copilot"
ms.date: 12/02/2024
---

# Azure File Share Graph Connector (Preview)

The Azure File Share Graph Connector allows organizations to integrate Azure File Share data into Microsoft 365. It enables users to access indexed files via Microsoft Search and Copilot while ensuring security by adhering to NTFS permissions.

This guide is designed for Microsoft 365 administrators responsible for configuring, managing, and monitoring the Azure File Share Graph Connector.

---

## Key Features

### Secure Access
- Seamless access to Azure File Share data within Microsoft Search and Copilot.
- Ensures data security by integrating NTFS permissions with Azure Active Directory (AAD).

### Data Synchronization
- Performs regular full crawls to update indexed content.
- Indexes file content, metadata, directory structures, and access control lists (ACLs).

---

## Limitations

### Supported Data
- Files up to 100 MB in size are indexed, with a maximum of 4 MB of text content extracted per file.
- Supported file formats include:
  - Microsoft Office files
  - PDFs
  - Text files
  - JSON files
- Non-text files are excluded by default.

---

## Prerequisites

Ensure the following requirements are met before starting the setup process:

### Azure File Share Configuration
- Mount your Azure File Share on a device.
- Install and register the **Graph Connector Agent (GCA)** on the same device.

### User Credentials
Use the same credentials for:
- Mounting the Azure File Share.
- Running the Graph Connector Agent.
- Configuring the connector in the Microsoft 365 Admin Center.

---

## Setup Guide

### Step 1: Name Your Connector
Provide a clear, descriptive name for the connector. For example:  
`Azure File Share - Marketing Team`

### Step 2: Specify the Source Folder Paths
Enter the UNC path for the Azure File Share, such as:  
`\\testpath.file.core.windows.net\test_folder\test_folder2`

### Step 3: Configure Authentication
- Select the registered **Graph Connector Agent (GCA)** for your tenant.
- Use Windows authentication with valid admin credentials.

### Step 4: Test with a Limited Audience
Deploy the connector to a small group to verify indexing and permissions functionality before expanding it to your entire organization.

---

## Default Settings

| Section  | Setting               | Default Value                                                     |
|----------|-----------------------|-------------------------------------------------------------------|
| **Users** | Access Permissions    | Respects NTFS permissions; only authorized files are accessible. |
| **Content** | Index Metadata       | Includes properties like file name, owner, last modified, and file path. |
| **Sync**  | Full Crawl Frequency  | Once daily.                                                      |

If these defaults don’t suit your requirements, use the **Custom Setup** option to make adjustments.

---

## Custom Setup

### Access Permissions
The connector adheres to NTFS ACLs to control file access. Administrators can broaden access, but maintaining default permissions is recommended for security.

### Enhancing Metadata

#### Adding Custom Properties
Create custom properties to extend the metadata available for search.

**Steps to Add a Custom Property**:
1. **Enter a Property Name**  
   Name the property as it will appear in search results.

2. **Define the Value Type**  
   Choose one of the following:
   - **Static Value**: A constant value applied to all search results.
   - **String/Regex Mapping**: A dynamic value based on specific rules.

3. **Configure the Value**  
   - For static values, enter the desired text.
   - For regex-based values:
     - In **Add Expressions**, choose a default property.
     - Provide a sample value for preview.
     - Add up to three regex expressions.  
       [Learn more about regex expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).
     - Combine extracted values using a formula in the **Create Formula** section.

#### Managing Schema and Labels
To manage schema and assign labels to properties, follow the [general setup instructions](./configure-connector.md).

---

## Troubleshooting

For help resolving common issues, refer to the [Troubleshooting Guide](troubleshoot-azure-file-share-connector.md).

---

## Next Steps

After completing the setup, monitor the connector’s status under the **Data Sources** section in the [Admin Center](https://admin.microsoft.com). For guidance on making updates or removing the connector, see [Manage Your Connector](manage-connector.md).

If you need additional help or have feedback, contact [Microsoft Graph Support](https://developer.microsoft.com/en-us/graph/support).
