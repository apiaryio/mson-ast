# MSON AST Source Map
This document defines serialization formats for [MSON][] AST Source Map.

## Version
Refer to the [MSON AST Definition][] – the version of the Source Map conforms to the version of MSON AST.

## Quick Links
+ [Source Map Definition](#source-map-definition)
+ [Media Types](#media-types)
+ [Example: JSON serialization](#example-json-serialization)

## Source Map Definition
Following is the description of MSON Source map media types using the [MSON][] syntax.

### Source Map (array)
This contains the information about the characters positions in the source.

#### Items
- (array, fixed)
- *1219* (number) - Zero-based index of the character position of the beginning of the source
- *30* (number) - Length of the source

### Named Type Source Map (object)
Source map of the [Named Type][]

#### Properties
- `name` ([Source Map][]) - Source map of name of the type
- `typeDefinition` ([Source Map][]) - Source map of ancestor type definition
- `sections` (array[[Type Section Source Map][]]) - Ordered array of type section source maps

### Type Section Source Map (enum)
Source map of the [Type Section][]

#### Members
- ([Source Map][]) - Source map of content for `blockDescription` or a literal value of `sample` and `default` classes
- (array[[Element Source Map][]]) - Ordered array of elements (`memberType`, `sample` or `default` classes only)

### Element Source Map (enum)
Source map of the [Element][]

#### Members
- ([Property Member Source Map][]) - Source map for `property` class
- ([Value Member Source Map][]) - Source map for `value` class
- ([Mixin Source Map][]) - Source map for `mixin` class
- ([One Of Source Map][]) - Source map for `oneOf` class
- ([Elements Source Map][]) - Source map for `group` class

### Elements Source Map (array[[Element Source Map][]])
Source map of the [Elements][]

### Property Member Source Map ([Value Member Source Map][])
Source map of the [Property Member][]

#### Properties
- `name` ([Source Map][]) - Source map of the property name

### Value Member Source Map (object)
Source map of the [Value Member][]

#### Properties
- `description` ([Source Map][]) - Source map of inline description
- `valueDefinition` ([Source Map][]) - Source map of member's value definition
- `sections` (array[[Type Section Source Map][]]) - List of member's type section source maps

### Mixin Source Map ([Source Map][])
Source map of the [Mixin][]

### One Of Source Map ([Elements Source Map][])
Source map of the [One Of][]

## Media Types
The Base type media type is `application/vnd.mson.sourcemap`.

### Serialization formats
Two supported, feature-equal, serialization formats are JSON and YAML:

+ `application/vnd.mson.sourcemap+json`
+ `application/vnd.mson.sourcemap+yaml`

### Example: JSON Serialization

`application/vnd.mson.sourcemap+json; version=2.0`

```json
{
  "name": [
    [0, 16]
  ],
  "typeDefinition": [
    [0, 16]
  ],
  "sections": [
    [
      {
        "name": [
          [18, 17]
        ],
        "description": [],
        "valueDefinition": [
          [18, 17]
        ],
        "sections": []
      },
      {
        "name": [
          [37, 19]
        ],
        "description": [],
        "valueDefinition": [
          [37, 19]
        ],
        "sections": []
      },
      {
        "name": [
          [58, 22]
        ],
        "description": [],
        "valueDefinition": [
          [58, 22]
        ],
        "sections": []
      },
      {
        "name": [
          [82, 18]
        ],
        "description": [],
        "valueDefinition": [
          [82, 18]
        ],
        "sections": []
      },
      {
        "name": [
          [102, 15]
        ],
        "description": [],
        "valueDefinition": [
          [102, 15]
        ],
        "sections": [
          [
            {
              "description": [],
              "valueDefinition": [
                [123, 2]
              ],
              "sections": []
            },
            {
              "description": [],
              "valueDefinition": [
                [131, 2]
              ],
              "sections": []
            },
            {
              "description": [],
              "valueDefinition": [
                [137, 4]
              ],
              "sections": []
            }
          ]
        ]
      }
    ]
  ]
}
```

> **NOTE:** This example uses the same MSON AST as used in the [MSON AST Definition][] example.

[MSON]: https://github.com/apiaryio/mson
[MSON AST Definition]: README.md

[Named Type]: README.md#named-type-object
[Type Section]: README.md#type-section-object
[Element]: README.md#element-object
[Elements]: README.md#elements-arrayelement
[Property Member]: README.md#property-member-value-member
[Value Member]: README.md#value-member-object
[Value Member]: README.md#value-member-object
[Mixin]: README.md#mixin-type-definition
[One Of]: README.md#one-of-elements

[Source Map]: #source-map-array
[Named Type Source Map]: #named-type-source-map-object
[Type Section Source Map]: #type-section-source-map-enum
[Element Source Map]: #element-source-map-enum
[Property Member Source Map]: #property-member-source-map-value-member-source-map
[Value Member Source Map]: #value-member-source-map
[Mixin Source Map]: #mixin-source-map-source-map
[One Of Source Map]: #one-of-source-map-elements-source-map
[Elements Source Map]: #elements-source-map-array-element-source-map
