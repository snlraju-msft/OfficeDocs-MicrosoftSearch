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

The Azure File Share Graph Connector brings Azure File Share data into the Microsoft 365 ecosystem. It allows users to access indexed data via Microsoft Search and Copilot, while maintaining NTFS permissions to ensure secure access for authorized users.

This guide is for Microsoft 365 administrators responsible for setting up and managing the Azure File Share Graph Connector.

---

## Key Features

### Secure Access
- Enables users to securely access Azure File Share data in Microsoft Search and Copilot.
- Integrates NTFS permissions with Azure Active Directory (AAD) for seamless security.

### Data Syncing
- Supports regular full crawls to keep indexed data up to date.
- Indexes text-based file content, metadata, directory structures, and access control lists (ACLs).

---

## Limitations

### Supported Data
- Indexes text-based files up to 4MB (3.8MB content + 0.2MB metadata).
- Supported file formats include:
  - Microsoft Office files
  - PDFs
  - Text files
  - JSON files
- Non-text files are excluded by default.

---

## Prerequisites

Before you begin setup, ensure the following requirements are met:

### Azure File Share Configuration
- Your Azure File Share must be mounted on a device.
- Install and register the Graph Connector Agent (GCA) on the same device.

### Credentials
Use the same user credentials for:
- Mounting the Azure File Share.
- Running the Graph Connector Agent.
- Configuring the connector in the Microsoft 365 Admin Center.

---

## Setup Guide

### Step 1: Configure the Display Name
Assign a descriptive name to the connector, such as "Azure File Share - Marketing Team," to easily identify it in the Admin Center.

### Step 2: Add Source Folder Paths
Enter the UNC path for the Azure File Share. For example:  
`\\testpath.file.core.windows.net\test_folder\test_folder2`

### Step 3: Set Up Authentication
- Select the registered GCA for your tenant.
- Use Windows authentication with valid admin credentials.

### Step 4: Test with a Limited Audience
Deploy the connector to a small user group to validate indexing and permissions before expanding to the entire organization.

### Step 5: Schedule Full Crawls
Set up regular full crawls to keep your indexed data updated. By default, full crawls are scheduled daily.

---

## Default Settings

| Section   | Setting               | Default Value                                                     |
|-----------|-----------------------|-------------------------------------------------------------------|
| **Users** | Access Permissions    | Respects NTFS permissions; only authorized files are accessible. |
| **Content** | Index Metadata       | Includes properties such as file name, owner, last modified date, and file path. |
| **Sync**  | Full Crawl Frequency  | Once daily.                                                      |

To modify these settings, use the **Custom Setup** option during configuration.

---

## Custom Setup

### Access Permissions
The connector strictly adheres to ACLs defined in NTFS setups. While administrators can allow broader access, it is recommended to maintain the default permissions for security.

### Enhancing Metadata

#### Adding Custom Properties
You can extend the indexed metadata by creating custom properties.

**Steps to Add a Custom Property**:
1. **Name the Property**  
   Enter a name that will appear in search results.

2. **Define the Value Type**  
   Choose either:
   - **Static Value**: A constant value that applies to all search results.
   - **String/Regex Mapping**: A dynamic value based on specific rules.

3. **Configure the Value**  
   - For static values, enter the desired string.
   - For regex-based values:
     - Select a default property in **Add Expressions**.
     - Provide a sample value for previewing purposes.
     - Add up to three regex-based expressions.  
       [Learn more about regex expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).
     - Combine extracted values using a formula in the **Create Formula** section.

#### Managing Schema and Property Labels
Follow the [general setup instructions](./configure-connector.md) to manage the schema and assign property labels.

---

## Troubleshooting

Refer to the [Troubleshooting Guide](troubleshoot-azure-file-share-connector.md) for assistance with common issues.

---

## Next Steps

Once your connector is configured, monitor its status under **Data Sources** in the [Admin Center](https://admin.microsoft.com). For instructions on updating or removing the connector, see [Manage Your Connector](manage-connector.md).

For additional support or feedback, contact [Microsoft Graph Support](https://developer.microsoft.com/en-us/graph/support).
