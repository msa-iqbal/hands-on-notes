# typeof Operator

The `typeof` operator in TypeScript is used to refer to the type of an existing variable, property, or value.

It has two related forms:

* **JavaScript `typeof`:** A runtime operator that returns a string describing a value's runtime type.
* **TypeScript type query `typeof`:** A compile-time operator that obtains the static type of a value.

The TypeScript form is particularly useful when you want to derive types from existing variables, objects, functions, and constants instead of defining the same structure manually.

Common use cases include:

* Deriving types from existing objects.
* Creating reusable types from constants.
* Extracting function types.
* Working with configuration objects.
* Combining `typeof` with `keyof`.
* Building type-safe APIs.
* Preserving relationships between runtime values and static types.

# 🗁 Basic Syntax

Use `typeof` in a type position to obtain the type of an existing value.

```typescript
const username = "Alice";

type Username = typeof username;
```

The resulting type is:

```typescript
type Username = string;
```

The type query references the variable's static type.

Another example:

```typescript
const age = 25;
const active = true;

type Age = typeof age;
type Active = typeof active;
```

Results:

```typescript
type Age = number;
type Active = boolean;
```

# 🗁 JavaScript typeof vs TypeScript typeof

Although they use the same keyword, their purposes are different.

**JavaScript runtime typeof**

```typescript
const name = "Alice";
const age = 25;
const active = true;

console.log(typeof name);   // "string"
console.log(typeof age);    // "number"
console.log(typeof active); // "boolean"
```

This operator executes at runtime and returns a string.

**TypeScript type query typeof**

```typescript
const name = "Alice";
const age = 25;

type Name = typeof name;
type Age = typeof age;
```

This operator is evaluated during type checking and produces types.

Comparison:

| Feature | JavaScript `typeof` | TypeScript `typeof` |
| --- | --- | --- |
| Purpose | Checks runtime type category | Obtains a static type |
| Execution | Runtime | Compile time |
| Result | String | Type |
| Syntax | Expression position | Type position |
| Example | `typeof value` | `type T = typeof value` |

# 🗁 typeof with Variables

You can derive a type from any variable that has a statically known type.

```typescript
let username = "Alice";
let age = 25;
let isActive = true;

type Username = typeof username;
type Age = typeof age;
type IsActive = typeof isActive;
```

Results:

```typescript
type Username = string;
type Age = number;
type IsActive = boolean;
```

For mutable variables, TypeScript generally infers a widened type.

For example:

```typescript
let status = "active";

type Status = typeof status;
```

The resulting type is `string`, not the literal type `"active"`.

# 🗁 typeof with const Variables

A `const` variable cannot be reassigned, but its inferred type is not always a literal type.

```typescript
const username = "Alice";

type Username = typeof username;
```

Result:

```typescript
type Username = "Alice";
```

Because the variable is a `const` initialized with a primitive literal, TypeScript preserves the literal type.

For object properties, the behavior is different:

```typescript
const user = {
  name: "Alice",
  age: 25,
};

type User = typeof user;
```

The inferred type is:

```typescript
type User = {
  name: string;
  age: number;
};
```

The object variable cannot be reassigned, but its properties remain mutable, so their types are widened.

Use `as const` when you need to preserve literal property types.

# 🗁 typeof with Objects

One of the most useful applications of `typeof` is deriving an object type from an existing object.

```typescript
const user = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  active: true,
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

You can now use the derived type elsewhere.

```typescript
const admin: User = {
  id: 2,
  name: "Bob",
  email: "bob@example.com",
  active: true,
};
```

This avoids duplicating the object structure.

# 🗁 typeof with Nested Objects

The type query also captures nested object structures.

```typescript
const employee = {
  id: 101,
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh",
  },
};
```

Derive its type:

```typescript
type Employee = typeof employee;
```

The resulting type is equivalent to:

```typescript
type Employee = {
  id: number;
  name: string;
  address: {
    city: string;
    country: string;
  };
};
```

You can also extract a nested property type using indexed access types.

```typescript
type Address = Employee["address"];
type City = Employee["address"]["city"];
```

Results:

```typescript
type Address = {
  city: string;
  country: string;
};

