# <s>Application Developer API</s>

Methods listed here are intended to be used by developers creating provisioning applications. Given an application registered in Microsoft Graph, owner (developer) of that application will be able to configure synchronization template for it. Template contains information required to synchronize identities into that application, such as which objects and which attributes are supported by the application, default attribute mappings, etc.

Read-only template methods (List Templates, Get Template by Id) are supported in the context of both application and service principal. Creating and changing templates is only supported in the context of a given application.  Root resource:

* Application context:

```http
https://graph.microsoft.com/<api-version>/applications/<AppId>/synchronization/templates/
```

* Service principal context:

```http
https://graph.microsoft.com/<api-version>/applicaservicePrincipals/<ServicePrincipalId>/synchronization/templates/
```

## List existing templates

List templates associated with a given application or service principal

### Request

```http
GET /synchronization/templates
```

### Response

If successful, returns 200 OK response and collection of [synchronizationTemplate](synchronization-template.md) objects

### Example

Sample request

```http
GET https://graph.microsoft.com/testSynchronization/applications/{id}/synchronization/templates
```

Sample response

```javascript
HTTP/1.1 200 OK
{
    "value": [
        {
            "id": "Slack",
            "factoryTag": "CustomSCIM",
            "schema": {
                    "directories": [...],
                    "synchronizationRules": [...]
                    }
        }
    ]
}
```

## Retrieve template

Retrieve template by its identifier

### Request

```http
GET /synchronization/templates/{templateId}
```

### Response

If successful, returns `200 OK` response with [synchronizationTemplate](synchronization-template.md) object. If template with given id is not fount, returns `404 Not Found`.

### Example

Sample request

```http
GET https://graph.microsoft.com/testSynchronization/applications/{id}/synchronization/templates/{templateId}
```

Sample response

```javascript
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


## Update template

Update (override) synchronization template associated with a given application

### Request

```http
PUT /synchronization/templates/{templateId}
Authorization: Bearer <token>
Content-type: application/json

{
        "id": "Slack",
        "applicationId": "{id}",
        "factoryTag": "CustomSCIM",
        "schema": {
            "directories": [...],
            "synchronizationRules": [...]
        }
}
```

### Request body

Body should contain [synchronizationTemplate](synchronization-template.md) with desired updates. Make sure all properties are provided, as missing properties will be erased

### Response

If successful, returns `204 No Content` response

### Examples

#### Request

```http
PUT https://graph.microsoft.com/testSynchronization/applications/{id}/synchronization/templates/{templateId}
Authorization: Bearer <token>
Content-type: application/json

{
    "id": "Slack",
    "applicationId": "{id}",
    "factoryTag": "CustomSCIM",
    "schema": {
        "directories": [...],
        "synchronizationRules": [...]
    }
}
```

#### Response

```http
HTTP/1.1 204 No Content
```


