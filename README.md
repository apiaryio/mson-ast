# MSON AST Serialization Media Types
This document defines serialization formats for [MSON](https://github.com/apiaryio/mson) abstract syntax tree.

## Version

- **Version**: 1.0
- **Created**: 2014-07-31
- **Updated**: 2014-08-05

## Media Types
Base type media type is `application/vnd.mson.ast`.

### Serialization formats
Two supported, feature-equal serialization formats are JSON and YAML:

- `application/vnd.mson.ast+json`
- `application/vnd.mson.ast+yaml`

## AST Serialization

### Element Object

- `description` (string) - Description of the value
- `primitive` - Primitive data type value for the element
    - `type` (string) - The data type of the element value

        The type value must be of one of the following:

        - `string`
        - `number`
        - `object`
        - `array`
        - `bool` or `boolean`

    - `value` - The actual value of the element

        - One of

            - (string) - The value represented as a string
            - (array) - The value represented as an array of elements or properties

                Note: In addition to type introspection the type of values stored in the array, the type can be also inferred from the `type` porperty of the parent element.

                - Elements

                    - One of

                        - ([Element]())
                        - ([Property]())

- `oneOf` (array) - The choice of values for the element

    - One of

        - ([Element]())
        - ([Property]())

- `ref` (string) - Reference to another element's value

### Property Object

- `name` (string, required) - Name of the property
- `required` (bool) - Boolean flag to denote required (`true`) or optional (`false`) property
- `templated` (bool) - Boolean flag to denote whether the name is a variable (`true`) or not (`false`)
- Inherits [Element]()

## AST Model
![AST Model Diagram](https://raw.githubusercontent.com/apiaryio/mson-ast/master/assets/astmodel.png)

## Example

### MSON

```
# Object
Hello World!

## Attributes
- id: 42 (number, required) - lorem ipsum
- One of
    - a: good
    - b: bad
- Inherits [Another]()
- mixed_feelings
    - One of
        - (number)
        - (string)
- vector (array)
    - 1
    - 2
    - 3
```

### MSON AST
`application/vnd.mson.ast+json`

```json
{
    "name": "Object",
    "description": "Hello World!",
    "primitive" : {
        "type": "object",
        "value": [
            {
                "name": "id",
                "required": true,
                "description": "description",
                "primitive" : {
                    "type": "number",
                    "value": "42"
                }
            },
            {
                "oneOf" : [
                    {
                        "name": "a",
                        "required": false,
                        "primitive" : {
                            "value": "good"
                        }
                    },
                    {
                        "name": "b",
                        "required": false,
                        "primitive" : {
                            "value": "bad"
                        }
                    }
                ]
            }, 
            {
                "reference": "Another"
            },
            {
                "name": "mixed_feelings",
                "required": false,
                "oneOf" : [
                    {
                        "primitive" : {
                            "type": "number",
                        }
                    },
                    {
                        "primitive" : {
                            "type": "string",
                        }
                    },
                ]
            }
            {
                "name": "vector",
                "required": false,
                "primitive" : {
                    "type": "array",
                    "value": [
                        {
                            "primitive" : {
                                "value": "1"
                            }
                        },
                        {
                            "primitive" : {
                                "value": "2"
                            }
                        },
                        {
                            "primitive" : {
                                "value": "3"
                            }
                        }
                    ]
                }
            }
        ]
    }
}

```