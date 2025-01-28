---
ms.date: 10/08/2019
title: "Oracle SQL Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot"
ms.author: mecampos
author: mecampos
manager: umas
audience: Admin
ms.audience: Admin
ms.topic: article
ms.service: mssearch
ms.localizationpriority: medium
search.appverid:
- BFB160
- MET150
- MOE150
description: "Set up the Oracle SQL Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot."
---
# Oracle SQL Microsoft Graph connector

The Oracle SQL Microsoft Graph connector allows your organization to discover and index data from an on-premises Oracle database. The connector indexes specified content in Microsoft Search and Microsoft Copilot 365. To keep the index up to date with source data, it supports periodic full and incremental crawls. With the Oracle SQL connector, you can also restrict access to search results for certain users.

This article is for Microsoft 365 administrators or anyone who configures, runs, and monitors an Oracle SQL Microsoft Graph connector.

## Capabilities
- Index records from your Oracle SQL database using a SQL query.
- Specify access permissions for every record with list of users or groups added in SQL query.
- Enable your end users to ask questions related to indexed records in Copilot.
- Use [Semantic search in Copilot](semantic-index-for-copilot.md) to enable users to find relevant content based on keywords, personal preferences, and social connections.

## Limitations
- Oracle SQL version: The on-premises database must run Oracle Database version 11g or later. The connector supports the Oracle database hosted on Windows, Linux, and Azure VM platforms.
- To support high crawl speed and better performance, the connector is built to support OLTP (Online Transaction Processing) workloads only. OLAP (Online Analytical Processing) workloads which don't execute the provided SQL query in 40-seconds timeout and aren't supported.
- ACLs are only supported by using a User Principal Name (UPN), Microsoft Entra ID, or Active Directory Security.
- Indexing rich content inside database columns isn't supported. Examples of such content are HTML, JSON, XML, blobs, and document parsings that exist as links inside the database columns.

## Prerequisites
- You must be the **search admin** for your organization's Microsoft 365 tenant.
- **Install the Microsoft Graph connector agent**: To access your Oracle SQL Server, you must install and configure the connector agent. See [Install the Microsoft Graph connector agent](graph-connector-agent.md) to learn more.
- **Service Account**: To connect to your SQL database and allow Microsoft Graph Connector to update records regularly, you need a service account with read permissions granted to the service account.

