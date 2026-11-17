# Readonly Properties

Readonly properties are object properties that cannot be reassigned after their initial assignment.

TypeScript provides the `readonly` modifier to prevent accidental modification of properties.

Readonly properties are useful when you want to:

* Protect important object data from reassignment.
* Represent immutable identifiers.
* Prevent accidental changes to configuration values.
* Define read-only application models.
* Improve type safety and code maintainability.
* Express which properties should remain unchanged after initialization.

# 🗁 Basic Syntax

Use the `readonly` keyword before a property declaration.

```typescript
interface User {
  readonly id: number;
  name: string;
}
```

In this example:

* `id` is readonly.
* `name` is mutable.

Create an object:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
};
```

You can modify the `name` property:

```typescript
user.name = "Bob";
```

But you cannot reassign the `id` property:

```typescript
// Error: Cannot assign to 'id' because it is a read-only property.
user.id = 2;
```

# 🗁 Readonly Properties in Type Aliases

Readonly properties also work with type aliases.

```typescript
type Product = {
  readonly id: number;
  name: string;
  price: number;
};
```

Example:

```typescript
const product: Product = {
  id: 101,
  name: "Laptop",
  price: 1200,
};

product.name = "Desktop";
product.price = 1500;

// Error:
product.id = 102;
```

# 🗁 Readonly Properties in Object Types

You can use readonly properties directly in an object type annotation.

```typescript
const user: {
  readonly id: number;
  name: string;
} = {
  id: 1,
  name: "Alice",
};
```

The `id` property cannot be reassigned.

```typescript
user.name = "Bob";

// Error:
user.id = 2;
```

# 🗁 Readonly with Optional Properties

A property can be both readonly and optional.

```typescript
interface User {
  readonly id: number;
  readonly email?: string;
  name: string;
}
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};
```

The optional `email` property may be omitted during object creation, but it cannot be reassigned afterward.

```typescript
// Error:
user.email = "new@example.com";
```

# 🗁 Readonly with Arrays

TypeScript provides the `readonly` modifier for array types.

```typescript
const numbers: readonly number[] = [10, 20, 30];
```

You can read array elements:

```typescript
console.log(numbers[0]);
```

But you cannot modify the array through this readonly reference.

```typescript
// Error:
numbers.push(40);

// Error:
numbers[0] = 100;

// Error:
numbers.pop();
```

The `readonly number[]` syntax is shorthand for `ReadonlyArray<number>`.

```typescript
const names: ReadonlyArray<string> = ["Alice", "Bob"];
```

# 🗁 Readonly Tuples

Readonly tuples prevent modification of tuple elements.

```typescript
const coordinates: readonly [number, number] = [10, 20];
```

You can access individual elements:

```typescript
console.log(coordinates[0]);
console.log(coordinates[1]);
```

But you cannot reassign elements or use mutating array methods.

```typescript
// Error:
coordinates[0] = 50;

// Error:
coordinates.push(30);
```

# 🗁 ReadonlyArray Type

`ReadonlyArray<T>` is a generic type that represents an array whose elements cannot be modified through that reference.

```typescript
function displayNames(names: ReadonlyArray<string>): void {
  names.forEach((name) => {
    console.log(name);
  });
}
```

Example:

```typescript
const users = ["Alice", "Bob", "Charlie"];

displayNames(users);
```

The function can read and iterate over the array but cannot modify it.

```typescript
function processNames(names: ReadonlyArray<string>): void {
  // Error:
  names.push("David");
}
```

# 🗁 Readonly Utility Type

TypeScript provides the built-in `Readonly<T>` utility type.

It converts all properties of a type into readonly properties.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type ReadonlyUser = Readonly<User>;
```

The resulting type is equivalent to:

```typescript
type ReadonlyUser = {
  readonly id: number;
  readonly name: string;
  readonly email: string;
};
```

Example:

```typescript
const user: ReadonlyUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};

// Error:
user.name = "Bob";
```

# 🗁 Readonly with Interface Extension

Readonly properties remain readonly when inherited through interface extension.

