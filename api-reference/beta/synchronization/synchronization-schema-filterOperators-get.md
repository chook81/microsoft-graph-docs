# List filter operators

Lists all operators supported in the scoping filters

## Request

```http
GET /servicePrincipals/{id}/synchronization/jobs/{jobId}/schema/filterOperators
GET /servicePrincipals/{id}/synchronization/templates/{templateId}/schema/filterOperators
GET /applications/{id}/synchronization/templates/{templateId}/schema/filterOperators
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
