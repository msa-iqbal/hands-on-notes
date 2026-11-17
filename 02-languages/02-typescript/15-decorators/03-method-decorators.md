# Method Decorators

Method decorators are functions applied to class methods to observe, modify, or replace their behavior.

They allow developers to add reusable functionality without changing the original method implementation.

Common use cases include:

* Logging method calls
* Measuring execution time
* Caching method results
* Authorization checks
* Validation
* Error handling
* Performance monitoring

TypeScript supports standard and legacy method decorators. Their signatures and behavior differ.

# 🗁 Standard Method Decorator

A standard method decorator receives two arguments:

* `value`: The original method function.
* `context`: A `ClassMethodDecoratorContext` object describing the decorated method.

Example:

```typescript
function Logger(
  originalMethod: Function,
  context: ClassMethodDecoratorContext
) {
  console.log(`Decorating method: ${String(context.name)}`);
}

class UserService {
  @Logger
  getUsers() {
    return ["Alice", "Bob"];
  }
}
```

Output:

```text
Decorating method: getUsers
```

The decorator executes when the class is defined. It does not execute every time the method is called.

# 🗁 Method Decorator Context

The standard method decorator context provides information about the decorated method.

Important properties include:

| Property | Description |
| --- | --- |
| `kind` | Identifies the element as a method. |
| `name` | Name or key of the decorated method. |
| `static` | Indicates whether the method is static. |
| `private` | Indicates whether the method is private. |
| `access` | Provides access operations for the decorated method. |
| `metadata` | Associated metadata object, when available. |
| `addInitializer()` | Registers an initializer for the decorated method's class instance or class, depending on the method. |

Example:

```typescript
function Inspect(
  originalMethod: Function,
  context: ClassMethodDecoratorContext
) {
  console.log({
    kind: context.kind,
    name: context.name,
    isStatic: context.static,
    isPrivate: context.private
  });
}

class UserService {
  @Inspect
  getUsers() {
    return [];
  }

  @Inspect
  static create() {
    return new UserService();
  }
}
```

# 🗁 Replacing a Method

A standard method decorator can return a replacement function.

This allows developers to execute additional logic before or after the original method.

```typescript
function Logger<This, Args extends unknown[], Return>(
  originalMethod: (this: This, ...args: Args) => Return,
  context: ClassMethodDecoratorContext<
    This,
    (this: This, ...args: Args) => Return
  >
) {
  const methodName = String(context.name);

  function replacementMethod(
    this: This,
    ...args: Args
  ): Return {
    console.log(`Calling ${methodName}`);

    const result = originalMethod.call(this, ...args);

    console.log(`Finished ${methodName}`);

    return result;
  }

  return replacementMethod;
}

class UserService {
  @Logger
  getUser(id: number) {
    return {
      id,
      name: "Alice"
    };
  }
}

const service = new UserService();

console.log(service.getUser(1));
```

Output:

```text
Calling getUser
Finished getUser
{ id: 1, name: 'Alice' }
```

The decorator preserves the original method's `this` context, arguments, and return type.

# 🗁 Method Decorator Factory

A decorator factory allows configuration to be passed to a method decorator.

```typescript
function Logger(prefix: string) {
  return function <This, Args extends unknown[], Return>(
    originalMethod: (this: This, ...args: Args) => Return,
    context: ClassMethodDecoratorContext<
      This,
      (this: This, ...args: Args) => Return
    >
  ) {
    const methodName = String(context.name);

    return function (
      this: This,
      ...args: Args
    ): Return {
      console.log(`${prefix}: ${methodName}`);

      return originalMethod.call(this, ...args);
    };
  };
}

class UserService {
  @Logger("SERVICE")
  getUsers() {
    return ["Alice", "Bob"];
  }
}

const service = new UserService();

service.getUsers();
```

Output:

```text
SERVICE: getUsers
```

Decorator factories are useful when different methods need different configurations.

# 🗁 Measuring Method Execution Time

A method decorator can measure how long a method takes to execute.

```typescript
function MeasureTime<This, Args extends unknown[], Return>(
  originalMethod: (this: This, ...args: Args) => Return,
  context: ClassMethodDecoratorContext<
    This,
    (this: This, ...args: Args) => Return
  >
) {
  const methodName = String(context.name);

  return function (
    this: This,
    ...args: Args
  ): Return {
    const start = performance.now();

    try {
      return originalMethod.call(this, ...args);
    } finally {
      const end = performance.now();

      console.log(
        `${methodName} took ${(end - start).toFixed(2)} ms`
      );
    }
  };
}

class DataService {
  @MeasureTime
  processData() {
    let total = 0;

    for (let i = 0; i < 1_000_000; i++) {
      total += i;
    }

    return total;
  }
}

new DataService().processData();
```

The `finally` block ensures that execution time is reported even when the original method throws an error.

For asynchronous methods, this synchronous implementation measures only the time required to return the Promise. Measuring the complete asynchronous operation requires awaiting its settlement.

# 🗁 Decorating Asynchronous Methods

An asynchronous method decorator can measure the complete execution time of a Promise-returning method.

```typescript
function MeasureAsync<This, Args extends unknown[], Return>(
  originalMethod: (
    this: This,
    ...args: Args
  ) => Promise<Return>,
  context: ClassMethodDecoratorContext<
    This,
    (this: This, ...args: Args) => Promise<Return>
  >
) {
  const methodName = String(context.name);

  return async function (
    this: This,
    ...args: Args
  ): Promise<Return> {
    const start = performance.now();

    try {
      return await originalMethod.call(this, ...args);
    } finally {
      const end = performance.now();

      console.log(
        `${methodName} took ${(end - start).toFixed(2)} ms`
      );
    }
  };
}

class UserService {
  @MeasureAsync
  async getUsers() {
    const response = await fetch("/api/users");

    return response.json();
  }
}
```

