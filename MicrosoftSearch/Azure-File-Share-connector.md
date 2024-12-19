--- 
title: "Azure File Share Graph connector for Microsoft Search and Copilot" 
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
description: "Set up the Azure File Share Graph connector for Microsoft Search and Copilot" 
ms.date: 12/02/2024
---

# Azure File Share Graph Connector (Preview)

The Azure File Share Graph Connector integrates Azure File Share data with the Microsoft 365 ecosystem, enabling advanced search and interaction capabilities via Microsoft Search and Copilot. By respecting NTFS permissions, it ensures that only authorized users can access indexed content within M365.

This guide is designed for Microsoft 365 administrators responsible for configuring, managing, and monitoring the Azure File Share Graph Connector.

## Capabilities

### Access
- Users can access Azure File Share data via Microsoft Search and Copilot.
- Access control integrates Azure Active Directory (AAD) and NTFS permissions, ensuring secure data interaction.

### Data Syncing
- Supports periodic full crawls and incremental crawls (configurable intervals).
- Indexes text-based file content, metadata, directory structures, and ACLs.

## Limitations

### Indexing
- Text-based files are indexed up to 4MB in size (3.8MB content + 0.2MB metadata).
- Supported formats include Microsoft Office files, PDFs, text files, JSON files, and other text-based files. The content indexing of non-text files is excluded by default.

## Prerequisites

Before you set up an Azure File Share connector, ensure the following prerequisites are met:

### Azure File Share Configuration
- Your Azure File Share should be mounted on a device.
- A Graph Connector Agent (GCA) must be installed and registered on the device.

### User Credentials
Use the same credentials for:
- Mounting the Azure File Share.
- Running the Graph Connector Agent.
- Configuring the connector in the Microsoft 365 Admin Center.

### Verify the Azure File Share URL
- The file share URL format: `https://<storageaccount>.file.core.windows.net/<filesharename>`.

### Plan the Scope
- Determine whether to index the entire file share or specific directories based on organizational needs.

## Set Up

### Display Name
Provide a name for the connector, such as "Azure File Share - Marketing Team."

### Add Source folder Path(s)
Enter UNC path of the Azure File Share to connect to the data source.
Foe example, "\\testpath.file.core.windows.net\test_folder\test_folder2"

### Configure Authentication
- Select the GCA for your tenant.
- Use Windows authentication with valid admin credentials.

### Rollout to Limited Audience
Deploy the connector for a small group to validate indexing and access controls before a broader rollout.

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

To adjust these defaults, select the **Custom Setup** option during configuration.

## Custom Setup

### Users

**Access Permissions**  
The connector respects ACLs defined in NTFS setups. Admins can optionally make all indexed content visible to all M365 users, though this is not recommended.

### Content

**Custom Properties**

You can enrich your indexed data by creating custom properties based on the connector's default properties.

:::image type="content" source="media/file-connector/connectors-custom-property-setup.png" alt-text="Custom property set up with a rule for URL.":::

To add a custom property:

  1. Enter a property name. This name appears in the search results from this connector.
  1. For the value, select **Static or String/Regex Mapping**. A static value is included in all search results from this connector. The string/regex value varies based on the rules you add.
  1. Select **Edit value**.
  1. If you selected a static value, enter the string you want to appear.
  1. If you selected a string/regex value:
      * In the **Add expressions** section, in the **Property** list, select a default property from the list.
      * For **Sample value**, enter a string to represent the type of values that could appear. This sample is used when you preview your rule.
      * For **Expression**, enter a regex expression to define the portion of the property value that should appear in search results. You can add up to three expressions. To learn more about regex expressions, see [Regular expression language quick reference](/dotnet/standard/base-types/regular-expression-language-quick-reference) or search the web for a regex expression reference guide.
      * In the **Create formula** section, enter a formula to combine the values extracted from the expressions. 

**Assign property labels**

Follow the general [setup instructions](./configure-connector.md).
<!---If the above phrase does not apply, delete it and insert specific details for your data source that are different from general setup instructions.-->

**Manage schema**

Follow the general [setup instructions](./configure-connector.md).
<!---If the above phrase does not apply, delete it and insert specific details for your data source that are different from general setup instructions.-->

### Sync

**Schedule Adjustments**  
- **Incremental Crawl**: Default is 15 minutes.
- **Full Crawl**: Default is daily.

## Troubleshooting

You can find troubleshooting steps for commonly seen issues [here](troubleshoot-azure-file-share-connector.md).

## What's Next

After publishing your connection, review the status under the **Data Sources** tab in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md).

If you have any additional issues or feedback, contact us at [Microsoft Graph | Support](https://developer.microsoft.com/en-us/graph/support).

