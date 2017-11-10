# Delete synchronization schema

Deletes customized  schema, effectively resetting the schema to the default configured by application developer. If the schema is deleted in the context of the template, it resets the schema to default one associated with the template's `factoryTag`

## HTTP Request

```http
DELETE /servicePrincipals/{id}/synchronization/jobs/{jobId}/schema
DELETE /applications/{id}/synchronization/templates/{templateId}/schema
```

## Request headers

| Name           | Type    | Description|
|:---------------|:--------|:-----------|
| Authorization  | string  | Bearer {token}. Required. |

## Request body

Do not supply a request body for this method.

## Response

If successful, this method returns a `201 No Content` response code. It does not return anything in response body.

## Example

##### Request
The following is an example of a request.

```http
DELETE https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/schema
```

##### Response
The following is an example of a response.

```http
HTTP/1.1 201 No Content
```
