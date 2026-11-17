# Typeof Type Operator

The `typeof` type operator allows you to extract the TypeScript type of an existing variable, constant, function, or object.

It is particularly useful when you want to derive types from existing values instead of defining the same structure manually.

TypeScript provides two forms of `typeof`:

* **JavaScript `typeof`:** A runtime operator that returns a value's type as a string.
* **TypeScript `typeof`:** A type-level operator that retrieves the type of an existing identifier.

Understanding the difference between these two forms is essential when working with advanced TypeScript types.

# 🗁 JavaScript `typeof` vs TypeScript `typeof`

JavaScript `typeof` operates at runtime.

```typescript
const name = "John";
const age = 25;

console.log(typeof name); // "string"
console.log(typeof age);  // "number"
```

TypeScript `typeof` operates at compile time and retrieves the type of an existing value.

```typescript
const name = "John";
const age = 25;

type Name = typeof name;
type Age = typeof age;
```

Result:

```typescript
type Name = string;
type Age = number;
```

The TypeScript type-level operator does not generate additional JavaScript code.

# 🗁 Basic Syntax

The basic syntax is:

```typescript
type TypeName = typeof value;
```

Example:

```typescript
const username = "john";
const userAge = 25;
const isActive = true;

type Username = typeof username;
type UserAge = typeof userAge;
type IsActive = typeof isActive;
```

Result:

```typescript
type Username = string;
type UserAge = number;
type IsActive = boolean;
```

TypeScript derives each type from its corresponding variable.

# 🗁 Extracting Object Types

You can use `typeof` to retrieve the complete type of an existing object.

```typescript
const user = {
  id: 1,
  name: "John",
  email: "john@example.com",
  active: true
};

type User = typeof user;
```

Result:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
  active: boolean;
};
```

You can then use the derived type to create additional objects.

```typescript
const anotherUser: User = {
  id: 2,
  name: "Alex",
  email: "alex@example.com",
  active: true
};
```

This avoids duplicating the object structure.

# 🗁 Extracting Types from Constants

By default, TypeScript often widens literal values in mutable variables and object properties.

```typescript
const user = {
  name: "John",
  role: "admin"
};

type User = typeof user;
```

Result:

```typescript
type User = {
  name: string;
  role: string;
};
```

Although `user` is declared with `const`, its object properties remain mutable.

Use `as const` to preserve literal types.

```typescript
const user = {
  name: "John",
  role: "admin"
} as const;

type User = typeof user;
```

Result:

```typescript
type User = {
  readonly name: "John";
  readonly role: "admin";
};
```

The `as const` assertion:

* Prevents literal values from widening.
* Makes object properties readonly.
* Converts array literals into readonly tuples.
* Preserves literal types throughout the expression.

# 🗁 Combining `typeof` with `keyof`

You can combine `typeof` and `keyof` to extract the keys of an existing object.

```typescript
const user = {
  id: 1,
  name: "John",
  email: "john@example.com"
};

type UserKeys = keyof typeof user;
```

Result:

```typescript
type UserKeys = "id" | "name" | "email";
```

Explanation:

* `typeof user` retrieves the object's type.
* `keyof` extracts the property keys from that type.

This combination is useful when deriving property names directly from existing objects.

# 🗁 Combining `typeof` with Indexed Access Types

You can use `typeof` and indexed access types to retrieve individual property types.

```typescript
const user = {
  id: 1,
  name: "John",
  active: true
};

type User = typeof user;

type UserId = User["id"];
type UserName = User["name"];
type UserStatus = User["active"];
```

Result:

```typescript
type UserId = number;
type UserName = string;
type UserStatus = boolean;
```

You can also write the expressions directly:

```typescript
type UserId = (typeof user)["id"];

type UserName = (typeof user)["name"];
```

Parentheses make the relationship between `typeof` and indexed access explicit.

# 🗁 Extracting Array Element Types

One of the most common uses of `typeof` is deriving an array's element type.

```typescript
const users = [
  { id: 1, name: "John" },
  { id: 2, name: "Alex" }
];

