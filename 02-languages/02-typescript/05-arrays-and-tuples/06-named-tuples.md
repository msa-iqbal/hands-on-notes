# Named Tuples

Named tuples are tuple types whose elements have descriptive names.

TypeScript allows you to assign names to tuple elements to make their purpose easier to understand.

Named tuples do not change how tuples work at runtime. They improve readability, documentation, and editor support.

Named tuples are useful when:

* Tuple positions represent specific values.
* Function parameters are described using tuple types.
* You want better autocomplete and editor hints.
* A tuple contains several values of the same or different types.
* You want to make positional data easier to understand.

# 🗁 Basic Syntax

You can give each tuple element a name followed by its type.

```typescript
type User = [
  id: number,
  name: string,
  email: string
];
```

Here:

* `id` represents the user's identifier.
* `name` represents the user's name.
* `email` represents the user's email address.

Create values using the same positional structure:

```typescript
const user: User = [
  1,
  "Alice",
  "alice@example.com",
];
```

The element names are descriptive labels. They do not become object properties.

```typescript
console.log(user[0]); // 1
console.log(user[1]); // Alice
console.log(user[2]); // alice@example.com
```

You cannot access tuple elements using their declared names:

```typescript
// Type error:
console.log(user.id);
```

# 🗁 Named Tuples Without Labels

A tuple can be declared without element names.

```typescript
type User = [
  number,
  string,
  string
];
```

This is valid, but it is less descriptive.

Compare:

```typescript
type UserWithoutNames = [
  number,
  string,
  string
];

type UserWithNames = [
  id: number,
  name: string,
  email: string
];
```

Both types represent the same positional structure, but named tuples communicate the meaning of each position.

# 🗁 Named Tuples with Optional Elements

Named tuple elements can be optional.

Use `?` after the element name.

```typescript
type User = [
  id: number,
  name: string,
  age?: number
];
```

Examples:

```typescript
const user1: User = [
  1,
  "Alice",
];

const user2: User = [
  2,
  "Bob",
  25,
];
```

The optional element may be omitted.

# 🗁 Named Tuples with Rest Elements

Named tuples also support rest elements.

```typescript
type User = [
  id: number,
  name: string,
  ...roles: string[]
];
```

Examples:

```typescript
const user1: User = [
  1,
  "Alice",
];

const user2: User = [
  2,
  "Bob",
  "admin",
  "editor",
];
```

The `roles` label describes the variable-length section.

# 🗁 Named Tuples with Readonly

You can create readonly named tuples using the `readonly` modifier.

```typescript
type User = readonly [
  id: number,
  name: string,
  age?: number
];

const user: User = [
  1,
  "Alice",
  25,
];

// Type error:
user[0] = 2;
```

Readonly prevents modifying tuple elements through that reference.

# 🗁 Named Tuples in Function Parameters

Named tuples are especially useful when defining function parameter lists.

```typescript
type CreateUserArguments = [
  name: string,
  email: string,
  age?: number
];

function createUser(
  ...args: CreateUserArguments
): void {
  const [name, email, age] = args;

  console.log({
    name,
    email,
    age,
  });
}

createUser(
  "Alice",
  "alice@example.com",
);

createUser(
  "Bob",
  "bob@example.com",
  25,
);
```

The names make the parameter structure easier to understand without changing the function's runtime behavior.

# 🗁 Named Tuples with Function Type Aliases

You can use named tuples to describe function parameters through reusable type aliases.

```typescript
type CalculateArguments = [
  first: number,
  second: number
];

type Calculate = (
  ...args: CalculateArguments
) => number;

const add: Calculate = (
  first,
  second
) => {
  return first + second;
};

console.log(add(10, 20)); // 30
```

The parameter names are available in editor hints, making the function signature more descriptive.

# 🗁 Named Tuples with Destructuring

Named tuples can be destructured like ordinary tuples.

```typescript
type Product = [
  id: number,
  name: string,
  price: number
];

const product: Product = [
  101,
  "Keyboard",
  75,
];

const [id, name, price] = product;

console.log(id);
console.log(name);
console.log(price);
```

The tuple labels do not automatically become destructured variable names. You choose the variable names in the destructuring statement.

# 🗁 Named Tuples with Tuple Composition

You can combine named tuple types using spread syntax.

