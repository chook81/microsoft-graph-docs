# Attribute Definition

Represents an attribute of an object. Defines such attribute properties as name and value type


## JSON representation

```json
{
  "name": "string",
  "type": "string",
  ...
}
```

## Properties

| Property     | Type |Description|
|:---------------|:--------|:----------|
|name           |String     |Name of the attribute. Must be unique for a given object. Not nullable|
|type           |String     |Valid values are `string`, `reference`, `TODO`. Default is `string`|
|multiValued    |String     |Defines if attribute can have multiple values. Default is `false`|

## Example

```json
TODO
```