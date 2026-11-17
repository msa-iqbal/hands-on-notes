# TypeScript Required Utility Type

The `Required<T>` utility type constructs a new type by making all properties of an existing type required.

It is useful when working with configuration objects, form data, API responses, and situations where optional properties must become mandatory.

# 🗁 Basic Syntax

```typescript
Required<Type>
```

* `Required` is a built-in TypeScript utility type.
* `Type` represents the original type.
* Every optional property becomes required in the resulting type.

# 🗁 Basic Example

Consider an interface containing optional properties.

```typescript
interface User {
  id: number;
  name?: string;
  email?: string;
  age?: number;
}
```

The following object is valid because only `id` is required.

```typescript
const user: User = {
  id: 1
};
```

Now use `Required<User>` to make every property mandatory.

```typescript
type CompleteUser = Required<User>;
```

A `CompleteUser` object must contain every property.

```typescript
const user: CompleteUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  age: 25
};
```

Omitting any property produces a TypeScript error.

```typescript
const user: CompleteUser = {
  id: 1,
  name: "Alice"
};

// Error: email and age are missing.
```

# 🗁 How Required Works

Conceptually, TypeScript defines `Required<T>` using a mapped type.

```typescript
type Required<T> = {
  [P in keyof T]-?: T[P];
};
```

Explanation:

* `keyof T` extracts all property keys from type `T`.
* `P in keyof T` iterates over those properties.
* `-?` removes the optional modifier.
* `T[P]` preserves each property's original value type.

The `-?` syntax is important because it removes optionality rather than adding it.

# 🗁 Required with Interfaces

You can use `Required<T>` with an interface to create a new type without modifying the original interface.

```typescript
interface Product {
  id: number;
  name: string;
  description?: string;
  price?: number;
}

type CompleteProduct = Required<Product>;

const product: CompleteProduct = {
  id: 1,
  name: "Laptop",
  description: "Professional laptop",
  price: 1200
};
```

The original `Product` type remains unchanged.

```typescript
const basicProduct: Product = {
  id: 2,
  name: "Keyboard"
};
```

# 🗁 Required with Type Aliases

`Required<T>` also works with object type aliases.

```typescript
type Settings = {
  theme?: "light" | "dark";
  language?: string;
  notifications?: boolean;
};

type CompleteSettings = Required<Settings>;

const settings: CompleteSettings = {
  theme: "dark",
  language: "en",
  notifications: true
};
```

# 🗁 Required with Functions

You can use `Required<T>` to ensure that a function receives an object containing all properties.

```typescript
interface UserInput {
  name?: string;
  email?: string;
  age?: number;
}

function createUser(data: Required<UserInput>): void {
  console.log(`Name: ${data.name}`);
  console.log(`Email: ${data.email}`);
  console.log(`Age: ${data.age}`);
}

createUser({
  name: "Alice",
  email: "alice@example.com",
  age: 25
});
```

A function using `Required<UserInput>` cannot accept an object that omits any of these properties.

# 🗁 Practical Example: Application Configuration

Applications often start with optional configuration properties but require a complete configuration before initialization.

```typescript
interface AppConfig {
  theme?: "light" | "dark";
  language?: string;
  apiUrl?: string;
  notifications?: boolean;
}

type CompleteAppConfig = Required<AppConfig>;

const defaultConfig: CompleteAppConfig = {
  theme: "light",
  language: "en",
  apiUrl: "https://api.example.com",
  notifications: true
};

function initializeApp(config: CompleteAppConfig): void {
  console.log("Application initialized");
  console.log(config);
}

initializeApp(defaultConfig);
```

This approach separates optional input configuration from the complete configuration required by the application.

# 🗁 Practical Example: Applying Defaults

You can combine `Partial<T>` and `Required<T>` to accept optional user configuration while producing a complete result.

