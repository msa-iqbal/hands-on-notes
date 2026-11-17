# Type Aliases

A type alias allows you to create a custom name for an existing type or a more complex type expression.

TypeScript uses the `type` keyword to declare type aliases.

Type aliases help make code easier to read, maintain, and reuse by giving meaningful names to types.

They are useful when:

* Reusing object structures across multiple files.
* Defining union and intersection types.
* Creating custom types for functions and tuples.
* Simplifying complex type expressions.
* Improving the readability of application code.

# 🗁 Basic Syntax

Use the `type` keyword followed by a name and the type definition.

```typescript
type TypeName = Type;
```

Example:

```typescript
type UserId = number;

let userId: UserId = 101;

console.log(userId);
```

Here, `UserId` is an alias for the `number` type.

The alias does not create a new runtime type. It exists only during type checking.

# 🗁 Type Aliases for Primitive Types

You can create aliases for primitive types such as `string`, `number`, and `boolean`.

```typescript
type Username = string;
type Age = number;
type IsActive = boolean;

const username: Username = "Alice";
const age: Age = 25;
const isActive: IsActive = true;
```

This can make domain-specific values easier to understand.

# 🗁 Type Aliases for Object Types

Type aliases are commonly used to define object structures.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
  active: boolean;
};

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  active: true,
};
```

The same alias can be reused wherever the application needs a `User` object.

```typescript
function printUser(user: User): void {
  console.log(user.name);
}

function createUser(): User {
  return {
    id: 2,
    name: "Bob",
    email: "bob@example.com",
    active: true,
  };
}
```

# 🗁 Type Aliases for Arrays

You can use type aliases to describe arrays.

```typescript
type Numbers = number[];

const scores: Numbers = [85, 90, 95];
```

You can also define arrays of objects.

```typescript
type Product = {
  id: number;
  name: string;
  price: number;
};

type Products = Product[];

const products: Products = [
  {
    id: 1,
    name: "Keyboard",
    price: 75,
  },
  {
    id: 2,
    name: "Mouse",
    price: 25,
  },
];
```

# 🗁 Type Aliases for Tuple Types

Type aliases can give descriptive names to tuple structures.

```typescript
type Coordinates = [
  latitude: number,
  longitude: number
];

const location: Coordinates = [
  23.8103,
  90.4125,
];
```

You can also define tuples with optional and rest elements.

```typescript
type User = [
  id: number,
  name: string,
  age?: number,
  ...roles: string[]
];

const user: User = [
  1,
  "Alice",
  25,
  "admin",
  "editor",
];
```

# 🗁 Type Aliases for Union Types

A union type allows a value to have one of several possible types.

```typescript
type Status = "pending" | "success" | "error";

let status: Status = "pending";

status = "success";

// Type error:
status = "completed";
```

Union aliases are useful for representing a fixed set of allowed values.

You can also combine primitive types:

```typescript
type Identifier = string | number;

let id: Identifier;

id = 101;
id = "USR-101";
```

# 🗁 Type Aliases for Intersection Types

An intersection type combines multiple types into one type.

Use the `&` operator.

```typescript
type Identity = {
  id: number;
  name: string;
};

type Contact = {
  email: string;
  phone: string;
};

type User = Identity & Contact;
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  phone: "1234567890",
};
```

The resulting type requires all properties from both types.

# 🗁 Type Aliases for Function Types

You can define function signatures using type aliases.

```typescript
type Add = (
  first: number,
  second: number
) => number;

const add: Add = (a, b) => {
  return a + b;
};

console.log(add(10, 20));
```

The alias describes the function's parameter and return types.

It does not create a function implementation.

# 🗁 Type Aliases for Function Properties

You can also use aliases inside object types.

```typescript
type EventHandler = (
  eventName: string
) => void;

type Event = {
  name: string;
  handler: EventHandler;
};

const event: Event = {
  name: "user-created",

  handler: (eventName) => {
    console.log(eventName);
  },
};
```

This is useful when the same function signature appears in multiple places.

# 🗁 Type Aliases with Generic Types

Type aliases can declare type parameters.

```typescript
type Response<T> = {
  data: T;
  success: boolean;
};

type User = {
  id: number;
  name: string;
};

const response: Response<User> = {
  data: {
    id: 1,
    name: "Alice",
  },
  success: true,
};
```

The generic parameter `T` allows the same structure to work with different data types.

Another example:

```typescript
type Result<T> = {
  value: T;
  error?: string;
};

const numberResult: Result<number> = {
  value: 100,
};

const stringResult: Result<string> = {
  value: "Completed",
};
```

# 🗁 Type Aliases with Generic Constraints

Generic type parameters can have constraints.

```typescript
type Entity<T extends { id: number }> = {
  data: T;
  createdAt: Date;
};
```

The constraint ensures that `T` has an `id` property whose type is `number`.

Example:

```typescript
type User = {
  id: number;
  name: string;
};