type User = typeof users[number];
```

Result:

```typescript
type User = {
  id: number;
  name: string;
};
```

Explanation:

* `typeof users` retrieves the array type.
* `[number]` accesses the array's element type.

This is especially useful when working with API data, configuration arrays, and collections.

# 🗁 Extracting Literal Unions from Arrays

Combining `typeof`, `as const`, and indexed access allows you to derive a union of literal values.

```typescript
const roles = ["admin", "editor", "user"] as const;

type Role = typeof roles[number];
```

Result:

```typescript
type Role = "admin" | "editor" | "user";
```

You can use the resulting type to restrict values:

```typescript
function assignRole(role: Role): void {
  console.log(`Assigned role: ${role}`);
}

assignRole("admin");
assignRole("editor");

// Error: "manager" is not assignable to Role.
assignRole("manager");
```

When the allowed values change, the derived union updates automatically.

# 🗁 Extracting Function Types

You can use `typeof` to retrieve the type of an existing function.

```typescript
function add(a: number, b: number): number {
  return a + b;
}

type AddFunction = typeof add;
```

Result:

```typescript
type AddFunction = (
  a: number,
  b: number
) => number;
```

You can use this type to define another function with the same signature.

```typescript
const subtract: AddFunction = (a, b) => {
  return a - b;
};
```

The new function must follow the extracted function signature.

# 🗁 Extracting Class Types

You can use `typeof` to retrieve the type of a class constructor.

```typescript
class User {
  constructor(
    public id: number,
    public name: string
  ) {}
}

type UserConstructor = typeof User;
```

The resulting type represents the class constructor, including its static side.

```typescript
const UserFactory: UserConstructor = User;

const user = new UserFactory(1, "John");
```

To retrieve the instance type of a class, use the class name directly:

```typescript
type UserInstance = User;
```

You can also use `InstanceType`:

```typescript
type UserInstance = InstanceType<typeof User>;
```

The distinction is important:

* `typeof User` represents the constructor and static side.
* `User` represents the instance type.
* `InstanceType<typeof User>` extracts the instance type from a constructor type.

# 🗁 Extracting Types from Enums

You can use `typeof` with enums to retrieve their object-like type.

```typescript
enum Status {
  Pending,
  Active,
  Completed
}

type StatusObject = typeof Status;
```

The resulting type describes the enum's static members.

To retrieve the enum's value type, use the enum name:

```typescript
type StatusValue = Status;
```

For example:

```typescript
const currentStatus: Status = Status.Active;
```

For string enums:

```typescript
enum UserRole {
  Admin = "admin",
  Editor = "editor",
  User = "user"
}

type UserRoleValue = UserRole;
type UserRoleObject = typeof UserRole;
```

These represent different aspects of the enum: its value type and its static object type.

# 🗁 Using `typeof` with Generic Utility Types

You can combine `typeof` with utility types such as `Parameters`, `ReturnType`, and `InstanceType`.

Example:

```typescript
function createUser(name: string, age: number) {
  return {
    id: 1,
    name,
    age
  };
}

type CreateUserFunction = typeof createUser;

type CreateUserParameters =
  Parameters<CreateUserFunction>;

type CreateUserReturn =
  ReturnType<CreateUserFunction>;
```

Result:

```typescript
type CreateUserParameters = [
  name: string,
  age: number
];

type CreateUserReturn = {
  id: number;
  name: string;
  age: number;
};
```

This allows you to derive function-related types without manually recreating their signatures.

# 🗁 Using `typeof` in Type Queries

TypeScript supports type queries using `typeof`.

A type query can reference an existing identifier or its accessible properties.

```typescript
const settings = {
  theme: "dark",
  language: "en"
};

type Settings = typeof settings;

type Theme = typeof settings.theme;
```

Result:

```typescript
type Settings = {
  theme: string;
  language: string;
};

type Theme = string;
```

Type queries are useful when working with nested objects and existing application configuration.

# 🗁 Practical Example: Application Configuration

You can define configuration values once and derive their type automatically.

```typescript
export const appConfig = {
  appName: "My Application",
  version: "1.0.0",
  environment: "production",
  features: {
    authentication: true,
    notifications: false
  }
} as const;

