## Create a template

Create new template for a given application

### Request

```http
POST /synchronization/templates/
Content-type: application/json
{
    "id": "Slack",
    "factoryTag": "CustomSCIM"
}
```

### Request body

Body should contain [synchronizationTemplate](synchronization-template.md) object to be created. Only `id` and `factoryTag` properties are required. When no `schema` is provided with the template, default schema associated with `factoryTag` will be used

### Response

If successful, returns `401 Created` response

### Example

#### Sample request

```http
POST https://graph.microsoft.com/testSynchronization/applications/{id}/synchronization/templates
Content-type: application/json
{ 
    "id": "SCIM-Test1",
    "factoryTag: "CustomSCIM"
}
```

#### Sample response

```http
HTTP/1.1 201 Created
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