```typescript
interface User {
  readonly id: number;
  name: string;
}

interface Admin extends User {
  permissions: string[];
}
```

Example:

```typescript
const admin: Admin = {
  id: 1,
  name: "Alice",
  permissions: ["manage-users"],
};

admin.name = "Bob";

// Error:
admin.id = 2;
```

An extending interface cannot remove the readonly restriction from an inherited property.

# 🗁 Readonly with Classes

The `readonly` modifier is also supported in classes.

```typescript
class User {
  readonly id: number;
  name: string;

  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }
}
```

Create an instance:

```typescript
const user = new User(1, "Alice");

user.name = "Bob";

// Error:
user.id = 2;
```

A class readonly property can be assigned in its declaration or constructor, but it cannot be reassigned afterward.

# 🗁 Readonly Parameter Properties

TypeScript allows readonly parameter properties in constructors.

```typescript
class Product {
  constructor(
    public readonly id: number,
    public name: string,
  ) {}
}
```

Example:

```typescript
const product = new Product(101, "Laptop");

product.name = "Desktop";

// Error:
product.id = 102;
```

This is useful for defining immutable identifiers without writing separate property declarations and assignments.

# 🗁 Readonly vs const

Both `const` and `readonly` prevent reassignment, but they apply to different things.

**const**

`const` prevents reassignment of a variable binding.

```typescript
const user = {
  name: "Alice",
};

// Valid:
user.name = "Bob";

// Error:
user = {
  name: "Charlie",
};
```

The object can still be modified because `const` protects the variable binding, not the object's properties.

**readonly**

`readonly` prevents reassignment of a property through a readonly reference.

```typescript
interface User {
  readonly id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: "Alice",
};

// Valid:
user.name = "Bob";

// Error:
user.id = 2;
```

| Feature | `const` | `readonly` |
| --- | --- | --- |
| Applied to | Variables | Properties and supported types |
| Prevents variable reassignment | Yes | No |
| Prevents property reassignment | No | Yes |
| Prevents object mutation automatically | No | No |
| Main purpose | Protect variable binding | Protect property assignment |

# 🗁 Readonly vs Object.freeze()

TypeScript's `readonly` modifier provides a compile-time restriction.

JavaScript's `Object.freeze()` provides a runtime restriction on an object's own properties.

Readonly example:

```typescript
interface User {
  readonly id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: "Alice",
};

// TypeScript error:
user.id = 2;
```

JavaScript runtime freezing:

```typescript
const user = Object.freeze({
  id: 1,
  name: "Alice",
});
```

`Object.freeze()` prevents adding, removing, or changing the object's own properties at runtime.

However, freezing is shallow. Nested objects are not automatically frozen.

```typescript
const user = Object.freeze({
  name: "Alice",
  address: {
    city: "Dhaka",
  },
});

// The nested object is not frozen:
user.address.city = "Chattogram";
```

| Feature | `readonly` | `Object.freeze()` |
| --- | --- | --- |
| TypeScript compile-time protection | Yes | Yes, through its return type |
| JavaScript runtime protection | No | Yes |
| Prevents property reassignment | Through typed references | Yes, for own properties |
| Deep protection | No | No, shallow by default |

# 🗁 Readonly Is Not Deeply Immutable

Readonly properties do not automatically make nested objects immutable.

Consider:

```typescript
interface User {
  readonly id: number;
  readonly address: {
    city: string;
  };
}
```

Create an object:

```typescript
const user: User = {
  id: 1,
  address: {
    city: "Dhaka",
  },
};
```

You cannot replace the `address` property:

```typescript
// Error:
user.address = {
  city: "Chattogram",
};
```

But you can modify a nested property:

```typescript
// Allowed:
user.address.city = "Chattogram";
```

The `readonly` modifier protects the property reference, not every nested property.

To make nested structures readonly, apply readonly types recursively or use a suitable deep-readonly utility type.

# 🗁 Custom DeepReadonly Utility Type

You can create a recursive utility type that makes nested object properties readonly.