const entity: Entity<User> = {
  data: {
    id: 1,
    name: "Alice",
  },
  createdAt: new Date(),
};
```

# 🗁 Type Aliases with Conditional Types

Type aliases can define conditional types using the `extends` keyword.

```typescript
type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false
```

Conditional types allow TypeScript to select a type based on another type.

They are useful for advanced type transformations.

# 🗁 Recursive Type Aliases

A recursive type alias references itself to describe nested or recursive structures.

```typescript
type TreeNode = {
  value: string;
  children?: TreeNode[];
};
```

Example:

```typescript
const tree: TreeNode = {
  value: "Root",
  children: [
    {
      value: "Child A",
    },
    {
      value: "Child B",
      children: [
        {
          value: "Grandchild",
        },
      ],
    },
  ],
};
```

Recursive aliases are useful for:

* Tree structures.
* Nested configuration objects.
* Linked lists.
* Recursive JSON-like data.

# 🗁 Type Aliases with Utility Types

TypeScript provides built-in utility types that can transform existing types.

These utilities work with type aliases.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};
```

Create a type where all properties are optional:

```typescript
type PartialUser = Partial<User>;
```

Create a type with only selected properties:

```typescript
type UserPreview = Pick<User, "id" | "name">;
```

Exclude a property:

```typescript
type UserWithoutEmail = Omit<User, "email">;
```

These utilities reduce duplication and keep related types consistent.

# 🗁 Type Aliases vs Interfaces

Type aliases and interfaces both describe object structures, but they have different capabilities.

Type alias:

```typescript
type User = {
  id: number;
  name: string;
};
```

Interface:

```typescript
interface User {
  id: number;
  name: string;
}
```

Comparison:

| Feature | Type Alias | Interface |
| --- | --- | --- |
| Describe object structures | Yes | Yes |
| Describe primitive aliases | Yes | No |
| Describe union types | Yes | No |
| Describe intersection types | Yes | Through extension or composition |
| Describe tuples | Yes | Not directly as tuple declarations |
| Generic types | Yes | Yes |
| Extend or combine structures | With intersections | With `extends` |
| Declaration merging | No | Yes |
| Conditional types | Yes | No |

A type alias cannot be reopened through another declaration with the same name.

An interface can be declared multiple times in the same declaration scope, and compatible declarations are merged.

# 🗁 Type Alias Declaration Rules

A type alias must have a unique name within its declaration scope.

Incorrect:

```typescript
type User = string;

type User = number;
```

TypeScript reports a duplicate identifier error.

You can create another alias using a different name:

```typescript
type Username = string;
type UserId = number;
```

You can also create a new type from an existing alias:

```typescript
type UserId = number;
type AdminId = UserId;
```

# 🗁 Type Aliases and Structural Typing

Type aliases do not create nominally distinct types.

Two object types with compatible structures are generally assignable to each other.

```typescript
type User = {
  id: number;
  name: string;
};

type Customer = {
  id: number;
  name: string;
};

const user: User = {
  id: 1,
  name: "Alice",
};

const customer: Customer = user;
```

Even though the aliases have different names, their structures are compatible.

If you need stronger distinction between domain values, consider branded types.

# 🗁 Practical Example: E-Commerce Application

Type aliases help organize the data structures of an application.

```typescript
type ProductId = number;

type ProductStatus =
  | "available"
  | "out-of-stock"
  | "discontinued";

type Product = {
  id: ProductId;
  name: string;
  price: number;
  status: ProductStatus;
  tags: string[];
};

type CartItem = {
  product: Product;
  quantity: number;
};

type ShoppingCart = {
  id: string;
  items: CartItem[];
  total: number;
};
```

Example:

```typescript
const cart: ShoppingCart = {
  id: "CART-001",
  items: [
    {
      product: {
        id: 101,
        name: "Keyboard",
        price: 75,
        status: "available",
        tags: ["computer", "accessory"],
      },
      quantity: 2,
    },
  ],
  total: 150,
};
```

Each alias represents a meaningful part of the application domain.

# 🗁 Best Practices

* Use descriptive names that communicate the purpose of a type.
* Prefer reusable aliases for complex or repeated structures.
* Use union aliases for a fixed set of allowed values.
* Use intersection types to combine compatible structures.
* Use generic aliases for reusable data structures.
* Use utility types to transform existing types instead of duplicating properties.
* Prefer interfaces when designing extensible object contracts that benefit from declaration merging.
* Prefer type aliases when working with unions, tuples, conditional types, or other type expressions.
* Avoid excessively complex aliases that are difficult to understand.
* Remember that aliases do not create new runtime values or nominally distinct types.

# 🗁 Summary

* Type aliases are declared using the `type` keyword.
* They assign descriptive names to existing or complex types.
* They can represent primitives, objects, arrays, tuples, unions, and intersections.
* They support generic parameters and constraints.
* They can describe conditional and recursive types.
* They work with TypeScript utility types.
* They do not create new runtime types.
* Unlike interfaces, type aliases cannot participate in declaration merging.

# 🗁 Related Topics

* Object Types
* Interfaces
* Interface Extension
* Interface Merging
* Union Types
* Intersection Types
* Generic Types
* Conditional Types
* Recursive Types
* Utility Types
* Structural Typing
