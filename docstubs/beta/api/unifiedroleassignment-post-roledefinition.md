---
title: "Add roleDefinition"
description: ""
localization_priority: Normal
author: "$(metadata.owner)"
ms.prod: "microsoft-identity-platform"
doc_type: "apiPageType"
---

# Add roleDefinition

Namespace: microsoft.graph

[!INCLUDE [beta-disclaimer](../../includes/beta-disclaimer.md)]

Add unifiedRoleDefinition by posting to the unifiedRoleDefinition collection.

## Permissions

One of the following permissions is required to call this API. To learn more, including how to choose permissions, see [Permissions](/graph/permissions-reference).

| Permission type                        | Permissions (from most to least privileged) |
| :------------------------------------- | :------------------------------------------ |
| Delegated (work or school account)     |                                             |
| Delegated (personal Microsoft account) |                                             |
| Application                            |                                             |

## HTTP request

<!-- {
  "blockType": "ignored"
}
-->

```http

```

## Request headers

| Name          | Description                 |
| :------------ | :-------------------------- |
| Authorization | Bearer {token}. Required.   |
| Content-Type  | application/json. Required. |

## Request Body

In the request body, supply JSON representation of the unifiedRoleDefinition object.

<!-- Actions and Functions -->

<!-- CRUD Methods -->

The following table shows the properties that are required when you create an unifiedRoleDefinition object.

| Property        | Type                                                                      | Description |
| :-------------- | :------------------------------------------------------------------------ | :---------- |
| description     | String                                                                    |             |
| displayName     | String                                                                    |             |
| id              | String                                                                    | Read-only.  |
| isBuiltIn       | Boolean                                                                   |             |
| isEnabled       | Boolean                                                                   |             |
| resourceScopes  | String collection                                                         |             |
| rolePermissions | [unifiedRolePermission](../resources/unifiedrolepermission.md) collection |             |
| templateId      | String                                                                    |             |
| version         | String                                                                    |             |

## Response

If successful, this method returns a `201 Created` response code and an unifiedRoleDefinition object in the response body.

## Examples

### Request

<!-- {
  "blockType": "request",
  "name": "add_roledefinition"
}
-->

```http
POST https://graph.microsoft.com/beta

Content-Type: application/json
Content-Length: 531

{
  "@odata.type": "#microsoft.graph.unifiedRoleDefinition",
  "description": "String",
  "displayName": "String",
  "isBuiltIn": "Boolean",
  "isEnabled": "Boolean",
  "resourceScopes": [
    "String"
  ],
  "rolePermissions": [
    {
      "@odata.type": "#microsoft.graph.unifiedRolePermission",
      "allowedResourceActions": [
        "String"
      ],
      "condition": "String",
      "excludedResourceActions": [
        "String"
      ]
    }
  ],
  "templateId": "String",
  "version": "String"
}

```

### Response

**Note:** The response object shown here might be shortened for readability.

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "Microsoft.EnterpriseRbac.unifiedRoleDefinition"
}
-->

```http
HTTP 1.1 201 Created

Content-Type: application/json
{
  "value": {
  "@odata.type": "#microsoft.graph.unifiedRoleDefinition",
  "description": "String",
  "displayName": "String",
  "id": "String(identifier)",
  "isBuiltIn": "Boolean",
  "isEnabled": "Boolean",
  "resourceScopes": [
    "String"
  ],
  "rolePermissions": [
    {
      "@odata.type": "#microsoft.graph.unifiedRolePermission",
      "allowedResourceActions": [
        "String"
      ],
      "condition": "String",
      "excludedResourceActions": [
        "String"
      ]
    }
  ],
  "templateId": "String",
  "version": "String"
}
}

```