# Synchronization Schema

## Overview

Synchronization schema controls most of the details of the synchronization between 2 directories. On a high level, it defines what objects will be synchronized and how. Most often you will want to customize some of the [attribute mappings](synchronization-attributeMapping.md) to suit your needs, or add a [scoping filter](synchronization-scopingFilter,md) to synchronize only objects which satisfy a certain condition.

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
|directories            |[directoryDefinition] collection   |Describes directories and objects which are part of the synchronization [job](#synchronization-job.md) or [template](#synchronization-template.md) |
|metadata               |metadataEntry collection           |Additional extension properties. Unless mentioned explicitly, metadata values should not be changed|
|synchronizationRules   |[synchronizationRule] collection   |Collection of synchronization rules configured for the synchronization [job](#synchronization-job.md) or [template](#synchronization-template.md) |
|version                |String                             |Version of the schema, updated automatically on every schema change|

## Schema Components

Top-level components in synchronization schema are "directories", defining directories and their objects, and "synchronizationRules", defining mappings between objects and their attributes

### Directory Definitions ("directories")

Directory definition provides synchronization engine with information about a directory and its objects. It tells synchronization engine, for example, that directory has objects named "User" and "Group", which attributes are supported for those objects, and what is the type of those attributes. In order for a particular object and attribute to be used in synchronization rules / object mappings, they have to be defined as part of the directory definition.

As a general rule, default synchronization schema provided as part of the synchronization template will define most commonly used objects / attributes for that directory. However, if directory supports addition of custom attributes, it is common one would want to expand the default definition with their own custom objects or attributes. For more information, please see [Walk-through: Synchronizing Custom Attributes](synchronization-walkthrough-custom-attributes)

### Synchronization Rules ("synchronizationRules")

Synchronization rules are at the core of the synchronization setup. They give synchronization engine crucial information regarding how the synchronization should be performed. That includes what objects should be synchronized, how objects from source directory should be matched with objects in target directory, and how attributes should be transformed going from source to target directory.

**Synchronization rule defines synchronization in one direction**, from source directory to target directory (source and target directories are designated through synchronizationRule.sourceDirectoryName and synchronizationRule.targetDirectoryName).

### Object mappings ("objectMappings")

Object mappings are the main part of the synchronization rule. Each object mapping defines how a given object should be synchronized from source directory to target.

- sourceObjectName - name of the object from the source directory
- targetObjectName - name of the object in target directory
- scope - scoping filters, which allow to synchronize only objects which satisfy a given filter
- attributeMappings - define which values should be synchronized to target object attributes. A number of functions is available to support transformation of original source values

#### Object matching and matching priority

When synchronization engine attempts to synchronize an object, it needs a way to find corresponding object in the target directory. If


## JSON Example

```json
{
    directories: [
        {
            name: "Azure AD",
            objects: [
                {
                    name: "User",
                    attributes: [
                        {
                            name: "userPrincipalName",
                            type: "string"
                        },
                        {...}
                    ]
                },
                {...}
            ]
        },
        {
            name: "Salesforce",
            objects: [...]
        }
    ],
    synchronizationRules:[
        {
            name: "USER_TO_USER",
            sourceDirectoryName: "Azure AD",
            targetDirectoryName: "Salesforce",
            objectMappings: [
                {
                    sourceObjectName: "User",
                    targetObjectName: "User",
                    attributeMappings: [
                        {
                            source: {...},
                            targetAttributeName: "userName"
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