```typescript
type Identity = [
  id: number,
  name: string
];

type Contact = [
  email: string,
  phone: string
];

type User = [
  ...Identity,
  ...Contact
];
```

Example:

```typescript
const user: User = [
  1,
  "Alice",
  "alice@example.com",
  "1234567890",
];
```

Each reusable tuple describes a meaningful part of the resulting structure.

# 🗁 Named Tuples and Function Overloads

Named tuples can be used with function overloads to describe different argument combinations.

```typescript
function createUser(
  ...args: [name: string]
): void;

function createUser(
  ...args: [name: string, age: number]
): void;

function createUser(
  ...args: [name: string, age?: number]
): void {
  const [name, age] = args;

  console.log({
    name,
    age,
  });
}

createUser("Alice");

createUser("Bob", 25);
```

Each overload describes an accepted argument structure.

# 🗁 Named Tuples vs Objects

Named tuples and object types can both represent structured data, but they have different characteristics.

Named tuple:

```typescript
type UserTuple = [
  id: number,
  name: string,
  email: string
];

const userTuple: UserTuple = [
  1,
  "Alice",
  "alice@example.com",
];
```

Object type:

```typescript
type UserObject = {
  id: number;
  name: string;
  email: string;
};

const userObject: UserObject = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};
```

| Feature | Named Tuple | Object |
| --- | --- | --- |
| Access values | By index | By property name |
| Element names | Type-level labels | Actual property names |
| Order matters | Yes | No |
| Destructuring | Supported | Supported |
| Optional values | Supported | Supported |
| Best suited for | Positional data | Structured records |

Prefer objects when data has several named properties or when positional access would make the code difficult to understand.

# 🗁 Practical Example: Coordinates

Coordinates are a good use case for named tuples because their positions have clear meanings.

```typescript
type Coordinates = [
  latitude: number,
  longitude: number
];

const location: Coordinates = [
  23.8103,
  90.4125,
];

const [latitude, longitude] = location;

console.log("Latitude:", latitude);
console.log("Longitude:", longitude);
```

The labels help developers understand which value represents latitude and which represents longitude.

# 🗁 Practical Example: HTTP Response

A simple HTTP response can be represented using a named tuple.

```typescript
type HttpResponse = [
  status: number,
  body: string,
  contentType?: string
];

const response1: HttpResponse = [
  200,
  "Request successful",
];

const response2: HttpResponse = [
  404,
  "Resource not found",
  "text/plain",
];
```

For complex HTTP responses, an object type is generally more maintainable.

# 🗁 Common Mistakes

**Treating tuple labels as object properties**

```typescript
type User = [
  id: number,
  name: string
];

const user: User = [1, "Alice"];

// Type error:
console.log(user.id);
```

Access tuple values using their indexes:

```typescript
console.log(user[0]);
console.log(user[1]);
```

**Changing the order of tuple values**

```typescript
type User = [
  id: number,
  name: string
];

// Type error:
const user: User = [
  "Alice",
  1,
];
```

Named tuples still enforce positional ordering.

**Using different labels in equivalent tuple structures**

```typescript
type UserA = [
  id: number,
  name: string
];

type UserB = [
  userId: number,
  username: string
];
```

The labels do not create distinct runtime or nominal types. Both describe the same underlying positional structure.

**Assuming labels are required for every element**

TypeScript supports tuple types with named elements, but when naming elements, follow consistent naming throughout the tuple declaration.

# 🗁 Best Practices

* Give tuple elements meaningful names.
* Use named tuples when positional values have distinct meanings.
* Keep tuple structures relatively small and understandable.
* Use optional labels for optional tuple elements.
* Use descriptive rest labels for variable-length sections.
* Prefer objects for complex records.
* Use readonly tuples when values should not be modified.
* Avoid assuming tuple labels are available as runtime properties.

# 🗁 Summary

* Named tuples assign descriptive labels to tuple elements.
* Labels improve readability and editor support.
* Tuple values are still accessed by numeric indexes.
* Labels do not create object properties.
* Named tuples support optional and rest elements.
* Named tuples can be readonly.
* They are useful for function parameter definitions and positional data structures.
* Objects are generally preferable for complex named records.

# 🗁 Related Topics

* Arrays
* Readonly Arrays
* Tuples
* Optional Tuple Elements
* Rest Elements
* Function Types
* Function Overloads
* Destructuring
* Object Types