type City = string;
```

# 🗁 typeof with Arrays

You can derive an array type from an existing array.

```typescript
const numbers = [10, 20, 30];

type Numbers = typeof numbers;
```

Result:

```typescript
type Numbers = number[];
```

Another example:

```typescript
const users = [
  {
    id: 1,
    name: "Alice",
  },
  {
    id: 2,
    name: "Bob",
  },
];

type Users = typeof users;
```

The resulting type is equivalent to:

```typescript
type Users = {
  id: number;
  name: string;
}[];
```

# 🗁 typeof with Tuple Types

When used with an ordinary array, `typeof` usually produces an array type.

To preserve individual element positions and types, use `as const`.

```typescript
const coordinates = [23.8103, 90.4125] as const;

type Coordinates = typeof coordinates;
```

Result:

```typescript
type Coordinates = readonly [23.8103, 90.4125];
```

Without `as const`:

```typescript
const coordinates = [23.8103, 90.4125];

type Coordinates = typeof coordinates;
```

Result:

```typescript
type Coordinates = number[];
```

The `as const` assertion preserves the tuple structure, literal element types, and readonly modifier.

# 🗁 typeof with Functions

You can derive the complete function type from an existing function.

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
  b: number,
) => number;
```

You can use the derived type for another function.

```typescript
const calculate: AddFunction = (a, b) => {
  return a * b;
};
```

The new function must follow the derived function signature.

# 🗁 typeof with Arrow Functions

The same approach works with arrow functions.

```typescript
const greet = (name: string): string => {
  return `Hello, ${name}`;
};

type GreetFunction = typeof greet;
```

Result:

```typescript
type GreetFunction = (name: string) => string;
```

Usage:

```typescript
const welcome: GreetFunction = (name) => {
  return `Welcome, ${name}`;
};
```

# 🗁 typeof with Function Properties

You can derive a type from a function stored inside an object.

```typescript
const userService = {
  getUser(id: number) {
    return {
      id,
      name: "Alice",
    };
  },

  deleteUser(id: number): boolean {
    return id > 0;
  },
};

type UserService = typeof userService;
```

Extract individual function types:

```typescript
type GetUser = typeof userService.getUser;
type DeleteUser = typeof userService.deleteUser;
```

Results:

```typescript
type GetUser = (id: number) => {
  id: number;
  name: string;
};

type DeleteUser = (id: number) => boolean;
```

This is useful for defining service contracts and reusable function signatures.

# 🗁 typeof with keyof

Combining `typeof` and `keyof` is a common TypeScript pattern.

* `typeof` obtains the type of a value.
* `keyof` extracts the property keys of that type.

Example:

```typescript
const user = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};

type User = typeof user;
type UserKeys = keyof User;
```

Result:

```typescript
type UserKeys = "id" | "name" | "email";
```

You can write this directly:

```typescript
type UserKeys = keyof typeof user;
```

This is especially useful when the object is the source of truth for a type.

# 🗁 typeof with Indexed Access Types

You can combine `typeof` with indexed access to retrieve property types.

```typescript
const product = {
  id: 101,
  title: "Keyboard",
  price: 75,
  available: true,
};

type Product = typeof product;

type ProductId = Product["id"];
type ProductTitle = Product["title"];
type ProductPrice = Product["price"];
```

Results:

```typescript
type ProductId = number;
type ProductTitle = string;
type ProductPrice = number;
```

You can also access a property type directly:

```typescript
type ProductPrice = typeof product["price"];
```

# 🗁 typeof with as const

The `as const` assertion is useful when deriving literal types from configuration objects.

```typescript
const settings = {
  theme: "dark",
  language: "en",
  notifications: true,
} as const;
```

Derive the type:

```typescript
type Settings = typeof settings;
```

Result:

```typescript
type Settings = {
  readonly theme: "dark";
  readonly language: "en";
  readonly notifications: true;
};
```

Unlike a regular object declaration, the property types remain their exact literal types.

You can derive individual values:

```typescript
type Theme = typeof settings.theme;
type Language = typeof settings.language;
```

Results:

```typescript
type Theme = "dark";
type Language = "en";
```

# 🗁 typeof with Enums

The `typeof` operator can obtain the type of an enum object.

