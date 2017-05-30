# Attribute Definition

Describes an attribute of an object

## JSON representation

```json
{
    "anchor": "Boolean",
    "caseExact": "Boolean",
    "defaultValue": null,
    "metadata": [{"@odata.type":"microsoft.graph.metadataEntry"}],
    "multivalued": "Boolean",
    "mutability": "ReadWrite",
    "name": "mail",
    "required": "Boolean",
    "referencedObjects": [{"@odata.type":"microsoft.graph.referencedObject"}],
    "type": "String"
}
```

## Properties

| Property      | Type      | Description    |
|:--------------|:----------|:---------------|
|anchor         |Boolean    | Defines if attribute is an anchor. Anchor attribute must have unique value identifying an object, and must be immutable. Default is `false`. One of the object's attributes must be designated as the anchor to support synchronization |
|caseExact      |Boolean    |`true` if value of this attribute should be treated as case-sensitive. This setting affects comparison of attribute values, for example when synchronization engine tries to detect changes to attributes
|metadata       |metadataEntry[]    |Additional extension properties. Unless mentioned explicitly, metadata values should not be changed|
|multivalued    |Boolean    |`true` if attribute can have multiple values. Default is `false`|
|mutability     |String     |Defines if attribute is read-only (`Read`), write-only (`Write`) or both readable and writable (`ReadWrite`). Default is `ReadWrite`|
|name           |String     |Name of the attribute. Must be unique for a given object. Not nullable|
|required       |Boolean    |`true` if attribute is required. Object can not be created if any of the required attributes are missing. If during synchronization required attribute has no value, default value will be used. If default value was not set, synchronization will record an error|
|referencedObjects|[referencedObject](synchronization-referencedObject.md) |For attributes with `reference` type, lists referenced objects (i.e. `manager` attribute would list `User` as referenced object)|
|type           |String     |Attribute value type. Supported values are `String`, `Reference`, `Binary`, `TODO`. Default is `String`|

## JSON Example

```json
"attributes": [
    {
        "anchor": false,
        "caseExact": false,
        "defaultValue": null,
        "metadata": [],
        "multivalued": false,
        "mutability": "ReadWrite",
        "name": "mail",
        "required": false,
        "referencedObjects": [],
        "type": "String"
    },
    {
        "anchor": false,
        "caseExact": false,
        "defaultValue": null,
        "metadata": [],
        "multivalued": false,
        "mutability": "ReadWrite",
        "name": "manager",
        "required": false,
        "referencedObjects": [
            {
                "referencedObjectName": "User",
                "referencedProperty": null
            }
        ],
        "type": "Reference"
    },
]
```