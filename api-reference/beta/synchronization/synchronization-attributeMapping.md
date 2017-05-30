# Attribute Mapping

Describes an object and its attributes. Object definitions are part of [directoryDefinition](synchronization-directoryDefinition.md), which is updated as part of [synchronizationSchema](synchronization-schema.md)

## JSON representation

```json
{
  "attributes": [{"@odata.type": "microsoft.graph.attributeDefinition"}],
  "metadata": [{"@odata.type": "microsoft.graph.metadataEntry"}],
  "name": "String"
}
```

## Properties

| Property      | Type      | Description    |
|:--------------|:----------|:---------------|
|metadata       |metadataEntry collection    |Additional extension properties. Unless mentioned explicitly, metadata values should not be changed|
|name           |String     |Name of the object. Must be unique within a directory definition. Not nullable|


## JSON Example

```json
{
    "metadata": [
        {
            "key": "IsSoftDeletionSupported",
            "value": "true"
        }
    ],
    "name": "User"
},
```