# Start synchronization job

Starts the synchronization job (job must already exist). If job is in paused state, it will continue processing changes from the point where it was paused. If job is in quarantine, quarantine status will be cleared.

## HTTP Request

```http
POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/start
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
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/start
```

##### Response
The following is an example of a response.

```http
HTTP/1.1 204 No Content
```
