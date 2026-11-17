# TypeScript Readonly Utility Type

The `Readonly<T>` utility type constructs a new type by making all properties of an existing type read-only.

Once an object is assigned to a `Readonly<T>` type, its properties cannot be reassigned through that reference.

It is useful for immutable data structures, application configuration, shared constants, and preventing accidental modifications.

# 🗁 Basic Syntax

```typescript
Readonly<Type>
```

* `Readonly` is a built-in TypeScript utility type.
* `Type` represents the original type.
* Every property becomes read-only in the resulting type.

# 🗁 Basic Example

Consider an interface with ordinary writable properties.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com"
};

user.name = "Bob"; // Valid
```

Now apply `Readonly<User>`.

```typescript
type ReadonlyUser = Readonly<User>;

const user: ReadonlyUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com"
};

// Error: Cannot assign to a read-only property.
user.name = "Bob";
```

All properties become read-only.

# 🗁 How Readonly Works

Conceptually, TypeScript defines `Readonly<T>` using a mapped type.

```typescript
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```

Explanation:

* `keyof T` extracts all property keys from type `T`.
* `P in keyof T` iterates over those properties.
* `readonly` adds the read-only modifier.
* `T[P]` preserves the original property type.

# 🗁 Readonly with Interfaces

You can use `Readonly<T>` with interfaces without modifying the original interface.

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
}

type ReadonlyProduct = Readonly<Product>;

const product: ReadonlyProduct = {
  id: 1,
  name: "Laptop",
  price: 1200
};

// Error: id is read-only.
product.id = 2;

// Error: name is read-only.
product.name = "Desktop";

// Error: price is read-only.
product.price = 1000;
```

The original `Product` interface remains unchanged.

```typescript
const editableProduct: Product = {
  id: 2,
  name: "Keyboard",
  price: 75
};

editableProduct.price = 80; // Valid
```

# 🗁 Readonly with Type Aliases

`Readonly<T>` also works with object type aliases.

```typescript
type Settings = {
  theme: string;
  language: string;
  notifications: boolean;
};

type ImmutableSettings = Readonly<Settings>;

const settings: ImmutableSettings = {
  theme: "dark",
  language: "en",
  notifications: true
};

// Error: Cannot modify a read-only property.
settings.theme = "light";
```

# 🗁 Readonly with Functions

You can use `Readonly<T>` to prevent a function from modifying an object through its parameter.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

function displayUser(user: Readonly<User>): void {
  console.log(user.name);
  console.log(user.email);

  // Error: name is read-only.
  user.name = "Bob";
}
```

This communicates that the function only needs to read the supplied object.

# 🗁 Practical Example: Application Configuration

Application configuration values should often remain unchanged after initialization.

```typescript
interface AppConfig {
  appName: string;
  apiUrl: string;
  version: string;
  debug: boolean;
}

const config: Readonly<AppConfig> = {
  appName: "Task Manager",
  apiUrl: "https://api.example.com",
  version: "1.0.0",
  debug: false
};

function displayConfig(config: Readonly<AppConfig>): void {
  console.log(`Application: ${config.appName}`);
  console.log(`API URL: ${config.apiUrl}`);
  console.log(`Version: ${config.version}`);
  console.log(`Debug: ${config.debug}`);
}

displayConfig(config);
```

Attempting to modify a configuration property produces a TypeScript error.

```typescript
config.debug = true;
```

# 🗁 Readonly Arrays

TypeScript also provides a `ReadonlyArray<T>` type for arrays whose elements cannot be modified through that reference.

```typescript
const numbers: ReadonlyArray<number> = [10, 20, 30];

console.log(numbers[0]);

// Error: Property 'push' does not exist.
numbers.push(40);

// Error: Index signature only permits reading.
numbers[0] = 100;
```

You can also use the shorthand syntax:

```typescript
const names: readonly string[] = [
  "Alice",
  "Bob",
  "Charlie"
];
```

The following operations are not allowed through a readonly array reference:

* `push()`
* `pop()`
* `shift()`
* `unshift()`
* `splice()`
* Direct index assignment

Non-mutating methods such as `map()`, `filter()`, and `slice()` remain available.

# 🗁 Readonly with Tuples

You can create readonly tuples using the `readonly` modifier.

```typescript
const coordinates: readonly [number, number] = [10, 20];

console.log(coordinates[0]);
console.log(coordinates[1]);

// Error: Cannot assign to a readonly tuple element.
coordinates[0] = 50;
```

Readonly tuples are useful for fixed collections of values that should not be modified.

# 🗁 Readonly with Nested Objects

`Readonly<T>` is shallow. It only makes the top-level properties read-only.

```typescript
interface User {
  name: string;
  address: {
    city: string;
    country: string;
  };
}

