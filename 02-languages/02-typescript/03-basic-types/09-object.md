# TypeScript Object Type

The `object` type in TypeScript represents non-primitive values.

It allows values such as objects, arrays, and functions, but does not allow primitive values such as strings, numbers, booleans, `null`, or `undefined`.

Understanding the `object` type is important when working with structured data, object properties, function parameters, and TypeScript's type system.

# 🗁 Understanding Object

The `object` type represents values that are not primitive types.

```typescript
let user: object;

user = {
    name: "John",
    age: 25
};

user = [10, 20, 30];

user = new Date();

user = () => {
    console.log("Hello");
};
```

All these assignments are valid because objects, arrays, dates, and functions are non-primitive values.

# 🗁 Primitive vs Non-Primitive Types

JavaScript values are commonly divided into primitive and non-primitive categories.

**Primitive types:**

* `string`
* `number`
* `boolean`
* `bigint`
* `symbol`
* `null`
* `undefined`

**Non-primitive values:**

* Objects
* Arrays
* Functions
* Dates
* Maps
* Sets
* Other object instances

The TypeScript `object` type accepts non-primitive values.

```typescript
let value: object;

// Valid
value = { name: "John" };
value = [10, 20];
value = new Date();

// Invalid
// value = "Hello";
// value = 100;
// value = true;
```

# 🗁 Declaring Object Variables

You can explicitly declare a variable using the `object` type.

```typescript
let user: object = {
    name: "John",
    age: 25
};
```

However, the `object` type does not describe the structure of the object.

For example:

```typescript
let user: object = {
    name: "John",
    age: 25
};

// TypeScript error:
// console.log(user.name);
```

TypeScript knows that `user` is an object, but it does not know that the object has a `name` property.

To access properties safely, define a more specific object type.

```typescript
let user: {
    name: string;
    age: number;
} = {
    name: "John",
    age: 25
};

console.log(user.name);
console.log(user.age);
```

# 🗁 Object Type Annotations

You can describe an object's structure directly using an object type annotation.

```typescript
let user: {
    name: string;
    age: number;
    isActive: boolean;
};

user = {
    name: "John",
    age: 25,
    isActive: true
};
```

The annotation defines the expected properties and their types.

If a required property is missing or has an incompatible type, TypeScript reports an error.

```typescript
let user: {
    name: string;
    age: number;
};

// Valid
user = {
    name: "John",
    age: 25
};

// Invalid: age must be a number.
// user = {
//     name: "John",
//     age: "25"
// };
```

# 🗁 Optional Object Properties

You can use the optional property modifier (`?`) to make an object property optional.

```typescript
let user: {
    name: string;
    age?: number;
};

user = {
    name: "John"
};

user = {
    name: "Jane",
    age: 25
};
```

The `age` property can be omitted.

When accessing an optional property, TypeScript considers that its value may be `undefined`.

```typescript
const user: {
    name: string;
    age?: number;
} = {
    name: "John"
};

console.log(user.age); // undefined
```

# 🗁 Readonly Object Properties

The `readonly` modifier prevents a property from being reassigned after initialization.

```typescript
let user: {
    readonly id: number;
    name: string;
};

user = {
    id: 101,
    name: "John"
};

user.name = "David";

// TypeScript error:
// user.id = 102;
```

The `readonly` modifier provides compile-time protection against reassignment.

It does not automatically make nested objects or their properties deeply immutable.

# 🗁 Object Type with Functions

Object types can contain function properties.

```typescript
let user: {
    name: string;
    greet: () => void;
};

user = {
    name: "John",

    greet() {
        console.log(`Hello, ${this.name}`);
    }
};

user.greet();
```

You can also define methods that accept parameters and return values.

```typescript
let calculator: {
    add: (a: number, b: number) => number;
    subtract: (a: number, b: number) => number;
};

calculator = {
    add: (a, b) => a + b,
    subtract: (a, b) => a - b
};

console.log(calculator.add(10, 5));
console.log(calculator.subtract(10, 5));
```

# 🗁 Object Type with Nested Objects

An object type can describe nested object structures.

```typescript
let user: {
    name: string;
    address: {
        city: string;
        country: string;
    };
};

user = {
    name: "John",
    address: {
        city: "Dhaka",
        country: "Bangladesh"
    }
};

console.log(user.address.city);
```

Nested object types help define structured application data.

# 🗁 Object Type with Arrays

Arrays are objects in JavaScript, so they are assignable to the TypeScript `object` type.

```typescript
let values: object;

values = [10, 20, 30];
```

However, if you want to describe an array containing specific values, use an array type.

```typescript
let numbers: number[] = [10, 20, 30];

let usernames: string[] = [
    "John",
    "Jane",
    "David"
];
```

For arrays containing structured objects:

```typescript
let users: {
    id: number;
    name: string;
}[] = [
    {
        id: 1,
        name: "John"
    },
    {
        id: 2,
        name: "Jane"
    }
];
```

# 🗁 Object Type with Index Signatures

An index signature allows an object to contain properties whose names follow a specified key type.

```typescript
let user: {
    [key: string]: string;
};

user = {
    name: "John",
    email: "john@example.com",
    city: "Dhaka"
};
```

