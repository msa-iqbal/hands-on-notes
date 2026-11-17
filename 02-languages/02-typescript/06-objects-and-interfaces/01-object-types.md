# Object Types

Object types describe the structure of objects in TypeScript by defining their properties and the types of values those properties can contain.

TypeScript uses structural typing, meaning an object is compatible with a type when it has the required structure.

Object types are useful when:

* Defining the structure of application data.
* Describing API request and response objects.
* Validating function arguments at compile time.
* Modeling users, products, configurations, and other entities.
* Ensuring objects contain properties with the expected types.

# 🗁 Basic Syntax

You can define an object type using an object type literal.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};
```

This type describes an object with three required properties:

* `id` must be a number.
* `name` must be a string.
* `email` must be a string.

Create an object using this type:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};
```

TypeScript checks the object against the declared structure.

# 🗁 Inline Object Types

You do not always need a named type alias. You can define an object type directly where it is needed.

```typescript
const user: {
  id: number;
  name: string;
  email: string;
} = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};
```

Inline object types are useful for small, local structures.

For reusable structures, prefer a named type alias or interface.

# 🗁 Required Properties

By default, object properties are required.

```typescript
type Product = {
  id: number;
  name: string;
  price: number;
};

const product: Product = {
  id: 101,
  name: "Keyboard",
  price: 75,
};
```

Missing a required property causes a type error.

```typescript
const product: Product = {
  id: 101,
  name: "Keyboard",
  // Error: price is missing.
};
```

# 🗁 Property Types

Object properties can use different TypeScript types.

```typescript
type User = {
  id: number;
  name: string;
  active: boolean;
  email: string;
  createdAt: Date;
};
```

You can also use arrays, tuples, unions, and other object types as property types.

```typescript
type User = {
  id: number;
  name: string;
  roles: string[];
  coordinates: [number, number];
  status: "active" | "inactive";
};
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  roles: ["admin", "editor"],
  coordinates: [23.8103, 90.4125],
  status: "active",
};
```

# 🗁 Nested Object Types

An object property can itself contain another object type.

```typescript
type Address = {
  city: string;
  country: string;
  postalCode: string;
};

type User = {
  id: number;
  name: string;
  address: Address;
};
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  address: {
    city: "Dhaka",
    country: "Bangladesh",
    postalCode: "1000",
  },
};
```

Nested types help organize complex data structures into reusable components.

You can also define nested object types inline:

```typescript
type User = {
  id: number;
  name: string;
  address: {
    city: string;
    country: string;
  };
};
```

# 🗁 Optional Properties

Use the `?` modifier to declare a property as optional.

```typescript
type User = {
  id: number;
  name: string;
  email?: string;
};
```

Both objects are valid:

```typescript
const user1: User = {
  id: 1,
  name: "Alice",
};

const user2: User = {
  id: 2,
  name: "Bob",
  email: "bob@example.com",
};
```

An optional property may be absent.

When accessed, its type includes `undefined`.

```typescript
const email = user1.email;

// string | undefined
```

Check the value before using it:

```typescript
if (user1.email !== undefined) {
  console.log(user1.email.toLowerCase());
}
```

# 🗁 Readonly Properties

Use the `readonly` modifier to prevent a property from being reassigned after object creation.

```typescript
type User = {
  readonly id: number;
  name: string;
};

const user: User = {
  id: 1,
  name: "Alice",
};

user.name = "Bob"; // Valid

// Type error:
user.id = 2;
```

Readonly is a compile-time restriction. It does not automatically freeze the object at runtime.

# 🗁 Function Properties

Object types can contain function-valued properties.

```typescript
type User = {
  id: number;
  name: string;
  greet: () => void;
};
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  greet: () => {
    console.log("Hello!");
  },
};

user.greet();
```

You can also describe function parameters and return values.

```typescript
type Calculator = {
  add: (a: number, b: number) => number;
  subtract: (a: number, b: number) => number;
};

const calculator: Calculator = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
};
```

# 🗁 Method Signatures

Object types can describe methods using method signature syntax.

```typescript
type User = {
  id: number;
  name: string;
  greet(): string;
};
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",

  greet() {
    return `Hello, ${this.name}!`;
  },
};

console.log(user.greet());
```

Method signatures and function-valued properties are related, but they have different syntax and can differ in some type-system behaviors, such as parameter variance.

# 🗁 Index Signatures

Index signatures allow an object to contain properties with keys described by a particular key type.

```typescript
type Scores = {
  [subject: string]: number;
};
```

Example:

```typescript
const scores: Scores = {
  mathematics: 95,
  physics: 88,
  chemistry: 91,
};
```

The index signature requires values associated with string keys to be numbers.

You can also combine explicitly declared properties with an index signature, provided their types are compatible.

```typescript
type User = {
  id: number;
  name: string;
  [key: string]: string | number;
};
```

