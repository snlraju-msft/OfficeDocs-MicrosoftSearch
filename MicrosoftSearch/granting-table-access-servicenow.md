---
ms.date: 01/09/2024
title: "Grant Table Access to a Service Account in ServiceNow"
ms.author: souravpoddar
author: souravpoddar001
manager: harshkum
audience: Admin 
ms.audience: Admin
ms.topic: article
ms.service: mssearch
ms.localizationpriority: medium
search.appverid:
- BFB160
- MET150
- MOE150
description: "Grant table access to a service account in ServiceNow which can then be used to set up Microsoft Graph Connectors for ServiceNow."
---

# Granting Table Access to a User in ServiceNow

This article explains how to grant table access to a service account in ServiceNow. The process involves creating a role, assigning it to a user, and configuring row-level and field-level access controls.

## Prerequisites

- **Administrator Role**: Ensure you have admin access in ServiceNow.
- **Security Admin Role**: Elevate to the `security_admin` role to make changes to Access Control Lists (ACLs).

---

## Step 1: Create a User

1. Navigate to **User Administration > Users**.
2. Click **New** to create a new user.
3. Fill in the user details, such as `microsoft.copilot` for the User ID and `Microsoft` and `Copilot` for the First Name and Last Name respectively.
4. Click **Submit** to save the user.

---

## Step 2: Create a Role

1. Navigate to **User Administration > Roles**.
2. Click **New**.
3. Enter a unique name for the role (e.g., `Microsoft Graph Connector Account`).
4. Click **Submit** to save the role.

---

## Step 3: Assign the Role to a User

1. Navigate to **User Administration > Users**.
2. Open the user record for the intended user (e.g., `Microsoft Copilot`).
3. In the **Roles** related list, click **Edit**.
4. Add the newly created role (`Microsoft Graph Connector Account`).
5. Click **Save** to finalize the assignment.
6. Click on **Update** to update the user record.

---

## Step 4: Grant Row-Level Access

To grant access to rows within a specific table, follow these steps:

1. Elevate to the `security_admin` role.
2. Navigate to **System Security > Access Control (ACL)**.
3. Click **New** to create a new ACL record.
4. Fill in the following fields:
   - **Type**: Select **record**.
   - **Operation**: Choose the 'read' operation.
   - **Name**: Enter the table name (e.g., `sys_dictionary`).
5. In the **Roles** section, add the previously created role (`Microsoft Graph Connector Account`).
6. Click **Submit** to save the ACL.

### Verification

1. Impersonate the user (e.g., `Microsoft Copilot`).
2. Access the target table (e.g., `sys_dictionary`) and confirm that rows are visible. You will notice that though the user can view the rows, the field values are not visible. To grant field-level access, proceed to the next step.

---

## Step 5: Grant Field-Level Access

If the user can view rows but not field values, field-level access must be configured.

1. Navigate to **System Security > Access Control (ACL)**.
2. Click **New** to create a new ACL record.
3. Fill in these fields:
   - **Type**: Select **record**.
   - **Operation**: Choose the 'read' operation.
   - **Name**: Enter the table name (e.g., `sys_dictionary`) and use `*` in the field name to apply to all fields.
4. In the **Roles** section, add the previously created role (`Microsoft Graph Connector Account`).
5. Click **Submit** to save the ACL.

### Final Verification

1. Impersonate the user (e.g., `Microsoft Copilot`).
2. Confirm that both rows and field values within the target table are now visible.

---

By following these steps, you have successfully granted table access to a service account in ServiceNow.