# TypeScript Partial Utility Type

The `Partial<T>` utility type constructs a new type by making all properties of an existing type optional.

It is particularly useful when working with update operations, configuration objects, forms, and objects that may contain only a subset of the original properties.

# 🗁 Basic Syntax

```typescript
Partial<Type>
```

* `Partial` is a built-in TypeScript utility type.
* `Type` represents the original type.
* Every property in the resulting type becomes optional.

# 🗁 Basic Example

Consider an interface with several required properties.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}
```

Normally, an object must contain every required property.

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  age: 25
};
```

If you want to create an object containing only some properties, using the original type directly will produce an error.

```typescript
const updatedUser: User = {
  name: "Bob"
};

// Error: Required properties are missing.
```

Using `Partial<User>` makes every property optional.

```typescript
const updatedUser: Partial<User> = {
  name: "Bob"
};
```

Now the object can contain any subset of the original properties.

# 🗁 How Partial Works

Conceptually, TypeScript defines `Partial<T>` using a mapped type.

```typescript
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

Explanation:

* `keyof T` extracts the property keys from type `T`.
* `P in keyof T` iterates over those keys.
* `?` makes each property optional.
* `T[P]` preserves the original property type.

For example:

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
}

type PartialProduct = Partial<Product>;
```

The resulting type is conceptually equivalent to:

```typescript
type PartialProduct = {
  id?: number;
  name?: string;
  price?: number;
};
```

# 🗁 Partial with Functions

`Partial<T>` is especially useful when creating functions that accept incomplete objects.

```typescript
interface User {
  name: string;
  email: string;
  age: number;
}

function createUser(data: Partial<User>): void {
  console.log(data);
}

createUser({
  name: "Alice"
});

createUser({
  email: "bob@example.com",
  age: 30
});
```

Both calls are valid because every property is optional.

# 🗁 Practical Example: Updating User Information

A common use case is updating only selected properties of an existing object.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  active: boolean;
}

function updateUser(
  user: User,
  updates: Partial<User>
): User {
  return {
    ...user,
    ...updates
  };
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  active: true
};

const updatedUser = updateUser(user, {
  name: "Alice Smith",
  active: false
});

console.log(updatedUser);
```

Output:

```typescript
{
  id: 1,
  name: "Alice Smith",
  email: "alice@example.com",
  active: false
}
```

The spread operator creates a new object, preserving unchanged properties and overriding only the supplied properties.

# 🗁 Partial with Configuration Objects

Configuration objects often need default values while allowing users to override only selected settings.

```typescript
interface AppConfig {
  theme: "light" | "dark";
  language: string;
  notifications: boolean;
  autoSave: boolean;
}

const defaultConfig: AppConfig = {
  theme: "light",
  language: "en",
  notifications: true,
  autoSave: true
};

function configureApp(
  options: Partial<AppConfig>
): AppConfig {
  return {
    ...defaultConfig,
    ...options
  };
}

const config = configureApp({
  theme: "dark",
  autoSave: false
});

console.log(config);
```

Output:

```typescript
{
  theme: "dark",
  language: "en",
  notifications: true,
  autoSave: false
}
```

This pattern is useful for application settings, reusable libraries, and configurable components.

# 🗁 Partial with Nested Objects

`Partial<T>` is shallow. It only makes the top-level properties optional.

```typescript
interface Address {
  city: string;
  country: string;
}

interface User {
  name: string;
  email: string;
  address: Address;
}

type PartialUser = Partial<User>;
```

The following object is valid because `address` is optional:

```typescript
const user1: PartialUser = {
  name: "Alice"
};
```

However, if you provide `address`, all its original properties remain required.

```typescript
const user2: PartialUser = {
  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};
```

This is invalid:

```typescript
const user3: PartialUser = {
  address: {
    city: "Dhaka"
  }
};

// Error: country is required.
```

# 🗁 Creating a Deep Partial Type

If you need nested properties to become optional as well, you can define a recursive utility type.

```typescript
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object
    ? DeepPartial<T[P]>
    : T[P];
};
```

Example:

```typescript
interface User {
  name: string;
  email: string;
  address: {
    city: string;
    country: string;
  };
}

const user: DeepPartial<User> = {
  name: "Alice",
  address: {
    city: "Dhaka"
  }
};
```

Here, nested properties are also optional.

This simplified implementation has limitations with arrays, functions, dates, maps, and other built-in object types. Production-grade deep partial types may need special handling for these structures.

# 🗁 Partial with Readonly Properties

`Partial<T>` preserves existing property modifiers such as `readonly`.

```typescript
interface User {
  readonly id: number;
  name: string;
  email: string;
}

type PartialUser = Partial<User>;

const user: PartialUser = {
  id: 1,
  name: "Alice"
};

// Error: id remains readonly.
user.id = 2;

// Valid: name is optional and writable.
user.name = "Bob";
```

Making a property optional does not remove its existing readonly restriction.

# 🗁 Partial vs Required

`Partial<T>` makes every property optional, while `Required<T>` makes every property required.

```typescript
interface User {
  name?: string;
  email?: string;
}

type PartialUser = Partial<User>;
type RequiredUser = Required<User>;
```

The resulting types behave differently:

```typescript
const user1: PartialUser = {};

const user2: RequiredUser = {
  name: "Alice",
  email: "alice@example.com"
};
```

# 🗁 Important Considerations

* `Partial<T>` changes the type system, not the original runtime object.
* It does not create default values for missing properties.
* It only makes top-level properties optional.
* Existing property types and readonly modifiers are preserved.
* Reading an optional property may produce `undefined` in its type.
* Use object spreading or explicit assignment when applying partial updates.
* Validate incoming data at runtime when receiving untrusted input, such as API requests.

# 🗁 Best Practices

* Use `Partial<T>` for update payloads and optional configuration overrides.
* Keep the original type for objects that must contain all required properties.
* Remember that `Partial<T>` is shallow.
* Use a carefully designed `DeepPartial<T>` when nested partial updates are required.
* Avoid using `Partial<T>` as a substitute for proper validation.
* Prefer explicit update types when only a specific subset of properties should be modifiable.

# 🗁 Key Takeaways

* `Partial<T>` makes all properties of a type optional.
* It is a built-in TypeScript utility type.
* It is implemented using mapped types.
* It is commonly used for update operations and configuration objects.
* It only affects top-level properties.
* It preserves existing property types and modifiers.
* It does not change the runtime structure of an object.
