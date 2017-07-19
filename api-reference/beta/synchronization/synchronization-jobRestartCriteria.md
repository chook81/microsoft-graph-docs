# Synchronization Job

Synchronization job performs synchronization by periodically running in the background, polling for changes in one directory and pushing them to another directory. Synchronization job is always specific to a particular instance of an application in your tenant. As part of the synchronization job setup, generally you would need to give authorization to read/write objects in your target directory, and customize job's synchronization schema to suit your needs.

## JSON representation

```json
{
    "id": "String (identifier)",
    "metadata": [{"@odata.type": "microsoft.graph.metadataEntry"}],
    "schema": {"@odata.type": "microsoft.graph.synchronizationSchema"},
    "status": {"@odata.type": "microsoft.graph.synchronizationStatus"},
    "templateId": "String (identifier)",
}
```

## Properties

| Property      | Type      | Description    |
|:--------------|:----------|:---------------|
|id             |String                     |Unique synchronization job identifier|
|metadata       |metadataEntry collection   |Additional extension properties. Unless mentioned explicitly, metadata values should not be changed|
|schema         |[synchronizationSchema](synchronization-schema.md)     |Synchronization schema configured for the job|
|status         |[synchronizationStatus](synchronization-status.md)     |Status of the job, which includes when the job was last executed, current job state and errors|
|templateId     |String    |Identifier of the [synchronization template](synchronization-template.md) this job is based on|

## Methods

| Method        | Return Type               | Description                  |
|:--------------|:--------------------------|:-----------------------------|
|[List](synchronization-jobs-get.md)       |[synchronizationJob](synchronization-job.md) collection  |List existing jobs for a given application instance (service principal)|
|[Get](synchronization-job-get.md)         |[synchronizationJob](synchronization-job.md)   |Retrieve existing job and its properties|
|[Create](synchronization-jobs-post.md)    |[synchronizationJob](synchronization-job.md)   |Create new job for a given application|
|[Start](synchronization-job-start.md)     |None   |Starts synchronization job. If job is in paused state, it continues from the point where job was paused. If job is in quarantine, quarantine status is cleared|
|[Restart](synchronization-job-restart.md) |None   |Forces job to start from scratch and re-process all the objects in the directory|
|[Pause](synchronization-job-pause.md)     |None   |Temporarily stops job execution. All the progress and job's state is persisted, and upon [Start](synchronization-job-start.md) call job execution will continue from where it left off|
|[Delete](synchronization-job-delete.md)   |None   |Stops synchronization job, and permanently deletes all the state associated with the job|

## JSON Example

```json
{
    TODO
}
```