# Synchronization Overview

## Synchronization API Object Model

### Synchronization Template

Synchronization template provides default synchronization settings for a particular application. Template is controlled by the developer of the application, although anyone can retrieve the template to see the default settings. One of the most important settings template provides is synchronization schema, which will be used as default one for any synchronization job based on this template. Template is required to create a synchronization job.

Application developer may provide multiple templates for a given application, and designate a default one. If multiple templates are available for the application you are interested in, seek application-specific guidance on which one better suits your case.

### Synchronization Job

Synchronization job performs actual synchronization by periodically running in the background, polling for changes in one directory and pushing them to another directory. Synchronization job is always specific to a particular instance of an application in your tenant. As part of synchronization job setup, generally you would need to give authorization to read/write objects in your target directory, and customize job's synchronization schema to suit your needs.

### Synchronization Schema

Synchronization schema controls all the details of a particular synchronization setup. On a high level, it defines what objects will be synchronized and how. Most often you will want to customize some of the attribute mappings to suit your needs, or add a scoping filter to synchronize only objects which satisfy a certain condition. For more information, please see [synchronization schema](synchronization-schema-overview.md)

#### Directory Definitions

Directory definition provides synchronization engine with information about a directory and its objects. It tells synchronization engine, for example, that directory has objects named "User" and "Group", which attributes are supported for those objects, and what is the type of those attributes. In order for a particular object and attribute to be used in synchronization rules / object mappings, they have to be defined as part of the directory definition.

As a general rule, default synchronization schema provided as part of the synchronization template will define most commonly used objects / attributes for that directory. However, if directory supports addition of custom attributes, it is common one would want to expand the default definition with their own custom objects or attributes. For more information, please see [Walk-through: Synchronizing Custom Attributes](synchronization-walkthrough-custom-attributes)

#### Synchronization Rules

Synchronization rules are at the core of the synchronization setup. They give synchronization engine crucial information regarding how the synchronization should be performed. That includes what objects should be synchronized, how objects from source directory should be matched with objects in target directory, and how attributes should be transformed going from source to target directory.

**Synchronization rule defines synchronization in one direction**, from source directory to target directory (source and target directories are designated through synchronizationRule.sourceDirectoryName and synchronizationRule.targetDirectoryName).

#### Object mappings

Object mappings are the main part of the synchronization rule. Each object mapping defines how a given object should be synchronized from source directory to target.

- sourceObjectName - name of the object from the source directory
- targetObjectName - name of the object in target directory
- scope - scoping filters, which allow to synchronize only objects which satisfy a given filter
- attributeMappings - define which values should be synchronized to target object attributes. A number of functions is available to support transformation of original source values

#### Object matching and matching priority

When synchronization engine attempts to synchronize an object, it needs a way to find corresponding object in the target directory. If