All properties must have string values.

You can also define index signatures with other value types.

```typescript
let scores: {
    [key: string]: number;
};

scores = {
    mathematics: 90,
    physics: 85,
    chemistry: 88
};
```

Index signatures are useful for objects with dynamic property names.

# 🗁 Object Type with Union Values

An object property can accept multiple types using a union type.

```typescript
let user: {
    id: number | string;
    name: string;
    age?: number | null;
};

user = {
    id: 101,
    name: "John",
    age: 25
};

user = {
    id: "USR-102",
    name: "Jane",
    age: null
};
```

Union types provide flexibility while preserving type checking.

# 🗁 Object vs Object Literal Types

The `object` type and an object literal type serve different purposes.

**Using object:**

```typescript
let user: object = {
    name: "John",
    age: 25
};
```

TypeScript knows only that `user` is a non-primitive value.

**Using an object literal type:**

```typescript
let user: {
    name: string;
    age: number;
} = {
    name: "John",
    age: 25
};

console.log(user.name);
console.log(user.age);
```

TypeScript knows the exact structure of the object.

Comparison:

| Feature | `object` | Object literal type |
| --- | --- | --- |
| Accepts non-primitive values | Yes | Depends on defined structure |
| Describes properties | No | Yes |
| Supports optional properties | No | Yes |
| Supports readonly properties | No | Yes |
| Provides property type checking | No | Yes |
| Useful for structured data | Limited | Yes |

# 🗁 Object vs Empty Object Type

TypeScript also provides the `{}` type, which is different from `object`.

The `{}` type accepts any non-nullish value, including primitives.

```typescript
let value: {};

value = "Hello";
value = 100;
value = true;
value = [];
value = {};
```

However, `null` and `undefined` are not assignable to `{}` when `strictNullChecks` is enabled.

Comparison:

| Type | Accepted values |
| --- | --- |
| `object` | Non-primitive values |
| `{}` | Any non-nullish value |
| `unknown` | Any value |
| `any` | Any value without type checking |

In most application code, prefer a specific object type instead of using `{}` when describing data structures.

# 🗁 Object vs Record

The `Record` utility type provides another way to describe objects.

```typescript
type User = Record<string, string>;

const user: User = {
    name: "John",
    city: "Dhaka",
    country: "Bangladesh"
};
```

This creates an object type whose string keys map to string values.

You can also use a union of specific keys.

```typescript
type Role = "admin" | "editor" | "viewer";

type Permissions = Record<Role, boolean>;

const permissions: Permissions = {
    admin: true,
    editor: true,
    viewer: false
};
```

`Record` is particularly useful when creating dictionaries, lookup tables, and configuration objects.

# 🗁 Passing Objects to Functions

You can define an object type directly in a function parameter.

```typescript
function displayUser(user: {
    name: string;
    age: number;
}): void {
    console.log(user.name);
    console.log(user.age);
}

displayUser({
    name: "John",
    age: 25
});
```

For reusable structures, use a type alias or interface.

```typescript
interface User {
    name: string;
    age: number;
}

function displayUser(user: User): void {
    console.log(`${user.name} is ${user.age} years old`);
}

displayUser({
    name: "John",
    age: 25
});
```

This makes function signatures easier to read and maintain.

# 🗁 Object Type with Type Aliases

Type aliases allow you to give a reusable name to an object structure.

```typescript
type User = {
    id: number;
    name: string;
    email: string;
};

const user: User = {
    id: 1,
    name: "John",
    email: "john@example.com"
};
```

You can reuse the same type throughout your application.

```typescript
function getUser(): User {
    return {
        id: 1,
        name: "John",
        email: "john@example.com"
    };
}

function displayUser(user: User): void {
    console.log(user.name);
}
```

# 🗁 Object Type with Interfaces

Interfaces provide another way to define object structures.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
}

const user: User = {
    id: 1,
    name: "John",
    email: "john@example.com"
};
```

Interfaces are especially useful for describing object contracts that can be extended or implemented by classes.

# 🗁 Best Practices

* Prefer specific object types over the generic `object` type.
* Use interfaces or type aliases for reusable object structures.
* Use optional properties when a property may be omitted.
* Use `readonly` for properties that should not be reassigned.
* Use nested object types to describe complex data structures.
* Use index signatures for objects with dynamic property names.
* Use `Record` for dictionary-like structures.
* Avoid confusing `object`, `{}`, and `Object`.
* Validate external data before trusting its structure.
* Keep object definitions clear and maintainable.

# 🗁 Summary

The TypeScript `object` type represents non-primitive values, including objects, arrays, and functions.

Important concepts include:

* Understanding primitive and non-primitive values.
* Declaring object variables.
* Object literal type annotations.
* Optional and readonly properties.
* Function properties and methods.
* Nested object structures.
* Arrays and index signatures.
* Union values in object properties.
* Differences between `object`, `{}`, and `unknown`.
* Using `Record`, type aliases, and interfaces.
* Passing structured objects to functions.

The key principle is to use specific object types whenever you need to describe an object's structure. The generic `object` type only identifies a value as non-primitive; it does not describe or validate its properties.