Example:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  age: 25,
  active: 1,
};
```

# 🗁 Union Types in Object Properties

Object properties can accept multiple types using union types.

```typescript
type User = {
  id: number;
  name: string;
  age: number | string;
};
```

Examples:

```typescript
const user1: User = {
  id: 1,
  name: "Alice",
  age: 25,
};

const user2: User = {
  id: 2,
  name: "Bob",
  age: "Twenty-five",
};
```

When accessing a union-typed property, narrow its type before using type-specific operations.

```typescript
function printAge(user: User): void {
  if (typeof user.age === "number") {
    console.log(user.age.toFixed(0));
  } else {
    console.log(user.age.toUpperCase());
  }
}
```

# 🗁 Object Types in Function Parameters

You can use object types to define the structure of function arguments.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

function printUser(user: User): void {
  console.log(user.id);
  console.log(user.name);
  console.log(user.email);
}

printUser({
  id: 1,
  name: "Alice",
  email: "alice@example.com",
});
```

This ensures that callers provide an object with the expected structure.

You can also define the object type inline:

```typescript
function printUser(user: {
  id: number;
  name: string;
}): void {
  console.log(user.name);
}
```

# 🗁 Object Types as Return Types

Functions can return objects described by object types.

```typescript
type Product = {
  id: number;
  name: string;
  price: number;
};

function createProduct(): Product {
  return {
    id: 101,
    name: "Keyboard",
    price: 75,
  };
}

const product = createProduct();
```

TypeScript checks that the returned object satisfies the declared return type.

# 🗁 Excess Property Checks

When an object literal is assigned directly to a variable with an object type, TypeScript performs an excess property check.

```typescript
type User = {
  id: number;
  name: string;
};

// Error: unexpected property 'age'.
const user: User = {
  id: 1,
  name: "Alice",
  age: 25,
};
```

However, TypeScript uses structural typing. An existing variable with additional properties can still be assignable if it contains the required compatible properties.

```typescript
const userData = {
  id: 1,
  name: "Alice",
  age: 25,
};

const user: User = userData; // Valid
```

The important distinction is that excess property checks are applied to fresh object literals. They are not a general rule that rejects every object with additional properties.

# 🗁 Empty Object Types

The type `{}` does not mean an object with no properties.

In TypeScript, `{}` accepts any non-nullish value, including strings, numbers, booleans, and objects.

```typescript
let value: {};

value = "Hello";
value = 100;
value = true;
value = { name: "Alice" };

// Error under strict null checking:
value = null;
value = undefined;
```

If you need a value that must be an object, consider using `object`.

```typescript
let value: object;

value = { name: "Alice" };
value = [1, 2, 3];
value = () => {};

// Error:
value = "Hello";
value = 100;
```

The `object` type excludes primitive values but does not describe a particular property structure.

# 🗁 Object Type vs object Type

These forms have different meanings:

| Type | Meaning |
| --- | --- |
| `{}` | Any non-nullish value |
| `object` | Any non-primitive value |
| `{ id: number }` | An object with a required numeric `id` property |

Examples:

```typescript
let a: {} = "Hello";

let b: object = {
  name: "Alice",
};

let c: { id: number } = {
  id: 1,
};
```

Use a specific object type whenever you know the expected structure.

# 🗁 Practical Example: API Response

Object types are frequently used to describe API responses.

```typescript
type ApiUser = {
  id: number;
  name: string;
  email: string;
  address: {
    city: string;
    country: string;
  };
  roles: string[];
  active: boolean;
};

const response: ApiUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  address: {
    city: "Dhaka",
    country: "Bangladesh",
  },
  roles: ["admin", "editor"],
  active: true,
};
```

This provides compile-time checking for the expected response structure.

It does not validate external JSON at runtime. Runtime validation requires an appropriate validation strategy.

# 🗁 Best Practices

* Define reusable object structures using named type aliases or interfaces.
* Use meaningful property names.
* Prefer specific property types over `any`.
* Use optional properties only when a property may genuinely be absent.
* Use `readonly` for properties that should not be reassigned.
* Break large nested structures into smaller reusable types.
* Use index signatures only when dynamic keys are part of the data model.
* Prefer specific object types over `{}` or `object` when the structure is known.
* Use runtime validation for data received from APIs or other untrusted sources.
* Choose objects rather than tuples when named property access improves readability.

# 🗁 Summary

* Object types describe the structure of objects.
* Properties have names and associated types.
* Properties are required by default.
* Optional properties use the `?` modifier.
* Readonly properties use the `readonly` modifier.
* Object types support nested objects, arrays, tuples, unions, and functions.
* Index signatures describe objects with dynamic property keys.
* TypeScript uses structural typing to determine object compatibility.
* Excess property checks help catch unexpected properties in fresh object literals.
* Runtime validation is separate from compile-time type checking.

# 🗁 Related Topics

* Type Aliases
* Interfaces
* Interface Extension
* Optional Properties
* Readonly Properties
* Index Signatures
* Function Types
* Union Types
* Intersection Types
* Structural Typing