>[!NOTE]
>If you use Windows authentication while configuring the Oracle SQL connector, the user with which you are trying to sign in needs to have interactive login rights to the machine where the connector agent is installed. For more information, see [login policy management](/windows/security/threat-protection/security-policy-settings/allow-log-on-locally#policy-management).

## Get Started with Setup

### 1. Display name 
A display name is used to identify each citation in Copilot, helping users easily recognize the associated file or item. Display name also signifies trusted content. Display name is also used as a [content source filter](/MicrosoftSearch/custom-filters#content-source-filters). A default value is present for this field, but you can customize it to a name that users in your organization recognize.

### 2. SQL server
To connect to your SQL data, you need to specify the hostname, port, and service (database) name.

If the service name isn't available and you connect using System Identifier (SID), the service name can be derived using one of the following commands (to be executed as sys admin).
* select SERVICE_NAME from gv$session where sid in (select sid from v$MYSTAT);
* select sys_context('userenv','service_name') from dual;

### 3. Graph Connector Agent

The Graph connector agent acts as a bridge between your website instance and the connector APIs, enabling secure and efficient data transfer. In this step, select the agent configuration you want to use for your connector. 

If you didn't install the [Microsoft Graph connector agent](https://www.microsoft.com/download/details.aspx?id=104045) already, you can [download the agent installer](https://www.microsoft.com/download/details.aspx?id=104045) and follow the installation instructions to set it up. Once installed, ensure that the agent is configured correctly to connect your on-premises websites with the connector.

### 4. Authentication Type

To authenticate and sync data from Oracle SQL, choose one of the two supported methods:

a. Basic authentication

b. Windows authentication

### 5. Roll out to limited audience
Deploy this connection to a limited user base if you want to validate it in Copilot and other Search surfaces before expanding the roll-out to a broader audience. To know more about limited rollout, [click here](staged-rollout-for-graph-connectors.md).

## Content
To search your database content, you must specify SQL queries when you configure the connector. These SQL queries need to name all the database columns that you want to index (source properties). This includes any SQL joins that need to be performed to get all the columns. To restrict access to search results, you must specify Access Control Lists (ACLs) within SQL queries when you configure the connector.

### 1. Full crawl (Required)

a. **Select data columns (Required) and ACL columns (Optional)** <br>

<details>
<summary>[Click to expand] Selecting data columns for full crawl query.</summary><br>

In this step, you configure the SQL query that runs a full crawl of the database. The full crawl selects all the columns or properties which need to be presented in Microsoft Copilot or Search. You can also specify ACL columns to restrict access to search results to specific users or groups.

> [!Tip]
> To get all the columns that you need, you can join multiple tables.

![Script showing the OrderTable and AclTable with example properties.](media/MSSQL-fullcrawl.png)

The example demonstrates a selection of five data columns that hold the data for the search: OrderId, OrderTitle, OrderDesc, CreatedDateTime, and IsDeleted. To set view permissions for each row of data, you can optionally select these ACL columns: AllowedUsers, AllowedGroups, DeniedUsers, and DeniedGroups. All these data columns also have the options to **Query**, **Search**, **Retrieve**, or **Refine**.

Select data columns as shown in this example query: 
 `SELECT orderId, orderTitle, orderDesc, allowedUsers, allowedGroups, deniedUsers, deniedGroups, createdDateTime, isDeleted`

The SQL connectors don't allow column names with nonalphanumeric characters in the SELECT clause. Remove any nonalphanumeric characters from column names using an alias. Example - SELECT *column_name* AS *columnName*

To manage access to the search results, you can specify one or more ACL columns in the query. The SQL connector allows you to control access at per record level. You can choose to have the same access control for all records in a table. If the ACL information is stored in a separate table, you might have to do a join with those tables in your query.

The use of each of the ACL columns in the above query is described below. The following list explains the four **access control mechanisms**.

- **AllowedUsers**: This column specifies the list of user IDs who can access the search results. In the following example, a list of users: john@contoso.com, keith@contoso.com, and lisa@contoso.com would only have access to a record with OrderId = 12.
- **AllowedGroups**: This column specifies the group of users who are able to access the search results. In the following example, group sales-team@contoso.com would only have access to record with OrderId = 12.
- **DeniedUsers**: This column specifies the list of users who do **not** have access to the search results. In the following example, users john@contoso.com and keith@contoso.com don't have access to the record with OrderId = 13, whereas everyone else has access to this record.
- **DeniedGroups**: This column specifies the group of users who do **not** have access to the search results. In the following example, groups engg-team@contoso.com and pm-team@contoso.com don't have access to a record with OrderId = 15, whereas everyone else has access to this record.  

![Sample data showing the OrderTable and AclTable with example properties.](media/MSSQL-ACL1.png)

</details>

b. **Supported data types** <br>

<details>
<summary>[Click to expand] List of supported data types.</summary><br>

The Oracle SQL Microsoft Graph connector supports the following data types. The table also summarizes the indexing data type for the supported SQL data type. To learn more about Microsoft Graph connectors supported data types for indexing, refer to the documentation on [property resource types](/graph/api/resources/property?preserve-view=true&view=graph-rest-beta#properties).

| Category | Source data type | Indexing data type |
| ------------ | ------------ | ------------ |
| Number datatype | NUMBER(p,0) | int64 (for p <= 18) <br> double (for p > 18). |
| Floating-point number datatype | NUMBER(p,s) <br> FLOAT(p) | double. |
| Date datatype | DATE <br> TIMESTAMP <br> TIMESTAMP(n) | datetime. |
| Character datatype | CHAR(n) <br> VARCHAR <br> VARCHAR2 <br> LONG <br> CLOB <br> NCLOB | string. |
| Unicode character datatype | NCHAR <br> NVARCHAR | string. |
| RowID datatype | ROWID <br> UROWID | string. |

For any other data type currently not directly supported, the column needs to be explicitly cast to a supported data type.

</details>

c. **Watermark (Required)** <br>

<details>
<summary>[Click to expand] Specifying the watermark column in full crawl query</summary><br>

To prevent overloading the database, the connector batches and resumes full-crawl queries with a full-crawl watermark column. Using the value of the watermark column, each subsequent batch is fetched, and querying is resumed from the last checkpoint. Essentially this mechanism controls data refresh for full crawls.

Create query snippets for watermarks as shown in these examples:

- `WHERE (CreatedDateTime > @watermark)`. Cite the watermark column name with the reserved keyword `@watermark`. If the sort order of the watermark column is ascending, use `>`; otherwise, use `<`.
- `ORDER BY CreatedDateTime ASC`. Sort on the watermark column in ascending or descending order.

In the configuration shown in the following image, `CreatedDateTime` is the selected watermark column. To fetch the first batch of rows, specify the data type of the watermark column. In this case, the data type is `DateTime`.

![Watermark column configuration.](media/MSSQL-watermark.png)

The first query fetches the first **N** number of rows by using: "CreatedDateTime > January 1, 1753 00:00:00" (min value of DateTime data type). After the first batch is fetched, the highest value of `CreatedDateTime` returned in the batch is saved as the checkpoint if the rows are sorted in ascending order. An example is March 1, 2019 03:00:00. Then the next batch of **N** rows is fetched by using "CreatedDateTime > March 1, 2019 03:00:00" in the query.

</details>

### 2. Manage properties

The SQL connector picks up all columns specified in the full crawl SQL query as source properties for ingestion. In this step, you can define the search schema for your content. This involves defining the search annotations like search, retrieve, query, and refine for selected source properties. This also includes assigning semantic labels and aliases to enhance search relevance. To learn more about search schema, refer to the documentation on [guidelines for 'manage properties'](/MicrosoftSearch/configure-connector#guidelines-for-manage-properties).

### 3. Incremental crawl (optional)

a. **Incremental sync query** <br>

In this optional step, provide a SQL query to run an incremental crawl of the database. With this query, the SQL connector determines any changes to the data since the last incremental crawl. As in the full crawl, select all columns where you want to select the options **Query**, **Search**, **Retrieve** or **Refine**. Specify the same set of ACL columns that you specified in the full crawl query.

The components in the following image resemble the full crawl components with one exception. In this case, "ModifiedDateTime" is the selected watermark column. Review the full crawl steps to learn how to write your incremental crawl query and see the following image as an example.

![Incremental crawl script showing OrderTable, AclTable and example properties that can be used.](media/MSSQL-incrcrawl.png)

b. **Soft delete instructions (Optional)**

In a SQL record system, a soft delete is a technique where, instead of physically removing a record from a database, you mark it as "deleted" by setting a specific flag or column. This allows the record to remain in the database, but is logically excluded from most operations. To delete soft-deleted rows in your database during incremental crawl, specify the soft-delete column name and value that indicates the row is deleted.

![Soft delete settings: "Soft delete column" and "Value of soft delete column which indicates a deleted row."](media/MSSQL-softdelete.png)

## Users

You can choose to use the **Only people with access to this data source** to restrict access to users or groups as selected in full crawl query or you can override them to make your content visible to **Everyone**.

### 1. Map columns containing access permissions information

Choose the various access control (ACL) columns that specify the access control mechanism. Select the column name you specified in the full crawl SQL query. Note that "deny" takes precedence over "allow" permissions.

Each of the ACL columns is expected to be a multi-valued column. These multiple ID values can be separated using separators such as semicolon (;), comma (,), and so on. You need to specify this separator in the **value separator** field.

The following ID types are supported for use as ACLs:

- **User Principal Name (UPN)**: A User Principal Name (UPN) is the name of a system user in an email address format. A UPN (for example: john.doe@domain.com) consists of the username (login name), separator (the @ symbol), and domain name (UPN suffix).
- **Microsoft Entra ID**: In Microsoft Entra ID, every user or group has an object ID that looks something like 'e0d3ad3d-0000-1111-2222-3c5f5c52ab9b'.
- **Active Directory (AD) Security ID**: In an on-premises AD setup, every user and group have an immutable, unique security identifier that looks something like 'S-1-5-21-3878594291-2115959936-132693609-65242.'

![Search permission settings to configure access control lists.](media/MSSQL-ACL2.png)

## Sync
The refresh interval determines how often your data is synced between the data source and the Graph connector index.

You can configure full and incremental crawls based on the scheduling options present here. By default, incremental crawl (if configured) is set for every 15 minutes, and full crawl is set for every day. If needed, you can adjust these schedules to fit your data refresh needs.

At this point, you're ready to create the connection for Oracle SQL. You can click on the "Create" button to publish your connection and index data from your database.

## Troubleshooting
After publishing your connection, you can review the status under the **Data sources** tab in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md).
You can find troubleshooting steps for commonly seen issues [here](troubleshoot-oraclesql-connector.md).

If you have issues or want to provide feedback, contact [Microsoft Graph | Support](https://developer.microsoft.com/en-us/graph/support).
