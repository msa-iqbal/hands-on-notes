# TypeScript Record Utility Type

The `Record<K, T>` utility type constructs an object type whose property keys are defined by `K` and whose property values are defined by `T`.

It is useful when creating dictionaries, lookup tables, configuration maps, status mappings, and objects with a consistent key-value structure.

# 🗁 Basic Syntax

```typescript
Record<Keys, Type>
```

* `Keys` represents the allowed property keys.
* `Type` represents the type of every property value.
* The resulting type requires every key represented by `Keys` to exist.

# 🗁 Basic Example

Create an object where every key is a string and every value is a number.

```typescript
type UserAges = Record<string, number>;

const ages: UserAges = {
  Alice: 25,
  Bob: 30,
  Charlie: 28
};

console.log(ages.Alice);
console.log(ages.Bob);
```

Here:

* `string` defines the key type.
* `number` defines the value type.
* Any string key can be used.

# 🗁 How Record Works

Conceptually, TypeScript defines `Record<K, T>` using a mapped type.

```typescript
type Record<K extends PropertyKey, T> = {
  [P in K]: T;
};
```

Explanation:

* `PropertyKey` represents `string`, `number`, or `symbol`.
* `K` defines the set of allowed keys.
* `P in K` iterates over every key.
* `T` defines the value type associated with each key.

# 🗁 Record with String Keys

Use `Record<string, T>` when an object can contain arbitrary string keys with a consistent value type.

```typescript
type ProductPrices = Record<string, number>;

const prices: ProductPrices = {
  laptop: 1200,
  keyboard: 75,
  mouse: 25
};

console.log(prices.laptop);
```

You can add additional string properties as long as their values are numbers.

```typescript
prices.monitor = 300;
prices.headphones = 80;
```

# 🗁 Record with Specific Keys

You can use a union of string literals to define a fixed set of keys.

```typescript
type Status = "pending" | "approved" | "rejected";

type StatusMessages = Record<Status, string>;

const messages: StatusMessages = {
  pending: "Your request is being processed.",
  approved: "Your request has been approved.",
  rejected: "Your request has been rejected."
};
```

Every key in the union is required.

Omitting a key produces a TypeScript error.

```typescript
const messages: StatusMessages = {
  pending: "Processing",
  approved: "Approved"
};

// Error: rejected is missing.
```

Adding an unsupported key to a fresh object literal also produces an error.

```typescript
const messages: StatusMessages = {
  pending: "Processing",
  approved: "Approved",
  rejected: "Rejected",
  completed: "Completed"
};

// Error: completed is not an allowed key.
```

# 🗁 Record with Number Keys

`Record` also supports numeric property keys.

```typescript
type UserScores = Record<number, string>;

const scores: UserScores = {
  101: "Excellent",
  102: "Good",
  103: "Average"
};

console.log(scores[101]);
```

JavaScript object property keys are ultimately strings or symbols. Numeric property access is converted to its corresponding string property key.

# 🗁 Record with Symbol Keys

You can use symbol keys when defining a record.

```typescript
const READ = Symbol("read");
const WRITE = Symbol("write");

type Permissions = Record<
  typeof READ | typeof WRITE,
  boolean
>;

const permissions: Permissions = {
  [READ]: true,
  [WRITE]: false
};

console.log(permissions[READ]);
```

This is useful when working with symbol-based object properties.

# 🗁 Record with Object Values

The value type does not have to be a primitive. It can be an interface, type alias, array, or another object type.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type UserDirectory = Record<string, User>;

const users: UserDirectory = {
  alice: {
    id: 1,
    name: "Alice",
    email: "alice@example.com"
  },
  bob: {
    id: 2,
    name: "Bob",
    email: "bob@example.com"
  }
};

console.log(users.alice.name);
```

Every property value must satisfy the `User` interface.

# 🗁 Record with Union Value Types

You can use a union type for the values.

```typescript
type Environment = "development" | "production";

type EnvironmentPorts = Record<Environment, number>;

const ports: EnvironmentPorts = {
  development: 3000,
  production: 8080
};
```

Each key must exist, but its value can be any number.

# 🗁 Practical Example: Application Configuration

`Record` is useful for maintaining configuration values for multiple environments.

```typescript
type Environment = "development" | "staging" | "production";

interface Config {
  apiUrl: string;
  debug: boolean;
}

const configurations: Record<Environment, Config> = {
  development: {
    apiUrl: "http://localhost:3000",
    debug: true
  },

  staging: {
    apiUrl: "https://staging.example.com",
    debug: true
  },

  production: {
    apiUrl: "https://api.example.com",
    debug: false
  }
};

function getConfig(environment: Environment): Config {
  return configurations[environment];
}

console.log(getConfig("production"));
```

This ensures that every supported environment has a corresponding configuration.

# 🗁 Practical Example: Role-Based Permissions

You can use `Record` to define permissions for application roles.

```typescript
type Role = "admin" | "editor" | "viewer";

