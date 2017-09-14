# Synchronization schema

## Overview

Synchronization schema controls most of the details of the synchronization between 2 directories. On a high level, it defines what objects will be synchronized and how. Most often you will want to customize some of the [attribute mappings](synchronization_attributeMapping.md) to suit your needs, or add a [scoping filter](synchronization_scopingFilter,md) to synchronize only objects which satisfy a certain condition.

## JSON representation

```json
{
  "directories": [{"@odata.type": "microsoft.graph.directoryDefinition"}],
  "metadata": [{"@odata.type": "microsoft.graph.metadataEntry"}],
  "synchronizationRules": [{"@odata.type": "microsoft.graph.synchronizationRule"}],
  "version": "String"
}
```

## Properties

| Property      | Type      | Description    |
|:--------------|:----------|:---------------|
|directories            |[directoryDefinition] collection   |Describes directories and objects which are part of the synchronization [job](#synchronization_job.md) or [template](#synchronization_template.md) |
|metadata               |metadataEntry collection           |Additional extension properties. Unless mentioned explicitly, metadata values should not be changed|
|synchronizationRules   |[synchronizationRule] collection   |Collection of synchronization rules configured for the synchronization [job](#synchronization_job.md) or [template](#synchronization_template.md) |
|version                |String                             |Version of the schema, updated automatically on every schema change|

## Methods

| Method        | Return Type               | Description                  |
|:--------------|:--------------------------|:-----------------------------|
|[Get schema](synchronization_schema_get.md)    |[synchronizationSchema](synchronization_schema.md)   |Retrieves  synchronization schema|
|[Update schema](synchronization_schema_put.md)    |None   |Updates synchronization schema |
|[Delete schema](synchronization_schema_delete.md)    |None   |Deletes customized  schema, effectively resetting the schema to the default configured by application developer |
|[List filter operators](synchronization_schema_filterOperators-get.md)    |filterOperatorSchema[]   |Lists all operators supported in the scoping filters |
|[List attribute mapping functions](synchronization_schema_functions-get.md)    |filterOperatorSchema[]   |Lists all functions supported in the attribute mapping expressions |

## Schema Components

Top-level components in synchronization schema are "directories", defining directories and their objects, and "synchronizationRules", defining mappings between objects and their attributes

### Directory Definitions ("directories")

Directory definitions are part of the [synchronization schema](synchronization_schema.md) and provide synchronization engine information about directories and their objects. It tells synchronization engine, for example, that directory "Azure AD" has objects named "User" and "Group", which attributes are supported for those objects, and what is the type of those attributes. In order for a particular object and attribute to be used in synchronization rules / object mappings, they have to be defined as part of the directory definition. For more information, please see [directory definition](synchronization_directoryDefinition.md)

#### Synchronization Rules ("synchronizationRules")

Synchronization rules area part of the [synchronization schema](synchronization_schema.md) and are at the core of the synchronization setup. They give synchronization engine crucial information regarding how the synchronization should be performed. That includes what objects should be synchronized, how objects from source directory should be matched with objects in target directory, and how attributes should be transformed going from source to target directory. For more information, please see [synchronization rule](synchronization_rule.md)

#### Object mappings ("objectMappings")

Object mappings are the main part of the synchronization rule. Each object mapping defines how a given object should be synchronized from source directory to target. In particular, it defines how object in source directory should be matched with an object in target directory, what (if any) scoping filters should be used to decide if we want to provision a given object, and how object attributes should be transformed going from source to target directory. For more information, please see [object mapping](synchronization_objectMapping.md)


## JSON Example

Example below is shortened for brevity. You can see a full sample of schema [here](synchronization_schema_sample.md).

```json
{
    "directories": [
        {
            "name": "Azure Active Directory",
            "objects": [
                {
                    "name": "User",
                    "attributes": [
                        {
                            "name": "userPrincipalName",
                            "type": "string"
                        },
                        {...}
                    ]
                },
                {...}
            ]
        },
        {
            "name": "Salesforce",
            "objects": [...]
        }
    ],
    "synchronizationRules":[
        {
            "name": "USER_TO_USER",
            "sourceDirectoryName": "Azure Active Directory",
            "targetDirectoryName": "Salesforce",
            "objectMappings": [
                {
                    "sourceObjectName": "User",
                    "targetObjectName": "User",
                    "attributeMappings": [
                        {
                            "source": {...},
                            "targetAttributeName": "userName"
                        },
                        {...}
                    ]
                },
                {...}
            ]
        },
        { ... }
    ]
}
```