# Class Decorators

Class decorators are functions applied to class declarations to observe, modify, or replace class behavior.

They are useful for implementing reusable functionality such as:

* Service registration
* Dependency injection
* Class validation
* Logging
* Framework configuration
* Runtime class extension
* Metadata association

TypeScript supports both standard class decorators and legacy experimental class decorators. Their signatures and behavior are different.

# 🗁 Standard Class Decorator

A standard class decorator receives two arguments:

* `value`: The constructor of the decorated class.
* `context`: A `ClassDecoratorContext` object containing information about the class.

Basic syntax:

```typescript
function Logger(
  value: Function,
  context: ClassDecoratorContext
) {
  console.log(`Decorating: ${String(context.name)}`);
}

@Logger
class UserService {
  getUsers() {
    return ["Alice", "Bob"];
  }
}
```

Output:

```text
Decorating: UserService
```

The decorator executes when the class declaration is evaluated, not whenever an instance is created.

# 🗁 Class Decorator Type

The standard decorator context can be explicitly typed using `ClassDecoratorContext`.

```typescript
function Logger(
  value: Function,
  context: ClassDecoratorContext
): void {
  console.log(context.kind);
  console.log(context.name);
}
```

Important context properties:

| Property | Description |
| --- | --- |
| `kind` | Always `"class"` for a class decorator. |
| `name` | Name of the decorated class. |
| `metadata` | Class metadata object, when supported and available. |
| `addInitializer()` | Registers an initializer associated with the class. |

# 🗁 Class Decorator Factory

A class decorator factory allows configuration to be passed to a decorator.

```typescript
function Entity(tableName: string) {
  return function (
    value: Function,
    context: ClassDecoratorContext
  ) {
    console.log(
      `${String(context.name)} maps to ${tableName}`
    );
  };
}

@Entity("users")
class User {}

@Entity("products")
class Product {}
```

Output:

```text
User maps to users
Product maps to products
```

The outer function receives configuration, while the returned function performs the decoration.

# 🗁 Returning a Replacement Class

A standard class decorator can return a new class that replaces the original class.

```typescript
function WithTimestamp<
  T extends new (...args: any[]) => object
>(
  Base: T,
  context: ClassDecoratorContext
) {
  return class extends Base {
    createdAt = new Date();
  };
}

@WithTimestamp
class User {
  constructor(public name: string) {}
}

const user = new User("Alice");

console.log(user.name);
console.log(user.createdAt);
```

The replacement class extends the original class and adds a property.

The replacement must remain compatible with the original class type. This is particularly important when adding new public members: the decorator's return type may need to be expressed through a more precise generic type or a class expression with an appropriate inferred type.

# 🗁 Adding Methods Through a Replacement Class

A decorator can add methods by returning a subclass.

```typescript
function WithLogging<
  T extends new (...args: any[]) => object
>(Base: T) {
  return class extends Base {
    log(message: string) {
      console.log(`[LOG]: ${message}`);
    }
  };
}

@WithLogging
class UserService {}

const service = new UserService();

// The runtime instance has the log method.
(service as UserService & {
  log(message: string): void;
}).log("Service started");
```

This example adds a method at runtime.

The type assertion makes the added method visible to TypeScript at the call site. For reusable libraries, a more explicit generic return type or factory-based composition may provide a stronger type contract.

# 🗁 Class Decorator with Initializer

The `addInitializer()` method registers a function that runs during class initialization.

```typescript
function Register(
  value: Function,
  context: ClassDecoratorContext
) {
  context.addInitializer(function () {
    console.log(
      `Class initialized: ${String(context.name)}`
    );
  });
}

@Register
class UserService {}
```

Class decorator initializers execute after the class has been defined and decorated.

They are useful for class-level setup and registration.

# 🗁 Class Registration Example

A common framework pattern is registering classes in a registry.

```typescript
const services = new Map<string, Function>();

function Injectable(name: string) {
  return function (
    value: Function,
    context: ClassDecoratorContext
  ) {
    services.set(name, value);
  };
}

@Injectable("userService")
class UserService {
  getUsers() {
    return ["Alice", "Bob"];
  }
}

@Injectable("productService")
class ProductService {
  getProducts() {
    return ["Laptop", "Phone"];
  }
}

console.log(services.get("userService"));
console.log(services.get("productService"));
```

The decorator registers each class when its declaration is evaluated.

A dependency injection container could use this registry to locate and construct services.

# 🗁 Legacy Class Decorators

Legacy class decorators use a different function signature.

They are enabled with:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

A legacy class decorator receives the constructor as its only argument.

```typescript
function Logger(
  constructor: Function
) {
  console.log(
    `Decorating class: ${constructor.name}`
  );
}

@Logger
class UserService {}
```

Unlike standard decorators, legacy decorators do not receive a context object.

# 🗁 Replacing a Class with a Legacy Decorator

A legacy class decorator can return a replacement constructor.

```typescript
function WithTimestamp<
  T extends new (...args: any[]) => object
>(
  Base: T
) {
  return class extends Base {
    createdAt = new Date();
  };
}

@WithTimestamp
class User {
  constructor(public name: string) {}
}

const user = new User("Alice");

console.log(user.name);
console.log(user.createdAt);
```

The returned constructor replaces the original class at runtime.

Legacy decorator return types must also be compatible with the decorated class.

# 🗁 Standard vs Legacy Class Decorators

| Feature | Standard | Legacy |
| --- | --- | --- |
| Configuration | No `experimentalDecorators` required | Requires `experimentalDecorators: true` |
| Arguments | Constructor and context | Constructor |
| Context object | Yes | No |
| Initializers | `addInitializer()` | Different legacy mechanisms |
| Replacement class | Supported | Supported |
| Metadata | Standard metadata integration | Commonly used with `reflect-metadata` |
| Parameter decorators | Not supported | Supported |

Do not use a standard decorator signature with a legacy decorator configuration or vice versa.

# 🗁 Practical Use Cases

Class decorators are particularly useful when behavior applies to an entire class.

**Service registration**

```typescript
@Service("user")
class UserService {}
```

**Entity mapping**

```typescript
@Entity("users")
class User {}
```

**Class-level logging**

```typescript
@Logger
class PaymentService {}
```

**Authorization configuration**

```typescript
@RequireRole("admin")
class AdminController {}
```

These examples illustrate common API designs. Each decorator must have an implementation that enforces or applies the declared behavior.

# 🗁 Best Practices

* Use class decorators for class-level behavior.
* Prefer decorator factories when configuration is required.
* Keep registration and initialization logic predictable.
* Preserve constructor compatibility when returning replacement classes.
* Make runtime-added members visible in the type system when consumers need to access them.
* Avoid modifying global state unnecessarily.
* Keep standard and legacy decorator implementations separate.
* Test replacement constructors and initializer behavior.
* Prefer composition when a decorator would make class behavior difficult to understand.

# 🗁 Summary

Class decorators operate on class declarations and can observe, register, initialize, or replace classes.

Key concepts:

* Standard class decorators receive a constructor and context object.
* Legacy class decorators receive the constructor.
* Decorator factories support configurable behavior.
* Replacement classes can extend existing classes.
* `addInitializer()` supports class-level initialization.
* Class decorators are commonly used for dependency injection, registration, logging, and framework configuration.
