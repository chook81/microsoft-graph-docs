# Retrieve synchroization template

Retrieve template by its identifier

### HTTP Request

```http
GET applications/{id}/synchronization/templates/{templateId}
GET servicePrincipals/{id}/synchronization/templates/{templateId}
```

## Request headers

| Name           | Type    | Description|
|:---------------|:--------|:-----------|
| Authorization  | string  | Bearer {token}. Required. |

## Request body

Do not supply a request body for this method.

### Response

If successful, this method returns a `200 OK` response code and [synchronizationTemplate](../resources/synchronization_template.md) object in the responce body.

### Example

##### Request
The following is an example of a request.

```http
GET https://graph.microsoft.com/testSynchronization/servicePrincipals/{id}/synchronization/templates/Slack
```

##### Response
The following is an example of a response.
>**Note:** The response object shown here might be shortened for readability. All the properties will be returned in an actual call.

```http
HTTP/1.1 200 OK
{
    "id": "Slack",
    "factoryTag": "CustomSCIM",
    "schema": {
        "directories": [...],
        "synchronizationRules": [...]
        }
}
```