# Delete synchronization job

Stops the synchronization job, and permanently deletes all the state associated with it. Synchronizaed accounts remain as-is.

## HTTP Request

```http
DELETE /servicePrincipals/{id}/synchronization/jobs/{jobId}/
```

## Request headers

| Name           | Type    | Description|
|:---------------|:--------|:-----------|
| Authorization  | string  | Bearer {token}. Required. |

## Request body

Do not supply a request body for this method.

## Response

If successful, returns `204 No Content` response.

## Example

##### Request
The following is an example of a request.

```http
DELETE https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/
```

##### Response
The following is an example of a response.

```http
HTTP/1.1 204 No Content
```
