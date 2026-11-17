# Property Decorators

Property decorators are used to add behavior or metadata to properties declared inside a class.

They are commonly associated with:

* Property validation
* Default value management
* Serialization and deserialization
* ORM entity mapping
* Configuration systems
* Dependency injection
* Property metadata

TypeScript supports property-related decorators through both standard decorators and legacy experimental decorators. However, their APIs and capabilities are different.

# 🗁 Standard Property Decorators

In the standard decorator model, a property decorator receives:

* `value`: The initial value or `undefined`, depending on the property declaration.
* `context`: A `ClassFieldDecoratorContext` object describing the property.

Example:

```typescript
function Inspect(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  console.log(`Decorating property: ${String(context.name)}`);
}

class User {
  @Inspect
  name = "Alice";
}
```

Output:

```text
Decorating property: name
```

Unlike legacy property decorators, standard field decorators can return an initializer function to transform the initial property value.

# 🗁 ClassFieldDecoratorContext

The standard property decorator context provides information about the decorated field.

Important properties include:

| Property | Description |
| --- | --- |
| `kind` | Identifies the element as a field. |
| `name` | Name or key of the property. |
| `static` | Indicates whether the property is static. |
| `private` | Indicates whether the property is private. |
| `access` | Provides access operations for the property. |
| `metadata` | Associated metadata object, when available. |
| `addInitializer()` | Registers an initializer for the decorated field's class instance or class. |

Example:

```typescript
function Inspect(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  console.log({
    kind: context.kind,
    name: context.name,
    isStatic: context.static,
    isPrivate: context.private
  });
}

class User {
  @Inspect
  name = "Alice";

  @Inspect
  age = 25;
}
```

# 🗁 Transforming Initial Property Values

A standard field decorator can return an initializer function.

The initializer receives the original initial value and returns the value that should be assigned to the property.

```typescript
function Uppercase(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  return function (initialValue: string) {
    return initialValue.toUpperCase();
  };
}

class User {
  @Uppercase
  name = "alice";
}

const user = new User();

console.log(user.name);
```

Output:

```text
ALICE
```

The transformation happens when an instance is initialized.

# 🗁 Generic Property Decorator

Generic types allow a property decorator to work with different property value types.

```typescript
function DefaultValue<Value>(
  defaultValue: Value
) {
  return function (
    initialValue: undefined | Value,
    context: ClassFieldDecoratorContext<unknown, Value>
  ) {
    return function (this: unknown, value: undefined | Value): Value {
      return value ?? defaultValue;
    };
  };
}

class User {
  @DefaultValue("Guest")
  name = undefined as string | undefined;

  @DefaultValue(18)
  age = undefined as number | undefined;
}

const user = new User();

console.log(user.name);
console.log(user.age);
```

Output:

```text
Guest
18
```

The decorator factory provides a configurable default value.

The initializer uses the supplied default only when the initial value is `null` or `undefined`.

# 🗁 Property Decorator with Validation

A property decorator can validate an initial value before it is assigned.

```typescript
function PositiveNumber(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  return function (initialValue: number) {
    if (initialValue <= 0) {
      throw new Error(
        `${String(context.name)} must be positive`
      );
    }

    return initialValue;
  };
}

class Product {
  @PositiveNumber
  price = 100;

  @PositiveNumber
  quantity = 5;
}

const product = new Product();

console.log(product.price);
console.log(product.quantity);
```

This validates the initial value during instance initialization.

It does not automatically validate future assignments to the property. Runtime validation of subsequent assignments requires a different approach, such as an auto-accessor decorator or an explicitly defined getter and setter.

# 🗁 Property Decorator with Logging

A decorator can log property initialization.

```typescript
function LogProperty(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  return function (initialValue: unknown) {
    console.log(
      `${String(context.name)} initialized:`,
      initialValue
    );

    return initialValue;
  };
}

class User {
  @LogProperty
  name = "Alice";

  @LogProperty
  age = 25;
}

const user = new User();
```

Output:

```text
name initialized: Alice
age initialized: 25
```

The initializer runs when an instance is created, so the messages appear for each new instance.

# 🗁 Using addInitializer()

The `addInitializer()` method allows a field decorator to register additional initialization logic.

```typescript
function LogProperty(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  const propertyName = String(context.name);

  context.addInitializer(function () {
    console.log(
      `Instance created with property: ${propertyName}`
    );
  });
}

class User {
  @LogProperty
  name = "Alice";

  @LogProperty
  age = 25;
}

const firstUser = new User();
const secondUser = new User();
```

