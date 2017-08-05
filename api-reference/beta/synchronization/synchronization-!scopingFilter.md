# Scoping Filter

Scoping filter specifies conditions which an object should satisfy in order to be synchronized. For example, we might want to only provision users which are located in US. When scoping filter is present, *objects which do not satisfy the filter will be be skipped* during synchronization.

Scoping filter is part of [object mapping](synchronization-objectMapping.md). For additional information, also see [Walkthrough: Configuring a scoping filter](synchronization-walkthrough-scopingFilters.md)

## JSON representation

```json
{
    "groups": [{"@odata.type":"microsoft.graph.filterGroup"}],
    "inputFilterGroups": [{"@odata.type":"microsoft.graph.filterGroup"}],
    "categoryFilterGroups": [{"@odata.type":"microsoft.graph.filterGroup"}],
}
```

## Properties

| Property      | Type      | Description    |
|:--------------|:----------|:---------------|
|groups                 |filterGroup    | Scoping filter conditions used to decide whether given object is in scope for provisioning  |
|inputFilterGroups      |filterGroup    | Scoping filter used to filter out objects at the early stage of reading them from the directory. If an object doesn't satisfy this filter it won't be processed further, in particular it will not be de-provisioned in case it is already provisioned |
|categoryFilterGroups   |filterGroup    | Scoping filter conditions used to decide whether given object is in scope for provisioning  |
## JSON Example

```json
TODO
```