```typescript
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

type DirectionObject = typeof Direction;
```

This represents the enum object, including its members.

To obtain the enum value type, use the enum name directly:

```typescript
type DirectionValue = Direction;
```

To obtain the names of the enum members:

```typescript
type DirectionName = keyof typeof Direction;
```

For a numeric enum, the result contains the member names:

```typescript
type DirectionName =
  | "Up"
  | "Down"
  | "Left"
  | "Right";
```

# 🗁 typeof with Class Constructors

The `typeof` operator can obtain the static side of a class.

```typescript
class User {
  constructor(
    public name: string,
  ) {}

  greet(): string {
    return `Hello, ${this.name}`;
  }
}

type UserInstance = User;
type UserConstructor = typeof User;
```

These types represent different things:

* `User` represents an instance of the class.
* `typeof User` represents the class constructor and static side.

You can use the constructor type to define a class reference.

```typescript
function createUser(
  UserClass: typeof User,
  name: string,
): User {
  return new UserClass(name);
}
```

Usage:

```typescript
const user = createUser(User, "Alice");
```

# 🗁 typeof with External Values

You can derive types from imported values.

Suppose a module exports an object:

```typescript
// config.ts

export const config = {
  appName: "My Application",
  port: 3000,
  debug: true,
};
```

Import the object:

```typescript
// app.ts

import { config } from "./config";

type Config = typeof config;
```

The resulting type is derived from the exported value.

You can also import only its type using an inline type query:

```typescript
import type { config } from "./config";

type Config = typeof config;
```

This uses the imported declaration only in a type position and does not require a runtime import.

# 🗁 typeof with Utility Types

The `typeof` operator works naturally with TypeScript utility types.

**ReturnType**

Extract a function's return type.

```typescript
function getUser() {
  return {
    id: 1,
    name: "Alice",
  };
}

type User = ReturnType<typeof getUser>;
```

Result:

```typescript
type User = {
  id: number;
  name: string;
};
```

**Parameters**

Extract a function's parameter types as a tuple.

```typescript
function createUser(
  name: string,
  age: number,
  active: boolean,
) {}

type CreateUserParams = Parameters<typeof createUser>;
```

Result:

```typescript
type CreateUserParams = [
  name: string,
  age: number,
  active: boolean,
];
```

**InstanceType**

Extract an instance type from a class constructor.

```typescript
class Product {
  constructor(
    public title: string,
    public price: number,
  ) {}
}

type ProductInstance = InstanceType<typeof Product>;
```

Result:

```typescript
type ProductInstance = Product;
```

These combinations are common in advanced TypeScript development.

# 🗁 Practical Example: Application Configuration

Define an application configuration object.

```typescript
export const appConfig = {
  appName: "My Application",
  version: "1.0.0",
  environment: "production",
  port: 3000,
  debug: false,
} as const;
```

Derive the configuration type:

```typescript
export type AppConfig = typeof appConfig;
```

Use the derived type:

```typescript
function initializeApp(config: AppConfig): void {
  console.log(config.appName);
  console.log(config.port);
  console.log(config.environment);
}

initializeApp(appConfig);
```

The object remains the source of truth for the configuration structure.

# 🗁 Practical Example: Type-Safe Routes

Define routes as a constant object.

```typescript
const routes = {
  home: "/",
  login: "/login",
  dashboard: "/dashboard",
  profile: "/profile",
} as const;
```

Extract the route names:

```typescript
type RouteName = keyof typeof routes;
```

Extract the route paths:

```typescript
type RoutePath = (typeof routes)[RouteName];
```

Results:

```typescript
type RouteName =
  | "home"
  | "login"
  | "dashboard"
  | "profile";

type RoutePath =
  | "/"
  | "/login"
  | "/dashboard"
  | "/profile";
```

Create a type-safe route helper:

```typescript
function navigate(route: RouteName): void {
  const path = routes[route];

  console.log(`Navigating to ${path}`);
}
```

Usage:

```typescript
navigate("dashboard");
navigate("profile");

// TypeScript error
// navigate("settings");
```

# 🗁 Practical Example: Deriving API Response Types

Define a sample API response.