```typescript
type DeepReadonly<T> = {
  readonly [K in keyof T]: T[K] extends object
    ? DeepReadonly<T[K]>
    : T[K];
};
```

Example:

```typescript
interface User {
  id: number;
  name: string;
  address: {
    city: string;
    country: string;
  };
}

type ImmutableUser = DeepReadonly<User>;
```

Create an object:

```typescript
const user: ImmutableUser = {
  id: 1,
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh",
  },
};
```

Both top-level and nested properties are readonly.

```typescript
// Error:
user.name = "Bob";

// Error:
user.address.city = "Chattogram";
```

This is a simplified example. Production deep-readonly utilities may need special handling for arrays, functions, maps, sets, dates, and other built-in types.

# 🗁 Practical Example: Application Configuration

Readonly properties are useful for configuration objects that should not be modified after initialization.

```typescript
interface AppConfig {
  readonly appName: string;
  readonly version: string;
  readonly environment: "development" | "production";
  readonly apiUrl: string;
}
```

Create the configuration:

```typescript
const config: AppConfig = {
  appName: "My Application",
  version: "1.0.0",
  environment: "production",
  apiUrl: "https://api.example.com",
};
```

Use the configuration:

```typescript
function displayConfig(config: AppConfig): void {
  console.log(`Application: ${config.appName}`);
  console.log(`Version: ${config.version}`);
  console.log(`Environment: ${config.environment}`);
  console.log(`API URL: ${config.apiUrl}`);
}

displayConfig(config);
```

Attempting to modify configuration properties produces TypeScript errors.

```typescript
// Error:
config.environment = "development";
```

# 🗁 Common Mistakes

**Assuming readonly means deeply immutable**

```typescript
interface User {
  readonly address: {
    city: string;
  };
}

const user: User = {
  address: {
    city: "Dhaka",
  },
};

// Allowed:
user.address.city = "Chattogram";
```

The nested property is still mutable.

**Assuming const makes object properties readonly**

```typescript
const user = {
  name: "Alice",
};

// Valid:
user.name = "Bob";
```

Use a readonly type when property reassignment must be restricted.

**Confusing readonly with runtime protection**

```typescript
interface User {
  readonly id: number;
}
```

This restriction exists in TypeScript's type system. It does not automatically prevent JavaScript code from changing the property at runtime.

Use `Object.freeze()` when runtime freezing is also required.

**Trying to modify a readonly array**

```typescript
const values: readonly number[] = [10, 20, 30];

// Error:
values.push(40);
```

Create a new array instead:

```typescript
const updatedValues = [...values, 40];
```

# 🗁 Best Practices

* Use readonly for identifiers and properties that should not be reassigned.
* Use readonly arrays when functions should only read collection data.
* Prefer immutable data patterns when predictable state changes matter.
* Use `Readonly<T>` to make all top-level properties readonly.
* Use recursive utility types when nested readonly behavior is required.
* Remember that readonly provides compile-time protection, not runtime freezing.
* Use `Object.freeze()` when runtime immutability of an object's own properties is required.
* Avoid unnecessary readonly modifiers when properties are expected to change.
* Prefer creating new objects or arrays instead of mutating readonly data.
* Document important immutability requirements in shared application types.

# 🗁 Summary

* The `readonly` modifier prevents property reassignment through a readonly reference.
* Readonly properties are supported in interfaces, type aliases, and classes.
* Properties can be both optional and readonly.
* Readonly arrays and tuples prevent collection mutation through their readonly references.
* `Readonly<T>` makes all top-level properties readonly.
* Readonly properties remain readonly when inherited.
* `const` protects variable bindings, while `readonly` protects property assignments.
* `Object.freeze()` provides shallow runtime freezing.
* Readonly does not automatically make nested objects immutable.
* DeepReadonly patterns can protect nested object structures at the type level.

# 🗁 Related Topics

* Object Types
* Type Aliases
* Interfaces
* Interface Extension
* Interface Merging
* Optional Properties
* Arrays
* Tuples
* Utility Types
* Generic Types
* Immutability
* Object.freeze()
* Structural Typing
