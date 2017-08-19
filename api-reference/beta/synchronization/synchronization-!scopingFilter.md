# Scoping Filter

Scoping filter allows one to limit objects which should get provisioned, based on certain conditions. For example, we might want to only provision users which are located in the US. When scoping filter is present, *objects which do not satisfy the filter will be skipped* during synchronization.

Scoping filter is part of [object mapping](synchronization-objectMapping.md). Object mapping's filter consists of several collections of filter groups. Each filter group consists of one or more clauses. An object is considered in scope for the group (group is evaluated to `true`) *if, and only if ALL of the clauses of the group are evaluated to `true`*.

An object is considered in scope for the group collection (group collection is evaluated to `true`) *if ANY of the groups in the collection is evaluated to `true`*.

For additional information, also see [Walkthrough: Configuring a scoping filter](synchronization-walkthrough-scopingFilters.md)

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
|groups                 |filterGroup[]    | Filter conditions used to decide whether given object is in scope for provisioning. This is the filter which should be used in most cases. If an object used to satisfy this filter at a given moment, and then the object or the filter was changed so that filter is not satisfied any longer, such object *will get de-provisioned". An object is considered in scope *if ANY of the groups in the collection is evaluated to `true`*. |
|inputFilterGroups      |filterGroup[]    | Filter conditions used to filter out objects at the early stage of reading them from the directory. If an object doesn't satisfy this filter it will not be processed further. Important to understand is that if an object used to satisfy this filter at a given moment, and then the object or the filter was changed so that filter is no longer satisfied, such object *will NOT get de-provisioned*. An object is considered in scope *if ANY of the groups in the collection is evaluated to `true`*. |
|categoryFilterGroups   |filterGroup[]    | Filter conditions used to decide whether given object belongs and should be processed as part of this object mapping. An object is considered in scope *if ANY of the groups in the collection is evaluated to `true`*. |

### filterGroup

Filter group defines a set of clauses which an object must satisfy to be considered in scope. An object is considered in scope for the group (group is evaluated to `true`) *if, and only if ALL of the clauses of the group are evaluated to `true`*.

```json
{
    "clauses": [{"@odata.type":"microsoft.graph.filterGroup"}],
    "name": "String"
}
```

| Property      | Type      | Description    |
|:--------------|:----------|:---------------|
|clauses        |filterClause[]    | Collection of filter clauses (conditions) in this  group. All clauses in a group must be satisfied in order for the filter group to evaluate to `true`  |
|name           |string    | Human-friendly name of the filter group|
|clauses        |filterClause[]    | Single clause of a filter|

### filterClause

Clause represent a single assertion which a candidate object must satisfy, and is evaluated to either `true` (object satisfies the assertion) or `false` (object does not satisfy the assertion).

```json
{
    "operatorName": "String",
    "sourceOperandName": "String",
    "targetOperand":[{"@odata.type":"microsoft.graph.filterOperand"}],
}
```

| Property      | Type      | Description    |
|:--------------|:----------|:---------------|
|operatorName   | String    | Name of the operator to be applied to source and target operands. Must be one fo the supported operators. Supported operators can be discovered |
|sourceOperandName | String    | Name of source operand (operand being tested). Source operand must be one of the attribute names of the source object|
|targetOperand   |filterOperand    | Values that source operand will be tested against|



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