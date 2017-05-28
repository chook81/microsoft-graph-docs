# Synchronization Overview

## Synchronization API Object Model

### Synchronization Template

Synchronization template provides default synchronization settings for a particular application. Template is controlled by the developer of the application, although anyone can retrieve the template to see the default settings. One of the most important settings template provides is synchronization schema, which will be used as default one for any synchronization job based on this template. Template is required to create a synchronization job.

Application developer may provide multiple templates for a given application, and designate a default one. If multiple templates are available for the application you are interested in, seek application-specific guidance on which one better suits your case.

### Synchronization Job

Synchronization job performs actual synchronization by periodically running in the background, polling for changes in one directory and pushing them to another directory. Synchronization job is always specific to a particular instance of an application in your tenant. As part of synchronization job setup, generally you would need to give authorization to read/write objects in your target directory, and customize job's synchronization schema to suit your needs.

### Synchronization Schema

Synchronization schema controls all the details of a particular synchronization setup. On a high level, it defines what objects will be synchronized and how. Most often you will want to customize some of the attribute mappings to suit your needs, or add a scoping filter to synchronize only objects which satisfy a certain condition. For more information, please see [synchronization schema](synchronization-schema-overview.md)