The `await` ensures that the decorator measures the complete asynchronous operation, including rejected Promises.

# 🗁 Using addInitializer()

The `addInitializer()` method allows a method decorator to register initialization logic.

One common use case is automatically binding a method to its instance.

```typescript
function Bound(
  originalMethod: Function,
  context: ClassMethodDecoratorContext
) {
  if (context.private || context.static) {
    throw new Error(
      "Bound can only decorate public instance methods"
    );
  }

  context.addInitializer(function () {
    const method = context.name as keyof typeof this;

    this[method] = this[method].bind(this);
  });
}

class UserService {
  name = "UserService";

  @Bound
  getName() {
    return this.name;
  }
}

const service = new UserService();

const getName = service.getName;

console.log(getName());
```

Output:

```text
UserService
```

Without binding, extracting a method from an object can cause it to lose its original `this` context.

The initializer runs for each instance, binding the method to that instance.

# 🗁 Method Decorator with Validation

A method decorator can validate arguments before invoking the original method.

```typescript
function ValidatePositiveNumber<This>(
  originalMethod: (this: This, value: number) => unknown,
  context: ClassMethodDecoratorContext<
    This,
    (this: This, value: number) => unknown
  >
) {
  return function (
    this: This,
    value: number
  ) {
    if (value <= 0) {
      throw new Error("Value must be positive");
    }

    return originalMethod.call(this, value);
  };
}

class ProductService {
  @ValidatePositiveNumber
  getProduct(id: number) {
    return {
      id,
      name: "Laptop"
    };
  }
}

const service = new ProductService();

console.log(service.getProduct(10));

// Throws an error:
// service.getProduct(-1);
```

This approach centralizes validation logic and keeps the original method focused on its main responsibility.

# 🗁 Multiple Method Decorators

Multiple decorators can be applied to a single method.

```typescript
function First<This, Args extends unknown[], Return>(
  method: (this: This, ...args: Args) => Return,
  context: ClassMethodDecoratorContext<
    This,
    (this: This, ...args: Args) => Return
  >
) {
  console.log("First decorator");
}

function Second<This, Args extends unknown[], Return>(
  method: (this: This, ...args: Args) => Return,
  context: ClassMethodDecoratorContext<
    This,
    (this: This, ...args: Args) => Return
  >
) {
  console.log("Second decorator");
}

class UserService {
  @First
  @Second
  getUsers() {
    return [];
  }
}
```

Decorator expressions are evaluated from top to bottom, while the resulting decorators are applied in reverse order.

The application order is:

* `Second`
* `First`

When decorators return replacement functions, each decorator receives the result of the decorator below it.

# 🗁 Legacy Method Decorators

Legacy method decorators use a different signature and require the `experimentalDecorators` compiler option.

```json
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

A legacy method decorator receives three arguments:

* `target`: The class prototype for instance methods or the constructor for static methods.
* `propertyKey`: The name of the decorated method.
* `descriptor`: The property descriptor of the method.

Example:

```typescript
function Logger(
  target: object,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  console.log(`Decorating method: ${propertyKey}`);
}

class UserService {
  @Logger
  getUsers() {
    return ["Alice", "Bob"];
  }
}
```

The descriptor provides access to the method's property descriptor, including its `value` function.

# 🗁 Replacing a Method with a Legacy Decorator

A legacy decorator can replace a method by modifying or returning its property descriptor.

```typescript
function Logger(
  target: object,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: unknown[]) {
    console.log(`Calling ${propertyKey}`);

    const result = originalMethod.apply(this, args);

    console.log(`Finished ${propertyKey}`);

    return result;
  };

  return descriptor;
}

class UserService {
  @Logger
  getUsers() {
    return ["Alice", "Bob"];
  }
}

const service = new UserService();

console.log(service.getUsers());
```

The decorator replaces the method implementation while preserving access to the original method.

# 🗁 Standard vs Legacy Method Decorators

| Feature | Standard | Legacy |
| --- | --- | --- |
| Arguments | Original method and context | Target, property key, descriptor |
| Context object | Yes | No |
| Replacement | Return a replacement function | Modify or return descriptor |
| Initializers | `addInitializer()` | No equivalent standard context API |
| Parameter decorators | Not supported | Supported |
| Configuration | No `experimentalDecorators` required | Requires `experimentalDecorators: true` |

# 🗁 Best Practices

* Keep method decorators focused on one responsibility.
* Preserve the original `this` context when wrapping methods.
* Preserve arguments and return types using generics.
* Use `try/finally` when cleanup or timing must happen even after errors.
* Handle asynchronous methods separately when measuring execution time.
* Avoid unexpected changes to method behavior.
* Use decorator factories for configurable behavior.
* Avoid expensive operations inside frequently executed wrappers.
* Test both successful and exceptional execution paths.
* Keep standard and legacy decorator implementations separate.

# 🗁 Summary

Method decorators provide a reusable way to extend or replace class method behavior.

Key concepts:

* Standard method decorators receive the original method and a context object.
* `ClassMethodDecoratorContext` describes the decorated method.
* A decorator can return a replacement method.
* Generic types help preserve `this`, arguments, and return types.
* Decorator factories support configurable behavior.
* `addInitializer()` can register instance or class initialization logic.
* Legacy method decorators use target, property key, and property descriptor arguments.
* Common use cases include logging, timing, validation, caching, and authorization.
