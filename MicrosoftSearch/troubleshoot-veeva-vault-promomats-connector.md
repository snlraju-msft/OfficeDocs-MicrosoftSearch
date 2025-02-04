--- 
ms.date: 08/28/2024 
title: "Troubleshooting the Veeva Vault - PromoMats Microsoft Graph connector" 
ms.author: dannyyao
author: dannyyaou
manager: jecui
audience: Admin 
ms.audience: Admin 
ms.topic: article 
ms.service: mssearch 
ms.localizationpriority: medium 
description: "Troubleshooting the Veeva Vault - PromoMats Graph connector for Microsoft Search and Microsoft 365 Copilot" 
--- 

# Troubleshooting the Veeva Vault - PromoMats Microsoft Graph connector  

| Error                   | Description                               | Resolution                                                                 |
|-------------------------|-------------------------------------------|---------------------------------------------------------------------------|
| `INVALID_SESSION_ID`    | Authentication session expired or invalid.| Reauthenticate with valid credentials.                                   |
| `INSUFFICIENT_ACCESS`   | User lacks permissions to access files.   | Verify user roles and ACLs in Veeva Vault.                               |
| `API_LIMIT_EXCEEDED`    | Too many API requests made in a short period. | Adjust crawl frequency or retry after some time.                        |
| **Missing Properties or Documents** | Required metadata properties are not enabled. | Ensure metadata properties are enabled in Veeva Vault and test retrieval.|

To view more error types, select the connection and click **error details** > **error code**. For more information, see [Monitor your connections](./manage-connector.md).
