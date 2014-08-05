# MSON AST Serialization Media Types
This document defines serialization formats for [MSON](https://github.com/apiaryio/mson) abstract syntax tree.

## Version

- **Version**: 1.0
- **Created**: 2014-07-31
- **Updated**: 2014-08-04

## Media Types
Base type media type is `application/vnd.mson.ast`.

### Serialization formats
Two supported, feature-equal serialization formats are JSON and YAML:

- `application/vnd.mson.ast+json`
- `application/vnd.mson.ast+yaml`

## AST Serialization

### Element Object
Element (item) of a MSON array.

- `description` (string) - Markdown-formatted description of the property
- `values` (array)
    - [Value](https://github.com/apiaryio/mson-ast#value-object)

#### Example

```json
{
    "description": "User-defined tag for the product",
    "type": "string",
    "values": null,
}
```

### Property Object
Property (member) of a MSON object. Inherits from element.

- `name` (string, required) - Name of the property
- `required` (bool) - Boolean flag to denote required (`true`) or optional (`false`) property

    The default is `false` (_optional_ property)
- Inherits [Element](https://github.com/apiaryio/mson-ast#element-object)

#### Example

```json
{
    "name": "id",
    "required": true,
    "description": "The unique identifier for a product",
    "type": "number",
    "values": null,
}
```

### Value Object
Value of an MSON element (or a property).

- `type` (string) - The data type of the property

    The type value must be of one of the following:

    - `string`
    - `number`
    - `object`
    - `array`
    - `bool` or `boolean`

- `v` (required) - The actual value of the element or property

    - One of

        - (string) - The value represeneted as a string
        - (array) - The value represented as an array of elements or properties

            Note: In addition to type introspection the type of values stored in the array can be also inferred from the `type` porperty of the parent element.

            - One of

                - [Element](https://github.com/apiaryio/mson-ast#element-object)
                - [Property](https://github.com/apiaryio/mson-ast#property-object)

#### Examples

##### String Value

```json
{
    "type": "number",
    "v": "1"
}
```

```json
{
    "type": "string",
    "v": "home"
}
```

##### Array Value (Object Data Type)

```json
{
    "type": "object",
    "v": [
        {
            "name": "id",
            "required": true,
            "description": "The unique identifier for a product",
            "values": [
                {
                    "type": "number",
                    "v": "1"
                }
            ]
        }
    ]
}
```

##### Array Value (Array Data Type)

```json
{
    "type": "array",
    "v": [
        {
            "description": null,
            "values": [
                {
                    "type": null,
                    "v": "home"
                }
            ]
        },
        {
            "description": null,
            "values": [
                {
                    "type": null,
                    "v": "green"
                }
            ]
        }
    ]
}
```

##### Complex Example

```json
{
    "type": "object"
    "v": [
        {
            "name": "id",
            "required": true,
            "description": "The unique identifier for a product",
            "values": [
                {
                    "type": "number",
                    "v": "1"
                }
            ]
        },
        {
            "name": "name",
            "required": false,
            "description": "Name of the product",
            "values": [
                {
                    "type": "string",
                    "v": "A green door"
                }
            ]
        },
        {
            "name": "price",
            "required": false,
            "description": null,
            "values": [
                {
                    "type": null,
                    "v": "12.50"
                }
            ]
        },
        {
            "name": "tags",
            "required": false,
            "description": null,
            "values": [
                {
                    "type": "array",
                    "v": [
                        {
                            "description": null,
                            "value": {
                                "type": null,
                                "v": "home"
                            }
                        },
                        {
                            "description": null,
                            "value": {
                                "type": null,
                                "v": "green"
                            }
                        }
                    ]
                }
            ]
        }
    ]
}
```
