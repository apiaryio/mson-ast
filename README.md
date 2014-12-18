# MSON AST Serialization Media Types
This document defines serialization formats for [MSON][] abstract syntax tree.

## Version

- **Version**: 2.0
- **Created**: 2014-07-31
- **Updated**: 2014-12-18

## Media Types
Base type media type is `application/vnd.mson.ast`.

### Serialization formats
Two supported, feature-equal serialization formats are JSON and YAML:

- `application/vnd.mson.ast+json`
- `application/vnd.mson.ast+yaml`

## AST Serialization
Following is description of MSON AST serializations data structures using the [MSON][] syntax. 

> **NOTE:** Refer to the [MSON Specification][] for the explanation of terms used throughout this document. 

---

### Document (object)
Top-level MSON document or block. 

#### Properties
- `types` (array[[Named Type][]]) - List of top-level [Named Types][] described in the document

### Named Type (object)
User-defined named type.

#### Properties
- `name` ([Type Name][]) - Name of the type being defined
- `typeDefinition` ([Type Definition][]) - The ancestor type definition
- `sections` (array[[Type Section][]]) - Ordered list of type sections

### Type Name (enum)
Base or named type's name.

#### Members
- `boolean` (string)
- `string` (string)
- `number` (string)
- `array` (string)
- `enum` (string)
- `object` (string)
- ([Symbol][])

### Symbol (object)
Type symbol (identifier).

#### Properties
- `literal` ([Literal][]) - Name of the symbol
- `variable`: `false` (boolean, default) - Boolean flag to denote [Variable Type Name][], `true` for variable type name, `false` otherwise

### Type Definition (object)
Definition of an instance value type.

#### Properties
- `typeSpecification` (object)
    - `name` ([Type Name][]) - Name of the value type in an MSON instance
    - `nestedTypes` (array[[Type Name][]]) - Array of nested value types, applicable only for types of an `array` or `enum` base type

- `attributes` (array) - List of attributes associated with the type
    - (enum[string])
        - `required`
        - `optional`
        - `default`
        - `sample`
        - `fixed`

### Type Section (object)
Section of a type. The section can be any of the [Type Sections][] as described in the MSON Specification. 

#### Properties
- `class` (enum[string]) - Denotes the class of the type section
    - `blockDescription` - Section is a markdown block description
    - `memberType` - Section holds member type(s) elements
    - `sample` - Section defines alternate value(s) for member types
    - `default` - Section defines the default value(s) for member types
    - `validation` - Reserved for future use

- `content` (enum) - Content of the section based on its class
    - ([Markdown][]) - Markdown formatted content of the section (for class `blockDescription`)
    - ([Literal][]) - Literal value for a sub-type of a primitive base type (for `sample` or `default` classes)
    - ([Elements][]) - Section elements for a sub-type of a structured base type (for `memberType`, `sample` or `default` classes)

### Element (object)
An element of a type section. The element holds either a member type (`value` or `property`), [Mixin][] type, [One Of][] type or groups elements in a collection of [Elements][].

#### Properties
- `class` (enum[string]) - Class of the member object
    - `property` - Property member type
    - `value` - Value member type
    - `mixin` - Mixin type
    - `oneOf` - One Of type
    - `group` - Group of other elements

- `content` (enum)
    - ([Property Member][]) - Property member type (for class `property`)
    - ([Value Member][]) - Value member type (for class `value`)
    - ([Mixin][]) - Mixin type (for class `mixin`)
    - ([One Of][]) - One Of type (for class `oneOf`)
    - ([Elements][]) - Group of elements (for class `group`)

### Elements (array[[Element][])
Collection of elements.

### Property Member ([Value Member][])
Individual member of an `object` type structure.

#### Properties
- `name` ([Property Name][]) - Name of the object property

### Property Name (object)
Name of a property member.

#### Properties
- One Of
    - `literal` ([Literal][]) - Literal name of the property
    - `variable` ([Value Definition][]) - Variable name of the property

### Value Member (object)
Individual member of an `array` or `enum` type structure.

#### Properties
- `description` ([Markdown][]) - Inline description of the member type
- `valueDefinition` ([Value Definition][]) - The definition of the member's value
- `sections` (array[[Type Section][]]) - List of member's type sections

