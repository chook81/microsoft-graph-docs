# Scoping Filter

Scoping filter allows one to limit objects which should get provisioned, based on certain conditions. For example, we might want to only provision users which are located in the US. When scoping filter is present, *objects which do not satisfy the filter will be skipped* during synchronization.

Scoping filter is part of [object mapping](synchronization-objectMapping.md). For additional information, also see [Walkthrough: Configuring a scoping filter](synchronization-walkthrough-scopingFilters.md)

Scoping filter consists of several filter group collections. Each group within the collection is evaluated independently, and the results are cobined using logical OR. If any of the clause groups within a collection is satisfied, group collection is considered satisfied. Group consist of one or more clauses - each clause must be satisfied in order for the group to be considered satisfied.

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
|groups                 |filterGroup[]    | Filter conditions used to decide whether given object is in scope for provisioning. This is the filter which should be used in most cases. If an object used to satisfy this filter at a given moment, and then the object or the filter was changed so that filter is not satisfied any longer, such object *will get de-provisioned" |
|inputFilterGroups      |filterGroup[]    | Filter conditions used to filter out objects at the early stage of reading them from the directory. If an object doesn't satisfy this filter it will not be processed further. Important to understand is that if an object used to satisfy this filter at a given moment, and then the object or the filter was changed so that filter is no longer satisfied, such object *will NOT get de-provisioned* |
|categoryFilterGroups   |filterGroup[]    | Filter conditions used to decide whether given object belongs and should be processed as part of this object mapping |

### filterGroup

```json
{
    "clauses": [{"@odata.type":"microsoft.graph.filterClause"}],
    "name": "String"
}
```

| Property      | Type      | Description    |
|:--------------|:----------|:---------------|
|clauses        |filterClause[]    | Collection of filter clauses (conditions) in this  group. All clauses in a group must be satisfied in order for the filter group to evaluate to `true`  |
|name           |string    | Human-friendly name of the filter group|
|clauses        |filterClause[]    | Single clause of a filter|

## JSON Example

```json
{
    "groups": [
        {
            "clauses": [
                {
                    "operatorName": "EQUALS",
                    "sourceOperandName": "dirSyncEnabled",
                    "targetOperand": {
                        "values": [
                            "True"
                        ]
                    }
                }
            ],
            "name": "AD On Premise users"
        }
    ],
    "inputFilterGroups": [],
    "categoryFilterGroups": []
}
```