# List supported functions

Lists all functions supported in the attribute mapping expressions.

## Request

```http
GET /servicePrincipals/{id}/synchronization/jobs/{jobId}/schema/functions
GET /servicePrincipals/{id}/synchronization/templates/{templateId}/schema/functions
GET /applications/{id}/synchronization/templates/{templateId}/schema/functions
```

## Response

If successful, returns `200 OK` response with a collection of [filterOperatorSchema](synchronization-filterOperatorSchema.md) in the response body.

## Example

### Sample request

```http
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/schema/filterOperators
```

### Sample response

Response below is shortened for brevity.

```json
HTTP/1.1 200 OK

TODO

```
