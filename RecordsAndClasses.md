Here are the key differences between records and classes in C#:
## Immutability:
*	Records: Designed to be immutable by default. You can opt in to create mutable properties if you really want to.
*	Classes: Mutable by default. Properties can be changed after the object is created.
## Value Equality:
*	Records: Use value-based equality. Two record instances are considered equal if their properties have the same values.
*	Classes: Use reference-based equality by default. Two class instances are considered equal only if they reference the same object.
## Use Case:
*	Records: Ideal for data-centric applications where the focus is on the data being stored rather than the behavior.
* Classes: Suitable for scenarios where the object’s behavior and state management are important.
## Inheritance:
*	Records: Support inheritance but are typically used for simple data structures.
*	Classes: More commonly used for complex inheritance hierarchies and polymorphism.
##	With-Expressions:
*	Records: Support with expressions to create a copy of an object with some properties modified.
*	Classes: Do not support with expressions natively.
## Serialization with System.Text.Json
* Identical for both records and classes
## Deserialization with System.Text.Json
* Records: Deserialized using primary constructor.
* Classes: Require a parameterless constructor (not explicitly defined) or a custom converter.