```typescript
interface AppConfig {
  theme: "light" | "dark";
  language: string;
  notifications: boolean;
}

const defaultConfig: AppConfig = {
  theme: "light",
  language: "en",
  notifications: true
};

function createConfig(
  options: Partial<AppConfig>
): Required<AppConfig> {
  return {
    ...defaultConfig,
    ...options
  };
}

const config = createConfig({
  theme: "dark",
  notifications: false
});

console.log(config);
```

Output:

```typescript
{
  theme: "dark",
  language: "en",
  notifications: false
}
```

Here:

* `Partial<AppConfig>` allows callers to provide only selected properties.
* `Required<AppConfig>` represents the complete configuration returned by the function.
* Object spreading combines default values with user-provided values.

# 🗁 Required with Readonly Properties

`Required<T>` preserves existing `readonly` modifiers.

```typescript
interface User {
  readonly id?: number;
  name?: string;
}

type CompleteUser = Required<User>;

const user: CompleteUser = {
  id: 1,
  name: "Alice"
};

// Error: id remains readonly.
user.id = 2;

// Valid: name is required but writable.
user.name = "Bob";
```

Making a property required does not make it writable.

# 🗁 Required with Partial

`Partial<T>` and `Required<T>` perform opposite operations on optional properties.

```typescript
interface User {
  id: number;
  name?: string;
  email?: string;
}

type OptionalUser = Partial<User>;
type CompleteUser = Required<User>;
```

Their behavior:

| Utility Type | Behavior |
| --- | --- |
| `Partial<T>` | Makes every property optional |
| `Required<T>` | Makes every property required |

Example:

```typescript
const user1: OptionalUser = {};

const user2: CompleteUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com"
};
```

# 🗁 Required with Nested Objects

`Required<T>` is shallow. It only removes optionality from the top-level properties.

```typescript
interface User {
  name?: string;
  address?: {
    city?: string;
    country?: string;
  };
}

type CompleteUser = Required<User>;
```

The `name` and `address` properties become required, but nested properties remain optional.

```typescript
const user: CompleteUser = {
  name: "Alice",
  address: {
    city: "Dhaka"
  }
};
```

The nested `country` property is still optional.

# 🗁 Removing Optionality vs Removing Undefined

An optional property and a property whose value type explicitly includes `undefined` are not exactly the same.

```typescript
interface Example {
  optionalValue?: string;
  explicitUndefined: string | undefined;
}

type CompleteExample = Required<Example>;
```

With `strictNullChecks` enabled, `Required<Example>` removes optionality from `optionalValue`.

The behavior of explicitly declared `undefined` depends on TypeScript's exact optional property type settings. With `exactOptionalPropertyTypes` enabled, `Required<T>` does not remove an explicitly declared `undefined` from the value type.

For example:

```typescript
interface Example {
  value?: string | undefined;
}

type CompleteExample = Required<Example>;
```

With `exactOptionalPropertyTypes` enabled, `value` is required but its value type still includes `undefined`.

# 🗁 Important Considerations

* `Required<T>` is a compile-time utility type.
* It does not insert missing properties at runtime.
* It does not generate default values.
* It only affects optionality at the top level.
* Existing property types are preserved.
* Existing `readonly` modifiers are preserved.
* It is commonly combined with `Partial<T>` when processing optional input and returning complete objects.

# 🗁 Best Practices

* Use `Required<T>` when all properties must be present.
* Keep the original type when optional properties are meaningful.
* Combine `Partial<T>` and `Required<T>` for configuration workflows.
* Use runtime validation when data comes from external sources.
* Do not assume `Required<T>` automatically fills missing values.
* Use a separate recursive utility when nested properties must also become required.

# 🗁 Key Takeaways

* `Required<T>` makes all properties of a type mandatory.
* It is implemented using mapped types and the `-?` modifier.
* It preserves original property types and readonly modifiers.
* It only affects top-level properties.
* It does not modify objects at runtime.
* It is useful for configuration objects, complete data models, and function parameters.
* It complements `Partial<T>`.