The initializer is registered by the decorator and executes during instance initialization.

# 🗁 Decorating Static Properties

Standard property decorators can also be applied to static fields.

```typescript
function Inspect(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  console.log({
    name: context.name,
    isStatic: context.static
  });
}

class Application {
  @Inspect
  static version = "1.0.0";
}
```

The `static` property of the context identifies the decorated field as a static class member.

# 🗁 Property Access and Context

The `access` object provides operations for interacting with the decorated property.

It exposes:

* `get(object)`: Reads the property value.
* `set(object, value)`: Updates the property value.
* `has(object)`: Checks whether the property exists on an object.

Example:

```typescript
function Inspect(
  value: undefined,
  context: ClassFieldDecoratorContext
) {
  context.addInitializer(function () {
    const currentValue = context.access.get(this);

    console.log(
      `${String(context.name)}:`,
      currentValue
    );
  });
}

class User {
  @Inspect
  name = "Alice";
}

new User();
```

This can be useful when decorators need to inspect decorated properties during initialization.

# 🗁 Legacy Property Decorators

Legacy property decorators use a different API.

They require:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

A legacy property decorator receives:

* `target`: The class prototype for instance properties or the constructor for static properties.
* `propertyKey`: The name or symbol of the decorated property.

Example:

```typescript
function LogProperty(
  target: object,
  propertyKey: string
) {
  console.log(`Decorating property: ${propertyKey}`);
}

class User {
  @LogProperty
  name = "Alice";
}
```

Legacy property decorators do not receive a property descriptor and cannot directly replace the property's initializer.

They are commonly used to register metadata about properties.

# 🗁 Legacy Property Metadata

A legacy property decorator can store information about a class property.

```typescript
const registeredProperties = new WeakMap<
  object,
  string[]
>();

function Column(
  target: object,
  propertyKey: string
) {
  const properties =
    registeredProperties.get(target) ?? [];

  properties.push(propertyKey);

  registeredProperties.set(target, properties);
}

class User {
  @Column
  name = "Alice";

  @Column
  email = "alice@example.com";
}

console.log(
  registeredProperties.get(User.prototype)
);
```

Output:

```text
["name", "email"]
```

This pattern can be used by ORM libraries to collect information about entity fields.

# 🗁 Standard vs Legacy Property Decorators

| Feature | Standard | Legacy |
| --- | --- | --- |
| Arguments | Initial value and context | Target and property key |
| Context object | Yes | No |
| Transform initial value | Supported through returned initializer | Not directly supported |
| Property descriptor | Not used for fields | Not provided |
| `addInitializer()` | Supported | Not available through the standard context API |
| Metadata | Standard metadata integration | Commonly used with `reflect-metadata` |
| Configuration | No `experimentalDecorators` required | Requires `experimentalDecorators: true` |

# 🗁 Important Limitations

* Standard field decorators run during field initialization.
* They do not automatically intercept future property assignments.
* Legacy property decorators cannot directly replace property initializers.
* A field decorator cannot return a replacement method or property descriptor.
* Standard field decorators and auto-accessor decorators use different context types.
* Decorators do not replace TypeScript's compile-time type checking.
* Runtime validation must be implemented explicitly.

# 🗁 Best Practices

* Use field decorators for initialization-related behavior.
* Use auto-accessors or getters and setters when future assignments must be intercepted.
* Keep property validation rules focused and predictable.
* Prefer decorator factories for configurable property behavior.
* Avoid unnecessary mutation of class prototypes.
* Use `WeakMap` when storing metadata associated with class prototypes or constructors.
* Keep standard and legacy implementations separate.
* Test behavior across multiple instances.
* Document whether a decorator validates initial values or every subsequent assignment.

# 🗁 Summary

Property decorators provide a mechanism for extending field initialization and associating additional behavior or metadata with class properties.

Key concepts:

* Standard property decorators receive an initial value and context object.
* `ClassFieldDecoratorContext` provides information about the decorated field.
* Returning an initializer function allows the initial value to be transformed.
* `addInitializer()` registers additional initialization logic.
* The `access` object provides property access operations.
* Legacy property decorators receive a target and property key.
* Legacy property decorators are commonly used for metadata registration.
* Initial-value validation does not automatically validate future assignments.
