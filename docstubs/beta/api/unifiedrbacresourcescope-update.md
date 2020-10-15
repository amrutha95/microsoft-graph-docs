---
title: "Update unifiedRbacResourceScope"
description: ""
localization_priority: Normal
author: "$(metadata.owner)"
ms.prod: "microsoft-identity-platform"
doc_type: "apiPageType"
---

# Update unifiedRbacResourceScope

Namespace: microsoft.graph

[!INCLUDE [beta-disclaimer](../../includes/beta-disclaimer.md)]

Update the properties of an unifiedRbacResourceScope object.

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

In the request body, supply JSON representation of the unifiedRbacResourceScope object.

<!-- Actions and Functions -->

<!-- CRUD Methods -->

The following table shows the properties that are required when you create an unifiedRbacResourceScope object.

| Property    | Type   | Description |
| :---------- | :----- | :---------- |
| displayName | String |             |
| id          | String | Read-only.  |
| scope       | String |             |
| type        | String |             |

## Response

If successful, this method returns a `200 OK` response code and an unifiedRbacResourceScope object in the response body.

## Examples

### Request

<!-- {
  "blockType": "request",
  "name": "update_unifiedrbacresourcescope"
}
-->

```http
PATCH https://graph.microsoft.com/beta

Content-Type: application/json
Content-Length: 137

{
  "@odata.type": "#microsoft.graph.unifiedRbacResourceScope",
  "displayName": "String",
  "scope": "String",
  "type": "String"
}

```

### Response

**Note:** The response object shown here might be shortened for readability.

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "Microsoft.EnterpriseRbac.unifiedRbacResourceScope"
}
-->

```http
HTTP 1.1 200 OK

Content-Type: application/json
{
  "value": {
  "@odata.type": "#microsoft.graph.unifiedRbacResourceScope",
  "displayName": "String",
  "id": "String(identifier)",
  "scope": "String",
  "type": "String"
}
}

```