type Permission = "create" | "read" | "update" | "delete";

type RolePermissions = Record<Role, Record<Permission, boolean>>;

const permissions: RolePermissions = {
  admin: {
    create: true,
    read: true,
    update: true,
    delete: true
  },

  editor: {
    create: true,
    read: true,
    update: true,
    delete: false
  },

  viewer: {
    create: false,
    read: true,
    update: false,
    delete: false
  }
};

function canAccess(
  role: Role,
  permission: Permission
): boolean {
  return permissions[role][permission];
}

console.log(canAccess("admin", "delete")); // true
console.log(canAccess("viewer", "delete")); // false
```

Nested `Record` types are useful for structured lookup tables.

# 🗁 Practical Example: API Route Mapping

You can use `Record` to define route handlers with consistent function signatures.

```typescript
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";

type RouteHandler = (path: string) => string;

const handlers: Record<HttpMethod, RouteHandler> = {
  GET: (path) => `Fetching ${path}`,
  POST: (path) => `Creating ${path}`,
  PUT: (path) => `Updating ${path}`,
  DELETE: (path) => `Deleting ${path}`
};

console.log(handlers.GET("/users"));
console.log(handlers.POST("/users"));
```

Every supported HTTP method must have a handler.

# 🗁 Record with Partial

You can combine `Record` with `Partial` when only some keys need to be provided.

```typescript
type Status = "pending" | "approved" | "rejected";

type StatusMessages = Partial<Record<Status, string>>;

const messages: StatusMessages = {
  pending: "Processing",
  approved: "Approved"
};
```

Unlike a regular `Record<Status, string>`, this type does not require every status key.

# 🗁 Record with Readonly

You can combine `Record` with `Readonly` to prevent property reassignment.

```typescript
type Status = "pending" | "approved" | "rejected";

type StatusCodes = Readonly<Record<Status, number>>;

const statusCodes: StatusCodes = {
  pending: 100,
  approved: 200,
  rejected: 400
};

// Error: Property is readonly.
statusCodes.pending = 500;
```

# 🗁 Record vs Index Signatures

Both `Record` and index signatures can describe objects with consistent key-value structures.

Using an index signature:

```typescript
interface ProductPrices {
  [key: string]: number;
}
```

Using `Record`:

```typescript
type ProductPrices = Record<string, number>;
```

Both describe objects whose string-keyed properties have numeric values.

However, `Record` is particularly convenient when defining a fixed set of keys.

```typescript
type Status = "pending" | "approved" | "rejected";

type StatusCodes = Record<Status, number>;
```

# 🗁 Record vs Map

`Record` describes a JavaScript object through TypeScript's type system, whereas `Map<K, V>` is a JavaScript collection with its own runtime API.

Using `Record`:

```typescript
const users: Record<string, string> = {
  user1: "Alice",
  user2: "Bob"
};

console.log(users.user1);
```

Using `Map`:

```typescript
const users = new Map<string, string>();

users.set("user1", "Alice");
users.set("user2", "Bob");

console.log(users.get("user1"));
```

| Feature | Record | Map |
| --- | --- | --- |
| Runtime structure | Plain object | Map collection |
| Key types | String, number, symbol | Any value |
| Built-in size property | No | Yes |
| Iteration | Object iteration methods | Direct iteration |
| Key-value operations | Object properties | `set()`, `get()`, `delete()` |
| Missing key | Usually `undefined` at runtime | `get()` returns `undefined` |

Use `Record` for object-shaped data with a known structure. Use `Map` when you need dynamic key-value collections and Map-specific behavior.

# 🗁 Important Considerations

* `Record<K, T>` is a compile-time utility type.
* It does not create an object at runtime.
* Every key in a finite key union is required unless combined with `Partial`.
* Every property value must match the specified value type.
* It supports string, number, and symbol property keys.
* `Record<string, T>` does not guarantee that an arbitrary key exists at runtime.
* For dynamic lookups, account for potentially missing properties.
* Use `Map` when keys need to be arbitrary JavaScript values or when Map-specific operations are required.

# 🗁 Best Practices

* Use `Record` for fixed key-value structures.
* Prefer literal unions when the allowed keys are known.
* Use nested records for structured lookup tables.
* Combine `Record` with `Partial` when entries are optional.
* Combine `Record` with `Readonly` when values should not be reassigned.
* Use interfaces when defining more complex object contracts.
* Use `Map` when dynamic collection behavior is more appropriate.

# 🗁 Key Takeaways

* `Record<K, T>` creates an object type with keys `K` and values `T`.
* It is implemented using mapped types.
* It supports string, number, and symbol keys.
* It requires every key in a finite key union.
* It is useful for configuration maps, dictionaries, permissions, and lookup tables.
* It can be combined with other utility types.
* It complements interfaces and index signatures.
