# Decorator Metadata

Decorator metadata allows decorators to share information about decorated classes and their members.

It is useful when building systems that need to inspect class definitions at runtime, such as:

* Dependency injection containers
* Object-relational mapping (ORM) systems
* Serialization frameworks
* Validation libraries
* Routing systems
* Plugin architectures
* Application frameworks

TypeScript supports different metadata approaches depending on the decorator model.

# 🗁 What Is Decorator Metadata?

Metadata is additional information associated with a class or its members.

For example, an ORM might associate a database table name with a class and column names with its properties.

```typescript
class User {
  name = "Alice";
  email = "alice@example.com";
}
```

A metadata-based system could associate information such as:

```typescript
{
  table: "users",
  columns: ["name", "email"]
}
```

This information is separate from the actual property values.

# 🗁 Standard Decorator Metadata

TypeScript 5.2 introduced support for decorator metadata through the `context.metadata` property.

The metadata object is shared by decorators applied to the same class.

It can be accessed through:

```typescript
context.metadata
```

The metadata object is available when the runtime supports the `Symbol.metadata` mechanism.

A compatible runtime may require a `Symbol.metadata` polyfill.

# 🗁 Enabling Symbol.metadata

Some JavaScript runtimes do not provide `Symbol.metadata` natively.

A polyfill can be added before decorated classes are evaluated.

```typescript
Symbol.metadata ??= Symbol("Symbol.metadata");
```

For projects using TypeScript, the appropriate library declarations must also be available. Depending on the TypeScript version and configuration, this may require including `ESNext.Decorators` in the `lib` compiler option.

Example configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "strict": true,
    "lib": ["ES2022", "ESNext.Decorators"]
  }
}
```

The runtime must also support the required decorator semantics.

# 🗁 Accessing Metadata from a Class Decorator

A class decorator can access the shared metadata object through its context.

```typescript
function Entity(
  value: Function,
  context: ClassDecoratorContext
) {
  context.metadata.table = "users";
}

@Entity
class User {
  name = "Alice";
}
```

The decorator associates a `table` property with the class metadata.

# 🗁 Reading Class Metadata

A decorated class exposes its metadata through the `Symbol.metadata` property.

```typescript
function Entity(
  value: Function,
  context: ClassDecoratorContext
) {
  context.metadata.table = "users";
}

@Entity
class User {
  name = "Alice";
}

const metadata = User[
  Symbol.metadata as keyof typeof User
];

console.log(metadata);
```

For clearer typing, a metadata helper can be used:

```typescript
type MetadataRecord = Record<PropertyKey, unknown>;

function getMetadata(
  constructor: Function
): MetadataRecord | undefined {
  return (
    constructor as Function & {
      [Symbol.metadata]?: MetadataRecord;
    }
  )[Symbol.metadata];
}
```

Usage:

```typescript
const metadata = getMetadata(User);

console.log(metadata?.table);
```

The metadata is associated with the class, not with individual instances.

# 🗁 Sharing Metadata Between Decorators

One of the most useful features of standard decorator metadata is that multiple decorators on the same class can access the same metadata object.

```typescript
function Entity(
  value: Function,
  context: ClassDecoratorContext
) {
  context.metadata.table = "users";
}

function Version(
  value: Function,
  context: ClassDecoratorContext
) {
  context.metadata.version = 1;
}

@Entity
@Version
class User {
  name = "Alice";
}
```

The resulting metadata contains both values:

```typescript
{
  table: "users",
  version: 1
}
```

This allows independent decorators to contribute information to a shared metadata structure.

# 🗁 Property Metadata

Field decorators can also add information to the class metadata object.

```typescript
function Column(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  const columns =
    (context.metadata.columns as string[] | undefined) ?? [];

  columns.push(String(context.name));

  context.metadata.columns = columns;
}

class User {
  @Column
  name = "Alice";

  @Column
  email = "alice@example.com";
}
```

The metadata object contains the registered property names.

However, decorators should avoid modifying shared metadata structures carelessly. Inherited metadata can be shared or derived through prototype relationships, so mutable collections need deliberate handling.

# 🗁 Metadata Inheritance

Metadata can be inherited through the metadata object's prototype chain.

Consider:

```typescript
function Entity(
  value: Function,
  context: ClassDecoratorContext
) {
  context.metadata.table = "users";
}

@Entity
class User {}

class Admin extends User {}
```

The metadata associated with a subclass can inherit from the metadata of its parent class.

When reading metadata, use the prototype chain intentionally:

```typescript
const userMetadata = User[Symbol.metadata];

const adminMetadata = Admin[Symbol.metadata];

console.log(
  Object.getPrototypeOf(adminMetadata) === userMetadata
);
```

This relationship can be useful when frameworks need to inherit class configuration.

If a subclass should have independent mutable metadata, create a new collection rather than modifying an inherited collection directly.

# 🗁 Metadata with Symbol Keys

Using symbols as metadata keys helps avoid naming conflicts between decorators.

```typescript
const TABLE_NAME = Symbol("tableName");
const COLUMNS = Symbol("columns");

function Entity(
  value: Function,
  context: ClassDecoratorContext
) {
  context.metadata[TABLE_NAME] = "users";
}

function Column(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  const columns =
    (context.metadata[COLUMNS] as string[] | undefined) ?? [];

  columns.push(String(context.name));

  context.metadata[COLUMNS] = columns;
}
```

Symbol keys are useful when multiple libraries contribute metadata to the same class.

# 🗁 Building a Metadata-Based ORM Example

The following example demonstrates a simple entity and column registration system.

```typescript
Symbol.metadata ??= Symbol("Symbol.metadata");