```typescript
const apiResponse = {
  success: true,
  data: {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
  },
  message: "User retrieved successfully",
};
```

Derive its type:

```typescript
type ApiResponse = typeof apiResponse;
```

Extract nested types:

```typescript
type ApiData = ApiResponse["data"];
type ApiUser = ApiData;
type ApiMessage = ApiResponse["message"];
```

You can now reuse these types in other parts of the application.

# 🗁 Practical Example: Function Contracts

Define a function:

```typescript
function calculateTotal(
  price: number,
  quantity: number,
): number {
  return price * quantity;
}
```

Derive its type:

```typescript
type CalculateTotal = typeof calculateTotal;
```

Create another compatible function:

```typescript
const calculateDiscountedTotal: CalculateTotal = (
  price,
  quantity,
) => {
  return price * quantity * 0.9;
};
```

This keeps related functions consistent without repeating their signatures.

# 🗁 Limitations and Important Considerations

* TypeScript's type-query `typeof` works only in type positions.
* JavaScript's runtime `typeof` returns strings and cannot produce TypeScript types.
* `typeof` derives a static type; it does not inspect runtime values during execution.
* Object properties are generally widened unless literal preservation is requested.
* `as const` preserves literal values and adds readonly modifiers.
* A type derived from a variable reflects its declared or inferred type, not necessarily every possible runtime value.
* `typeof` does not validate external API responses or untrusted data.
* Types are erased during compilation, so TypeScript type queries do not create runtime type information.

# 🗁 Common Mistakes

**Using typeof in a type declaration without an existing value**

```typescript
// TypeScript error
// type User = typeof user;
```

The variable must be declared or imported before its type can be queried.

Correct:

```typescript
const user = {
  id: 1,
  name: "Alice",
};

type User = typeof user;
```

**Confusing runtime typeof with a type query**

```typescript
const age = 25;

const result = typeof age;
```

Here, `result` is a runtime string with the value `"number"`.

To obtain the static type:

```typescript
type Age = typeof age;
```

**Expecting object properties to preserve literal types**

```typescript
const settings = {
  theme: "dark",
};

type Theme = typeof settings.theme;
```

The resulting type is `string`, not `"dark"`.

Use `as const`:

```typescript
const settings = {
  theme: "dark",
} as const;

type Theme = typeof settings.theme;
```

Now the type is `"dark"`.

**Using keyof directly on a value**

```typescript
const user = {
  id: 1,
  name: "Alice",
};

// TypeScript error
// type Keys = keyof user;
```

Correct:

```typescript
type Keys = keyof typeof user;
```

**Assuming typeof performs runtime validation**

```typescript
const response: unknown = JSON.parse(
  '{"id": 1, "name": "Alice"}',
);
```

Writing a type query cannot prove that the parsed response has a particular structure. Runtime validation is required before treating untrusted data as a specific type.

# 🗁 Best Practices

* Derive types from existing values when the value is the source of truth.
* Use `typeof` with `keyof` to derive object key unions.
* Use `typeof` with indexed access types to extract property types.
* Use `as const` when exact literal values are important.
* Use `ReturnType` and `Parameters` with `typeof` to derive function-related types.
* Use `InstanceType<typeof ClassName>` to derive class instance types.
* Avoid duplicating object and function structures manually.
* Keep runtime checks separate from compile-time type derivation.
* Use descriptive type aliases for complex derived types.
* Prefer derived types when they reduce duplication and improve consistency.

# 🗁 Summary

* TypeScript's `typeof` type query obtains the static type of an existing value.
* JavaScript's runtime `typeof` returns a string describing a value's runtime type.
* It works with variables, objects, arrays, functions, classes, and imported values.
* It can derive nested object and function types.
* `keyof typeof value` extracts keys from an existing object's type.
* `as const` preserves literal values and readonly properties.
* It works with utility types such as `ReturnType`, `Parameters`, and `InstanceType`.
* It helps keep type definitions synchronized with runtime values.
* It does not perform runtime validation.

# 🗁 Related Topics

* Generic Basics
* Generic Constraints
* keyof Operator
* Indexed Access Types
* Literal Types
* as const
* Type Inference
* Utility Types
* ReturnType
* Parameters
* InstanceType
* Type Queries
