# Synchronization Template

Synchronization template provides default synchronization configuration for a particular application. Template is controlled by the developer of the application, although anyone can retrieve the template to see the default settings. One of the most important parts of the template is [synchronization schema](synchronization-schema.md), which will be used as default for any job based on this template. Template is required to create a synchronization job.

Application developer may provide multiple templates for a given application, and designate a default one. If multiple templates are available for the application you are interested in, seek application-specific guidance on which one better suits your case.

## JSON Example

```json
{
    "id": "SfOutDelta",
    "default": true,
    "description": null,
    "discoverable": true,
    "factoryTag": "SfOutDelta",
    "metadata": [
        {
            "key": "galleryApplicationIdentifier",
            "value": "cd3ed3de-93ee-400b-8b19-b61ef44a0f29"
        },
        {
            "key": "galleryApplicationKey",
            "value": "salesforce.com"
        },
        {
            "key": "isOAuthEnabled",
            "value": "false"
        },
        {
            "key": "configurationFields",
            "value": "[{\"name\":\"username\"},{\"name\":\"password\",\"secret\":true},{\"name\":\"secrettoken\",\"secret\":true}]"
        }
    ],
    "schema": {...}
}