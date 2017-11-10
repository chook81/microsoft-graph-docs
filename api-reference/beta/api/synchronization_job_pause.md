# Pause synchronization job

Temporarily stops job execution. All the progress and job's state is persisted, and upon [Start](synchronization_job_start.md) call job execution will continue from where it left off

## HTTP Request

```http
POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/pause
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
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/pause
```

##### Response
The following is an example of a response.

```http
HTTP/1.1 204 No Content
```
