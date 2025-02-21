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
* Records: Tricky for records that have primary constructors because deserialization does not invoke the primary constuctor. Rather it tries to change the immutable properties which causes an exception when they are used. Possible solutions:
  * Use a parameterless constructor and init properties - not ideal since we love primary constructors with auto properties.
  * Mark the primary constructor with `[JsonConstructor]`. This is the recommended approach.
  * Decorate parameter properties in primary constructors as in `public record PersonRecord([property: JsonPropertyName("Name")] string Name)`.
  * Use `var options = new JsonSerializerOptions { IncludeFields = true };` to allow the serializer to work with private fields. Not recommended.
* Classes: No problem for Poco classes.
