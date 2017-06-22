# Synchronization Status

Current status of the synchronization job

## JSON representation

    <ComplexType Name="synchronizationStatus">
    <Property Name="countSuccessiveCompleteFailures" Type="Edm.Int64" Nullable="false"/>
    <Property Name="escrowsPruned" Type="Edm.Boolean" Nullable="false"/>
    <Property Name="synchronizedEntryCountByType" Type="Collection(microsoft.graph.stringKeyLongValuePair)"/>
    <Property Name="code" Type="microsoft.graph.synchronizationStatusCode" Nullable="false"/>
    <Property Name="lastExecution" Type="microsoft.graph.synchronizationTaskExecution"/>
    <Property Name="lastSuccessfulExecution" Type="microsoft.graph.synchronizationTaskExecution"/>
    <Property Name="lastSuccessfulExecutionWithExports" Type="microsoft.graph.synchronizationTaskExecution"/>
    <Property Name="steadyStateFirstAchievedTime" Type="Edm.DateTimeOffset" Nullable="false"/>
    <Property Name="steadyStateLastAchievedTime" Type="Edm.DateTimeOffset" Nullable="false"/>
    <Property Name="quarantine" Type="microsoft.graph.synchronizationQuarantine"/>
    <Property Name="troubleshootingUrl" Type="Edm.String" Unicode="false"/>
    </ComplexType>
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
|countSuccessiveCompleteFailures        |Number        |Number of consecutive times this job failed|
|escrowsPruned                          |Boolean     |`true` if the job's escrows (object-level errors) were pruned during initial synchronization. Escrows can be pruned if during initial synchronization we reach the threshold of errors which would normally put the job in quarantine. Instead of going into quarantine, synchronization opts out to clear job's errors and continue until the initial synchronization is completed. Once initial synchronization is completed, job will be paused and wait for customer's manual intervention to clean up the errors  |
|synchronizedEntryCountByType           |KeyValuePair<string,integer> collection     |Count of synchronized objects, listed by object type|
|code                                   |[statusCode](synchronization-statusCode.md)  |Job's current status code|
|lastExecution                          |[synchronizationJobExecution](synchronization-jobExecution.md)    |Details of the last execution of the job|
|lastSuccessfulExecution       |[synchronizationJobExecution](synchronization-jobExecution)        |Details of the last execution of this job, which didn't have any errors|
|lastSuccessfulExecutionWithExports     |[synchronizationJobExecution](synchronization-jobExecution.md)        |Details of the last execution of the job, which exported objects into the target directory|
|steadyStateFirstAchievedTime           |DateTimeOffset        |Time when steady state (no more changes to process) was first achieved|
|steadyStateLastAchievedTime            |DateTimeOffset        |Time when steady state (no more changes to process) was last achieved|
|quarantine     |[synchronizationQuarantine](synchronization-quarantine.md)        |If job is in quarantine, quarantine details|


## JSON Example

```json
{
    TODO
}
```