export type AppConfig = typeof appConfig;
```

The resulting type preserves the literal values and readonly properties.

```typescript
type Environment = AppConfig["environment"];

type Features = AppConfig["features"];
```

Result:

```typescript
type Environment = "production";

type Features = {
  readonly authentication: true;
  readonly notifications: false;
};
```

This approach is useful for application settings, feature flags, and environment-specific configuration.

# 🗁 Practical Example: API Response Types

You can define a sample API response and derive reusable types from it.

```typescript
const apiResponse = {
  success: true,
  data: {
    id: 1,
    name: "John",
    email: "john@example.com"
  },
  message: "User retrieved successfully"
};

type ApiResponse = typeof apiResponse;

type ApiUser = ApiResponse["data"];

type ApiUserId = ApiUser["id"];
```

Result:

```typescript
type ApiUser = {
  id: number;
  name: string;
  email: string;
};

type ApiUserId = number;
```

For real API contracts, explicitly define the expected response type when inference from a sample value is too broad or does not accurately represent all possible responses.

# 🗁 Practical Example: Event Names

You can derive event names from an existing constant object.

```typescript
const events = {
  USER_CREATED: "user:created",
  USER_UPDATED: "user:updated",
  USER_DELETED: "user:deleted"
} as const;

type EventName = typeof events[keyof typeof events];
```

Result:

```typescript
type EventName =
  | "user:created"
  | "user:updated"
  | "user:deleted";
```

You can now create a function that accepts only valid event names.

```typescript
function publishEvent(event: EventName): void {
  console.log(`Publishing: ${event}`);
}

publishEvent("user:created");

// Error: Invalid event name.
publishEvent("user:logged-in");
```

# 🗁 Common Mistakes

**Using TypeScript `typeof` with arbitrary expressions**

Type-level `typeof` is used to query identifiers and their accessible properties, not to evaluate arbitrary expressions.

```typescript
const user = {
  id: 1,
  name: "John"
};

// Incorrect type query:
// type User = typeof getUser();
```

Instead, reference an existing value:

```typescript
const result = getUser();

type User = typeof result;
```

**Confusing object `const` with literal preservation**

```typescript
const user = {
  role: "admin"
};

type Role = typeof user.role;
```

Result:

```typescript
type Role = string;
```

Use `as const` when you need the literal type:

```typescript
const user = {
  role: "admin"
} as const;

type Role = typeof user.role;
```

Result:

```typescript
type Role = "admin";
```

**Confusing constructor and instance types**

```typescript
class User {
  id = 1;
}

type Constructor = typeof User;
type Instance = User;
```

`Constructor` describes the class constructor, while `Instance` describes objects created from that class.

**Expecting runtime values from type-level `typeof`**

```typescript
const user = {
  id: 1,
  name: "John"
};

type User = typeof user;
```

`User` exists only in the TypeScript type system. It does not create a JavaScript variable at runtime.

# 🗁 Best Practices

* Use `typeof` to derive types from existing values.
* Combine `typeof` with `keyof` to extract object property keys.
* Combine `typeof` with indexed access to retrieve specific property types.
* Use `as const` when literal values must be preserved.
* Use `typeof array[number]` to derive array element types.
* Use `typeof function` with `Parameters` and `ReturnType` to reuse function signatures.
* Distinguish class constructor types from instance types.
* Prefer a single source of truth for configuration values and their corresponding types.
* Avoid deriving public API contracts from incomplete sample data.

# 🗁 Summary

The TypeScript `typeof` type operator retrieves the type of an existing identifier or its accessible property.

Key concepts:

* `typeof variable` extracts a variable's type.
* `typeof object` extracts an object's complete type.
* `typeof object.property` extracts a specific property type.
* `keyof typeof object` extracts object keys.
* `typeof array[number]` extracts array element types.
* `typeof function` extracts a function signature.
* `typeof Class` extracts a class constructor type.
* `as const` preserves literal values and readonly properties.
* Utility types can be combined with `typeof` to derive parameters, return types, and instance types.

The `typeof` type operator is an essential tool for deriving reusable types directly from existing JavaScript values.
