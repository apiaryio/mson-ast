# MSON AST Serialization Media Types
This document defines serialization formats for [MSON][] abstract syntax tree.

## Version

- **Version**: 2.0
- **Created**: 2014-07-31
- **Updated**: 2014-10-08

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
- `base` ([Type Definition][]) - The ancestor type definition
- `sections` (array[[Type Section][]]) - Ordered list of type sections

### Type Name (object)
Base or named type's name.

#### Properties
- `name` (enum)
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
- `literal` (string) - Name of the symbol
- `variable`: `false` (boolean, default) - Boolean flag to denote [Variable Type Name][], `true` for variable type name, `false` otherwise

### Type Definition (object)
Definition of an instance value type.

#### Properties
- `type_specification` (object)
    - `name` ([Type Name]) - Name of the value type in an MSON instance
    - `nested_types` (array[[Type Name][]]) - Array of nested value types

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
- `type` (enum[string]) - Denotes the type of the section
    - `description` - Section is a markdown block description
    - `member` - Section contains member type(s)
    - `sample` - Section contains sample member type(s)
    - `default` - Section contains default member type(s)
    - `validation` - Reserved for future use

- `content` (enum) - Content of the section based on its type
    - ([Markdown][]) - Markdown formatted content of the section, applicable for `block_description` type only
    - (array[[Member Type][]]) - Member types, applicable for `member`, `sample` or `default` types only

### Member Type (object)
Member Type of a structure as described in the MSON Specification. In addition, this object may also represent [Mixin][] and / or [One Of][] types.

#### Properties
- `type` (enum[string]) - Type of the member object
    - `property` - Property Member  
    - `value` - Value Member
    - `mixin` - Mixin Type
    - `oneof` - One Of Type

- `content` (enum)
    - ([Property Member][]) - Member for `property` type
    - ([Value Member][]) - Member for `value` type
    - ([Mixin][]) - Member for `mixin` type
    - ([One Of][]) - Member for `oneof` type

### Property Member ([Value Member][])
Individual member of an `object` type structure.

#### Properties
- `name` ([Property Name][]) - Name of the object property

### Property Name (object)
Name of a property member.

#### Properties
- One Of
    - `literal` (string) - Literal name of the property
    - `variable` ([Value Definition][]) - Variable name of the property

### Value Member (object)
Individual member of an `array` or `enum` type structure.

#### Properties
- `description` ([Markdown][]) - Description of the member
- `value_definition` ([Value Definition][]) - The definition of the member's value
- `sections` (array[[Type Section][]]) - List of member's type sections

### Mixin (object)
Mixin type. In the case of an AST, the Mixin type is treated as a special case of a member type.

#### Properties
- `type_definition` ([Type Definition][]) - Type Name or full Type Definition of the type to be included

### One Of (object)
One Of type. In the case of AST the One Of type is treated as a special case of a member type. 

#### Properties
- `members` (array[[Member Type][]]) - List of mutually exclusive member types.

Note only Member Types of `property`, `mixin` and `oneof` are allowed in the members array.

### Value Definition (object)
Value definition of a type instance.

#### Properties
- `values` (array[[Value][]]) - List of values specified in the definition
- `type_definition` ([Type Definition][]) - Type of the value

### Value (object)
Sample or actual value of a type instance

#### Properties 
- `literal` (string) - The literal value
- `variable`: `false` (boolean, default) - `true` to denote variable value, `false` otherwise

### Markdown (string)
Markdown formatted plain text string.

---

## Example 

### MSON

```
- id: 1
- name: A green door
- price: 12.50 (number)
- tags: home, green

```

### application/vnd.mson.ast+json

```json

{
    "types": [
        {
            "name": null,
            "base": {
                "type_specification": {
                    "name": "object"
                }
            },
            "sections": [
                {
                    "type": "member",
                    "content": [
                        {
                            "type": "property",
                            "content": {
                                "name": {
                                    "literal": "id"
                                },
                                "value_definition": {
                                    "values": [
                                        {
                                            "literal": "1"
                                        }
                                    ]
                                }
                            }
                        },
                        {
                            "type": "property",
                            "content": {
                                "name": {
                                    "literal": "name"
                                },
                                "value_definition": {
                                    "values": [
                                        {
                                            "literal": "A green door"
                                        }
                                    ]
                                }
                            }
                        },
                        {
                            "type": "property",
                            "content": {
                                "name": {
                                    "literal": "price"
                                },
                                "value_definition": {
                                    "values": [
                                        {
                                            "literal": "12.50"
                                        }
                                    ],
                                    "type_definition": {
                                        "type_specification": {
                                            "name": "number"
                                        }
                                    }
                                }
                            }
                        }
                        {
                            "type": "property",
                            "content": {
                                "name": {
                                    "literal": "tags"
                                },
                                "value_definition": {
                                    "values": [
                                        {
                                            "literal": "home"
                                        },
                                        {
                                            "literal": "green"
                                        }                                        
                                    ],
                                    "type_definition": {
                                        "type_specification": {
                                            "name": "array"
                                        }
                                    }
                                }
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
[Type Name]: #type-name-object
[Type Definition]: #type-definition-object
[Type Section]: #type-section-object
[Symbol]: #symbol-object
[Member Type]: #member-type-object
[Markdown]: #markdown-string
[Value]: #value-object
[Property Member]: #property-member-object
[Value Member]: #value-member-object
[Mixin]: #mixin-object
[One Of]: #one-of-object
[Property Name]: #property-name-object
[Value Definition]: #value-definition-object
