# MSON AST Serialization Media Types
This document defines serialization formats for [MSON](https://github.com/apiaryio/mson) abstract syntax tree.

## Version

- **Version**: 1.0
- **Created**: 2014-07-31
- **Updated**: 2014-07-31

## Media Types
Base type media type is `application/vnd.mson.ast`.

### Serialization formats
Two supported, feature-equal serialization formats are JSON and YAML:

- `application/vnd.mson.ast+json`
- `application/vnd.mson.ast+yaml`

## AST Serialization

### Property ([Element]())
Property (member) of a MSON object. Inherits from element.

- `name` (string, required) - Name of the property
- `required` (bool) - Boolean flag to denote required (`true`) or optional (`false`) property

    Note: The optional (`false`) is the default.

#### Example

```json
{ 
    "name": "id",
    "required": true,
    "description": "The unique identifier for a product",
    "type": "number",
    "value": null,
}
```

### Element
Element (item) of a MSON array.

- `description` (string) - Markdown-formatted description of the property
- `type` (string) - The data type of the property

    The type value must be of one of the following:

    - `string`
    - `number`
    - `object`
    - `array`
    - `bool` or `boolean`

- [`Value`]() - The value of the element

#### Example

```json
{
    "description": "User-defined tag for the product",
    "type": "string",
    "value": null,
}
```

### Value
Value of an MSON element (or a property). Note the value is an object whith for the future-compatibility.

- `v` (required, one of) - The actual value of the element or property
    - (object) - Value represented as an object with properties
        - [Property]()
    - (array) - Value represented as an array with elements
        - [Element]()
    - (string) - Value represeneted as a string

#### Examples

```json
{
    "v": "1"
}
```

```json
{
    "v": "home"
}
```

```json
{
    "v": [
        {
            "type": "number",
            "value": {
                "v": "1"
            }
        },
        {
            "type": "number",
            "value": {
                "v": "2"
            }
        }
    ]   
}
```

```json
{
    "v": [
        {
            "name": "id",
            "required": true,            
            "description": "The unique identifier for a product",
            "type": "number",
            "value": {
                "v": "1"
            }
        },
        {
            "name": "name",
            "required": false,
            "description": "Name of the product",
            "type": "string",
            "value": {
                "v": "A green door"
            }
        },
        {
            "name": "price",
            "required": false,
            "description": null,
            "type": null,
            "value": {
                "v": "12.50"
            }
        },
        {
            "name": "tags",
            "required": false,
            "description": null,
            "type": "array",
            "value": {
                "v": [
                    {
                        "description": null,
                        "type": null,
                        "value": {
                            "v": "home"
                        }
                    },
                    {
                        "description": null,
                        "type": null,
                        "value": {
                            "v": "home"
                        }
                    }
                ]
            }
        }  
    ]
}
```
