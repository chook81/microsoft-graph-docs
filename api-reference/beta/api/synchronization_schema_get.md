# Get synchronization schema

Retrieves effective schema for a given job or template

## HTTP Request

```http
GET /servicePrincipals/{id}/synchronization/jobs/{jobId}/schema
GET /servicePrincipals/{id}/synchronization/templates/{templateId}/schema
GET /applications/{id}/synchronization/templates/{templateId}/schema
```

## Request headers

| Name           | Type    | Description|
|:---------------|:--------|:-----------|
| Authorization  | string  | Bearer {token}. Required. |

## Request body

Do not supply a request body for this method.

## Response

If successful, this method returns a `200 OK` response code and [synchronizationSchema](../resources/synchronization_schema.md) object in the response body.

## Example

##### Request
The following is an example of a request.

```http
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/schema
```

##### Response
The following is an example of a response.
>**Note:** The response object shown here might be shortened for readability. All the properties will be returned in an actual call.

```http
HTTP/1.1 200 OK

{
    "directories": [
        {
            "name": "Azure Active Directory",
            "objects": [
                {
                    "name": "User",
                    "attributes": [
                        {
                            "name": "userPrincipalName",
                            "type": "string"
                        },
                        {...}
                    ]
                },
                {...}
            ]
        },
        {
            "name": "Salesforce",
            "objects": [...]
        }
    ],
    "synchronizationRules":[
        {
            "name": "USER_TO_USER",
            "sourceDirectoryName": "Azure Active Directory",
            "targetDirectoryName": "Salesforce",
            "objectMappings": [
                {
                    "sourceObjectName": "User",
                    "targetObjectName": "User",
                    "attributeMappings": [
                        {
                            "source": {...},
                            "targetAttributeName": "userName"
                        },
                        {...}
                    ]
                },
                {...}
            ]
        },
        { ... }
    ]
}
```
