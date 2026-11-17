# Decorator Basics

Decorators are a TypeScript feature that allows developers to modify or extend the behavior of classes and class members through reusable functions.

They provide a declarative way to attach additional behavior to classes, methods, accessors, properties, and parameters, depending on the decorator model being used.

Decorators are commonly used in frameworks and libraries for:

* Dependency injection
* Logging and instrumentation
* Validation
* Authorization
* Metadata management
* Registration and configuration
* Framework-specific behavior

# 🗁 What Is a Decorator?

A decorator is a function that is applied to a class or class member using the `@` syntax.

For example:

```typescript
function Logger(
  target: Function,
  context: ClassDecoratorContext
) {
  console.log("Class decorated:", context.name);
}

@Logger
class UserService {
  getUsers() {
    return ["Alice", "Bob"];
  }
}
```

When TypeScript evaluates the class declaration, it invokes the decorator and passes information about the decorated class.

The `@Logger` syntax is equivalent to applying the decorator function to the class according to the decorator model.

# 🗁 Decorator Syntax

The general syntax is:

```typescript
@decoratorFunction
class Example {}
```

A decorator can also be created by calling a decorator factory:

```typescript
@decoratorFactory()
class Example {}
```

The factory returns the actual decorator function.

# 🗁 TypeScript Decorator Models

TypeScript supports two decorator models.

**Standard decorators**

* Introduced in TypeScript 5.0.
* Follow the standardized JavaScript decorator proposal.
* Use context objects to describe decorated elements.
* Support classes, methods, accessors, properties, and auto-accessors.
* Do not support parameter decorators.
* Do not use the legacy `experimentalDecorators` configuration.

**Legacy experimental decorators**

* Available through TypeScript's earlier experimental implementation.
* Enabled using `"experimentalDecorators": true`.
* Use target, property key, and descriptor arguments depending on the decorator type.
* Support parameter decorators.
* Commonly encountered in older TypeScript frameworks and codebases.

These models have different APIs and should not be mixed.

# 🗁 Standard Class Decorator

A standard class decorator receives the class constructor and a class context object.

```typescript
function Logger(
  value: Function,
  context: ClassDecoratorContext
) {
  console.log(`Decorating class: ${String(context.name)}`);
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
Decorating class: UserService
```

The `value` parameter represents the decorated class.

The `context` parameter provides information about the decorated element.

# 🗁 Decorator Factory

A decorator factory is a function that accepts configuration and returns a decorator.

This allows decorators to be customized when applied.

```typescript
function Logger(prefix: string) {
  return function (
    value: Function,
    context: ClassDecoratorContext
  ) {
    console.log(`${prefix}: ${String(context.name)}`);
  };
}

@Logger("Service")
class UserService {}

@Logger("Controller")
class UserController {}
```

Output:

```text
Service: UserService
Controller: UserController
```

Decorator factories are useful when the same decorator behavior needs different configurations.

# 🗁 Class Decorator Context

The standard class decorator context provides information about the decorated class.

Important properties include:

| Property | Description |
| --- | --- |
| `kind` | Identifies the decorated element type. |
| `name` | Name of the decorated class. |
| `metadata` | Metadata object associated with the class, when available. |
| `addInitializer()` | Registers an initializer to run during class initialization. |

The context object is supplied by TypeScript's decorator runtime.

# 🗁 Returning a Replacement Class

A standard class decorator can return a replacement constructor.

```typescript
function WithTimestamp<T extends new (...args: any[]) => object>(
  value: T,
  context: ClassDecoratorContext
) {
  return class extends value {
    createdAt = new Date();
  };
}

@WithTimestamp
class UserService {}

const service = new UserService();

console.log(service.createdAt);
```

The decorator returns a class that extends the original class and adds a `createdAt` property.

A replacement class must remain compatible with the original class type.

# 🗁 Adding Initializers

The `addInitializer()` method allows a decorator to register initialization logic.

```typescript
function Register(
  value: Function,
  context: ClassDecoratorContext
) {
  context.addInitializer(function () {
    console.log(`${String(context.name)} initialized`);
  });
}

@Register
class UserService {}
```

Initializers are useful for performing setup operations associated with decorated elements.

The exact execution timing depends on the decorated element and decorator model.

# 🗁 Multiple Decorators

Multiple decorators can be applied to the same declaration.

```typescript
function First(
  value: Function,
  context: ClassDecoratorContext
) {
  console.log("First decorator");
}

function Second(
  value: Function,
  context: ClassDecoratorContext
) {
  console.log("Second decorator");
}

@First
@Second
class UserService {}
```

Decorator expressions are evaluated from top to bottom, while the resulting decorators are invoked in reverse order.

For the example above, the invocation order is:

```text
Second decorator
First decorator
```

Understanding this order is important when decorators depend on one another.

# 🗁 Using Decorators with TypeScript Configuration

Standard decorators do not require the legacy experimental decorator option.

A typical configuration is:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "strict": true
  }
}
```

For legacy decorators, enable:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

Use the configuration appropriate to the decorator model required by your project.

# 🗁 Standard Decorator Limitations

Standard decorators have some important limitations:

* Parameter decorators are not supported.
* They do not automatically emit the legacy `design:type`, `design:paramtypes`, and `design:returntype` metadata.
* Their function signatures differ from legacy decorators.
* A standard decorator cannot freely replace a decorated value with an incompatible type.
* Decorator behavior depends on the element type being decorated.

These differences matter when migrating legacy decorator-based applications.

# 🗁 Practical Example

The following example uses a decorator factory to register service classes.

```typescript
const registeredServices = new Map<string, Function>();

function Service(name: string) {
  return function (
    value: Function,
    context: ClassDecoratorContext
  ) {
    registeredServices.set(name, value);

    console.log(
      `Registered ${String(context.name)} as ${name}`
    );
  };
}

@Service("user")
class UserService {
  getUsers() {
    return ["Alice", "Bob"];
  }
}

@Service("product")
class ProductService {
  getProducts() {
    return ["Laptop", "Phone"];
  }
}

console.log(registeredServices);
```

This example demonstrates how decorators can perform registration during class definition.

A framework could use a similar pattern to discover services and construct a dependency injection container.

# 🗁 Best Practices

* Understand whether a project uses standard or legacy decorators.
* Prefer standard decorators for new code when the target framework supports them.
* Keep decorators focused on one responsibility.
* Use decorator factories when configurable behavior is needed.
* Avoid unexpected side effects during class definition.
* Preserve the original class or member type when returning replacements.
* Document decorators that modify runtime behavior.
* Test decorator behavior independently.
* Avoid using decorators when a simple function or composition pattern is clearer.

# 🗁 Summary

Decorators provide a reusable mechanism for extending class and member behavior.

Key concepts:

* Decorators use the `@` syntax.
* A decorator is a function applied to a declaration.
* Decorator factories provide configurable decorators.
* Standard decorators use context objects.
* Class decorators can return replacement constructors.
* `addInitializer()` registers initialization logic.
* Multiple decorators execute in reverse application order.
* Standard and legacy decorators use different APIs.