const TABLE_NAME = Symbol("tableName");
const COLUMNS = Symbol("columns");

function Entity(tableName: string) {
  return function (
    value: Function,
    context: ClassDecoratorContext
  ) {
    context.metadata[TABLE_NAME] = tableName;
  };
}

function Column(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  const columns =
    (context.metadata[COLUMNS] as string[] | undefined) ?? [];

  context.metadata[COLUMNS] = [
    ...columns,
    String(context.name)
  ];
}

@Entity("users")
class User {
  @Column
  name = "Alice";

  @Column
  email = "alice@example.com";
}

const metadata = User[Symbol.metadata];

console.log(metadata?.[TABLE_NAME]);
console.log(metadata?.[COLUMNS]);
```

Output:

```text
users
["name", "email"]
```

This pattern can be extended to support column types, constraints, indexes, and relationships.

# 🗁 Legacy Decorator Metadata

Legacy decorators commonly use the `reflect-metadata` library to store and retrieve metadata.

This approach is frequently found in older TypeScript applications and frameworks.

Install the package:

```bash
npm install reflect-metadata
```

Import it before using its metadata APIs:

```typescript
import "reflect-metadata";
```

# 🗁 Using Reflect.defineMetadata()

The `Reflect.defineMetadata()` method associates metadata with a target object.

```typescript
import "reflect-metadata";

class User {}

Reflect.defineMetadata(
  "entity:table",
  "users",
  User
);
```

Arguments:

| Argument | Description |
| --- | --- |
| Metadata key | Identifies the metadata entry. |
| Metadata value | Stores the associated information. |
| Target | Object associated with the metadata. |

# 🗁 Using Reflect.getMetadata()

The `Reflect.getMetadata()` method retrieves metadata from a target.

```typescript
import "reflect-metadata";

class User {}

Reflect.defineMetadata(
  "entity:table",
  "users",
  User
);

const tableName = Reflect.getMetadata(
  "entity:table",
  User
);

console.log(tableName);
```

Output:

```text
users
```

The method searches the target's prototype chain.

For metadata defined directly on a target without inherited lookup, use `Reflect.getOwnMetadata()`.

# 🗁 Using Reflect.metadata() in Legacy Decorators

The `Reflect.metadata()` helper creates a legacy decorator that associates metadata with a class or class member.

```typescript
import "reflect-metadata";

function Entity(
  target: Function
) {
  console.log(
    Reflect.getMetadata("entity:table", target)
  );
}

@Entity
@Reflect.metadata("entity:table", "users")
class User {}
```

Output:

```text
users
```

Legacy decorator application order is important: the metadata decorator executes before the class decorator in this example, allowing the class decorator to read the metadata.

# 🗁 Emitting Design-Type Metadata

Legacy TypeScript decorators can work with compiler-generated design metadata.

Enable the relevant compiler options:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

Import the metadata library:

```typescript
import "reflect-metadata";
```

Example:

```typescript
import "reflect-metadata";

function Inspect(
  target: object,
  propertyKey: string
) {
  const propertyType = Reflect.getMetadata(
    "design:type",
    target,
    propertyKey
  );

  console.log(propertyKey, propertyType?.name);
}

class User {
  @Inspect
  name = "Alice";

  @Inspect
  age = 25;
}
```

Typical output:

```text
name String
age Number
```

TypeScript can emit metadata such as:

* `design:type`
* `design:paramtypes`
* `design:returntype`

These are runtime constructor references, not complete representations of TypeScript's static type system.

For example, interfaces, generic type arguments, and many complex types cannot be fully represented by this metadata.

# 🗁 Standard vs Legacy Metadata

| Feature | Standard decorators | Legacy decorators |
| --- | --- | --- |
| Metadata access | `context.metadata` | `Reflect.defineMetadata()` and related APIs |
| Metadata key | Object property or symbol | Arbitrary metadata key |
| Metadata storage | Shared class metadata object | Metadata associated with a target and optional property key |
| Inheritance | Metadata prototype chain | Metadata lookup can traverse target prototypes |
| Automatic design types | Not provided automatically | Supported through `emitDecoratorMetadata` |
| Additional package | Runtime may need `Symbol.metadata` support or a polyfill | Commonly uses `reflect-metadata` |
| Decorator model | Standard decorator proposal | Experimental legacy decorators |

These approaches are not interchangeable. Choose the metadata system that matches the decorator model and framework used by the application.

# 🗁 Best Practices

* Use standard decorator metadata for new standard-decorator implementations.
* Use `reflect-metadata` when working with frameworks built around legacy decorators.
* Prefer symbol keys to avoid metadata naming collisions.
* Define clear metadata types instead of relying on unstructured values.
* Handle inherited metadata deliberately.
* Avoid mutating inherited arrays or objects unintentionally.
* Do not assume TypeScript metadata contains complete type information.
* Keep metadata registration separate from metadata consumption.
* Test metadata behavior for subclasses and decorated members.
* Avoid mixing standard and legacy decorator APIs.

# 🗁 Summary

Decorator metadata allows decorators and frameworks to share runtime information about classes and their members.

Key concepts:

* Standard decorators expose metadata through `context.metadata`.
* `Symbol.metadata` provides access to class-associated metadata.
* Multiple decorators on a class can contribute to the same metadata object.
* Symbol keys help prevent naming conflicts.
* Metadata can participate in inheritance through prototype relationships.
* Legacy decorators commonly use the `reflect-metadata` package.
* `Reflect.defineMetadata()` stores metadata.
* `Reflect.getMetadata()` retrieves metadata, including inherited metadata.
* `emitDecoratorMetadata` generates limited design-type metadata for legacy decorators.
* Standard and legacy metadata systems use different APIs.