### Mixin ([Type Definition][])
Mixin type.

### One Of ([Elements][])
One Of type. List of mutually exclusive elements.

Note the only allowed [Element][] classes are are `property`, `mixin`, `oneOf` and `elements`.

### Value Definition (object)
Value definition of a type instance.

#### Properties
- `values` (array[[Value][]]) - List of values specified in the definition
- `typeDefinition` ([Type Definition][]) - Type of the value

### Value (object)
Sample or actual value of a type instance

#### Properties 
- `literal` ([Literal][]) - The literal value
- `variable`: `false` (boolean, default) - `true` to denote variable value, `false` otherwise

### Markdown (string)
Markdown formatted plain text string.

### Literal (string)
Literal value in the form of a plain-text.

---

## Example 

### MSON

```
- id: 1 (required)
- name: A green door
- price: 12.50 (number)
- tags: home, green
- vector (array)
    - 1
    - 2
    - 3
```

### application/vnd.mson.ast+json

```json
{
  "types": [
    {
      "name": null,
      "typeDefinition": {
        "typeSpecification": {
          "name": "object"
        }
      },
      "sections": [
        {
          "class": "memberType",
          "content": [
            {
              "class": "property",
              "content": {
                "name": {
                  "literal": "id"
                },
                "valueDefinition": {
                  "values": [
                    {
                      "literal": "1"
                    }
                  ],
                  "typeDefinition": {
                    "attributes": [
                      "required"
                    ]
                  }
                }
              }
            },
            {
              "class": "property",
              "content": {
                "name": {
                  "literal": "name"
                },
                "valueDefinition": {
                  "values": [
                    {
                      "literal": "A green door"
                    }
                  ]
                }
              }
            },
            {
              "class": "property",
              "content": {
                "name": {
                  "literal": "price"
                },
                "valueDefinition": {
                  "values": [
                    {
                      "literal": "12.50"
                    }
                  ],
                  "typeDefinition": {
                    "typeSpecification": {
                      "name": "number"
                    }
                  }
                }
              }
            },
            {
              "class": "property",
              "content": {
                "name": {
                  "literal": "tags"
                },
                "valueDefinition": {
                  "values": [
                    {
                      "literal": "home"
                    },
                    {
                      "literal": "green"
                    }
                  ]
                }
              }
            },
            {
              "class": "property",
              "content": {
                "name": {
                  "literal": "vector"
                },
                "valueDefinition": {
                  "typeDefinition": {
                    "typeSpecification": {
                      "name": "array"
                    }
                  }
                },
                "sections": [
                  {
                    "class": "memberType",
                    "content": [
                      {
                        "class": "value",
                        "content": {
                          "valueDefinition": {
                            "values": [
                              {
                                "literal": "1"
                              }
                            ]
                          }
                        }
                      },
                      {
                        "class": "value",
                        "content": {
                          "valueDefinition": {
                            "values": [
                              {
                                "literal": "2"
                              }
                            ]
                          }
                        }
                      },
                      {
                        "class": "value",
                        "content": {
                          "valueDefinition": {
                            "values": [
                              {
                                "literal": "3"
                              }
                            ]
                          }
                        }
                      }
                    ]
                  }
                ]
              }
            }
          ]
        }
      ]
    }
  ]
}
```

[MSON]: https://github.com/apiaryio/mson
[MSON Specification]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md
[Type Sections]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#4-type-sections
[Named Types]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#22-named-types
[Variable Type Name]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#3521-variable-type-name
[Mixin]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#51-mixin-inheritance
[One Of]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#52-one-of-type

[Named Type]: #named-type-object
[Type Name]: #type-name-enum
[Type Definition]: #type-definition-object
[Type Section]: #type-section-object
[Symbol]: #symbol-object
[Element]: #element-object
[Elements]: #elements-arrayelement
[Markdown]: #markdown-string
[Literal]: #literal-string
[Value]: #value-object
[Property Member]: #property-member-value-member
[Value Member]: #value-member-object
[Mixin]: #mixin-type-definition
[One Of]: #one-of-elements
[Property Name]: #property-name-object
[Value Definition]: #value-definition-object
