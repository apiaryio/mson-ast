# MSON AST Serialization Media Types
This document defines serialization formats for [MSON][] abstract syntax tree.

## Version

- **Version**: 2.0
- **Created**: 2014-07-31
- **Updated**: 2014-10-07

## Media Types
Base type media type is `application/vnd.mson.ast`.

### Serialization formats
Two supported, feature-equal serialization formats are JSON and YAML:

- `application/vnd.mson.ast+json`
- `application/vnd.mson.ast+yaml`

## AST Serialization
Following is description of MSON AST serializations data structures using the [MSON][] syntax. 

> **NOTE:** Refer to the [MSON Specification][] for the explanation of terms used throughout this document. 

### Document (object)
Top-level MSON document or block. 

#### Properties
- types (array[Named Type]) - List of top-level [Named Types][] described in the document

### Named Type (object)
- name (Type Name) - Name of the type being defined
- base (Type Definition) - The ancestor type definition
- sections (array[Type Section]) - Ordered list of type sections

### Type Name (object)
- name (enum)
    - boolean (string)
    - string (string)
    - number (string)
    - array (string)
    - enum (string)
    - object (string)
    - (Symbol)

### Symbol (object)
- name (string) - Name of the symbol (identifier)
- variable (bool) - Boolean flag to denote [Variable Type Name][], true for variable type name, false otherwise

### Type Definition (object)
- type_specification (object)
    - name (Type Name) - Name of the value type in an MSON instance
    - nested_types (array[Type Name]) - Array of nested value types

- attributes (array) - List of attributes associated with the type
    - enum[string]
        - required
        - optional
        - default
        - sample
        - fixed

### Type Section
Section of a type description. The section can be any of the [Type Sections][] as described in the MSON Specification. 
#### Properties
- type (enum[string]) - Denotes the type of the section
    - block_description - Section is a markdown block description
    - member - Section contains member type(s)
    - sample - Section contains sample member type(s)
    - default - Section contains default member type(s)
    - validation - Reserved for future use

- content (enum) - Content of the section based on its type
    - (Markdown) - Markdown formatted content of the section (`block_description` only)
    - (array[Member Type]) - Array of the member types

### Member Type (object)
This object is a Member Type of a structure type as described in the MSON Specification. However in **addition** this object may also represent a [Mixin][] or [One Of][] types.

#### Properties
- type (enum[string]) - Type of the member object
    - property - Property Member  
    - value - Value Member
    - mixin - Mixin Type
    - oneof - One Of Type

- content (enum)
    - (Property Member)
    - (Value Member)
    - (Mixin)
    - (One Of)


[MSON]: https://github.com/apiaryio/mson
[MSON Specification]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md
[Type Sections]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#4-type-sections
[Named Types]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#22-named-types
[Variable Type Name]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#3521-variable-type-name
[Mixin]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#51-mixin-inheritance
[One Of]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#52-one-of-type