const user: Readonly<User> = {
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

// Error: name is read-only.
user.name = "Bob";

// Valid: nested properties are not automatically readonly.
user.address.city = "Chittagong";
```

The `address` reference cannot be reassigned, but its nested properties remain writable.

```typescript
// Error: address itself is readonly.
user.address = {
  city: "London",
  country: "United Kingdom"
};
```

# 🗁 Creating a Deep Readonly Type

If you need nested properties to become read-only, you can create a recursive utility type.

```typescript
type DeepReadonly<T> = {
  readonly [P in keyof T]: T[P] extends object
    ? DeepReadonly<T[P]>
    : T[P];
};
```

Example:

```typescript
interface User {
  name: string;
  address: {
    city: string;
    country: string;
  };
}

const user: DeepReadonly<User> = {
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

// Error: name is readonly.
user.name = "Bob";

// Error: nested city is also readonly.
user.address.city = "London";
```

This simplified implementation has limitations with functions, arrays, maps, sets, dates, and other built-in object types. A production-grade deep readonly utility may require specialized handling for those types.

# 🗁 Readonly vs Const

`const` and `readonly` solve different problems.

| Feature | `const` | `readonly` |
| --- | --- | --- |
| Applies to | Variables | Object properties and class properties |
| Prevents reassignment | Variable binding | Property assignment |
| Prevents nested mutation | No | No, unless nested properties are also readonly |
| Used with | Variable declarations | Types, interfaces, and classes |

Example:

```typescript
const user = {
  name: "Alice"
};

// Valid: object properties remain writable.
user.name = "Bob";

// Error: cannot reassign a const variable.
user = {
  name: "Charlie"
};
```

Using `readonly`:

```typescript
interface User {
  readonly name: string;
}

const user: User = {
  name: "Alice"
};

// Error: name is readonly.
user.name = "Bob";
```

# 🗁 Readonly vs Object.freeze()

`Readonly<T>` is a compile-time restriction, while `Object.freeze()` is a JavaScript runtime operation.

```typescript
interface User {
  name: string;
  age: number;
}

const user: Readonly<User> = {
  name: "Alice",
  age: 25
};
```

TypeScript prevents property assignments during type checking, but it does not automatically freeze the object at runtime.

JavaScript's `Object.freeze()` provides runtime protection against modifying an object's own properties.

```typescript
const settings = Object.freeze({
  theme: "dark",
  language: "en"
});

// Runtime modification is prevented.
```

`Object.freeze()` is also shallow. Nested objects are not automatically frozen.

# 🗁 Practical Example: Immutable Data Processing

Readonly parameters can help prevent accidental modification when processing application data.

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
}

function calculateTotal(
  products: readonly Product[]
): number {
  return products.reduce(
    (total, product) => total + product.price,
    0
  );
}

const products: Product[] = [
  { id: 1, name: "Keyboard", price: 75 },
  { id: 2, name: "Mouse", price: 25 },
  { id: 3, name: "Monitor", price: 300 }
];

console.log(calculateTotal(products)); // 400
```

The function can read and process the array but cannot directly add, remove, or replace its elements.

Note that `readonly Product[]` prevents changing the array structure, not the properties of each `Product`. To make both levels read-only, use `ReadonlyArray<Readonly<Product>>`.

# 🗁 Important Considerations

* `Readonly<T>` is a compile-time utility type.
* It does not automatically freeze an object at runtime.
* It makes only top-level properties read-only.
* Nested objects require a deep readonly strategy.
* Readonly arrays prevent modifications through that array reference.
* Readonly properties can still be initialized during object creation.
* Another mutable reference to the same object may still modify it.

# 🗁 Best Practices

* Use `Readonly<T>` for data that functions should only inspect.
* Use readonly arrays for collections that should not be modified by consumers.
* Prefer readonly properties for identifiers and immutable configuration values.
* Use `Object.freeze()` when runtime protection is required.
* Remember that readonly restrictions are shallow by default.
* Avoid unnecessary readonly restrictions when mutation is part of the intended design.

# 🗁 Key Takeaways

* `Readonly<T>` makes every property read-only.
* It is implemented using mapped types.
* It preserves original property types and optionality.
* It does not provide runtime immutability.
* It only affects top-level properties.
* `ReadonlyArray<T>` prevents array modifications through a readonly reference.
* `DeepReadonly<T>` can provide recursive readonly behavior when implemented appropriately.
