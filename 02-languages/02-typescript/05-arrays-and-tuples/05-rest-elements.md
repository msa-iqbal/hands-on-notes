# Rest Elements

Rest elements allow a tuple to contain zero or more additional elements after its explicitly declared elements.

TypeScript uses the rest syntax (`...`) to define rest elements in tuple types.

Rest elements are useful when:

* A tuple has a fixed beginning followed by a variable number of values.
* A function accepts a variable number of arguments.
* You need to describe function parameters precisely.
* A data structure combines required elements with a variable-length collection.

# 🗁 Basic Syntax

A rest element is declared using three dots (`...`) followed by an array type.

```typescript
type NumberTuple = [
  first: number,
  ...rest: number[]
];
```

This tuple contains:

* One required `number` element.
* Zero or more additional `number` elements.

Valid assignments:

```typescript
const values1: NumberTuple = [10];

const values2: NumberTuple = [10, 20];

const values3: NumberTuple = [10, 20, 30, 40];
```

The first element is required, but the rest elements are optional in quantity.

# 🗁 Rest Elements with Different Types

A tuple can have a rest element whose type differs from its preceding elements.

```typescript
type User = [
  id: number,
  name: string,
  ...roles: string[]
];
```

Examples:

```typescript
const user1: User = [1, "Alice"];

const user2: User = [
  2,
  "Bob",
  "admin",
];

const user3: User = [
  3,
  "Charlie",
  "editor",
  "moderator",
];
```

The tuple always starts with a number and a string. Additional elements must be strings.

# 🗁 Rest Elements with Union Types

A rest element can accept multiple types through a union.

```typescript
type Values = [
  id: number,
  ...items: (string | number)[]
];
```

Valid assignments:

```typescript
const values1: Values = [1];

const values2: Values = [
  2,
  "Alice",
  100,
];

const values3: Values = [
  3,
  "Bob",
  200,
  "Developer",
];
```

Every element after the first must be either a string or a number.

# 🗁 Rest Elements with Optional Elements

Rest elements can follow optional tuple elements.

```typescript
type User = [
  id: number,
  name: string,
  age?: number,
  ...tags: string[]
];
```

Examples:

```typescript
const user1: User = [1, "Alice"];

const user2: User = [
  2,
  "Bob",
  25,
];

const user3: User = [
  3,
  "Charlie",
  30,
  "developer",
  "typescript",
];
```

However, remember that positional values cannot skip an optional position automatically.

To provide tags without an age, use `undefined` as the third element:

```typescript
const user: User = [
  4,
  "David",
  undefined,
  "designer",
  "frontend",
];
```

# 🗁 Rest Elements with Fixed Elements After Them

TypeScript supports tuple types with rest elements followed by additional required elements.

```typescript
type Data = [
  ...names: string[],
  enabled: boolean
];
```

Examples:

```typescript
const data1: Data = [true];

const data2: Data = [
  "Alice",
  true,
];

const data3: Data = [
  "Alice",
  "Bob",
  "Charlie",
  false,
];
```

The final element must always be a boolean. All preceding elements must be strings.

This is useful when a tuple has a variable-length section followed by a required value.

# 🗁 Multiple Rest Elements

A tuple cannot contain multiple unbounded rest elements.

Incorrect:

```typescript
type Invalid = [
  ...numbers: number[],
  ...strings: string[]
];
```

TypeScript cannot determine where the first unbounded section ends and the second begins.

However, multiple rest-like sections can be represented when their lengths are constrained.

```typescript
type Valid = [
  ...numbers: [number, number],
  ...strings: string[]
];
```

Here, the first section always contains exactly two numbers, followed by zero or more strings.

# 🗁 Rest Elements with Readonly Tuples

Rest elements can also be used in readonly tuples.

```typescript
type User = readonly [
  id: number,
  name: string,
  ...roles: string[]
];

const user: User = [
  1,
  "Alice",
  "admin",
  "editor",
];

// Type error:
user[0] = 2;
```

The readonly modifier prevents modifications through the tuple reference.

# 🗁 Rest Elements in Function Parameters

One of the most common applications of tuple rest elements is defining functions with variable-length arguments.

```typescript
type LogArguments = [
  message: string,
  ...values: unknown[]
];

function log(...args: LogArguments): void {
  const [message, ...values] = args;

  console.log(message, ...values);
}

log("Application started");

log("User details", {
  id: 1,
  name: "Alice",
});

log("Request completed", 200, "OK");
```

The first argument must be a string. Any additional arguments can have any value because their type is `unknown`.

# 🗁 Function Parameters from Tuple Types

Tuple types can describe a function's complete parameter list.

```typescript
type Parameters = [
  name: string,
  age: number,
  ...skills: string[]
];

function createUser(
  ...args: Parameters
): void {
  const [name, age, ...skills] = args;

  console.log({
    name,
    age,
    skills,
  });
}

createUser(
  "Alice",
  25,
  "JavaScript",
  "TypeScript",
  "React",
);
```

This approach provides strict type checking while allowing a variable number of arguments.

# 🗁 Spreading Tuples into Another Tuple

You can use tuple spread syntax to combine existing tuple types.

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

The resulting `User` type is equivalent to:

```typescript
type User = [
  id: number,
  name: string,
  email: string,
  phone: string
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

# 🗁 Spreading Tuples with Rest Elements

Tuple types can also combine fixed elements and variable-length sections.

```typescript
type Prefix = [
  id: number,
  name: string
];

type Suffix = [
  ...tags: string[]
];

type User = [
  ...Prefix,
  ...Suffix
];
```

Example:

```typescript
const user: User = [
  1,
  "Alice",
  "developer",
  "typescript",
];
```

This allows reusable tuple structures to be composed from smaller types.

# 🗁 Practical Example: Command-Line Arguments

A command-line command may require a command name followed by a variable number of arguments.

```typescript
type Command = [
  command: string,
  ...args: string[]
];

function executeCommand(...command: Command): void {
  const [name, ...args] = command;

  console.log("Command:", name);
  console.log("Arguments:", args);
}

executeCommand("install");

executeCommand("install", "typescript");

executeCommand(
  "create",
  "project",
  "--template",
  "react",
);
```

The first element identifies the command, while the remaining elements represent its arguments.

# 🗁 Practical Example: Event Handler

An event handler can accept a required event name followed by additional payload values.

```typescript
type EventArguments = [
  eventName: string,
  ...payload: unknown[]
];

function emit(...args: EventArguments): void {
  const [eventName, ...payload] = args;

  console.log({
    eventName,
    payload,
  });
}

emit("user:created", {
  id: 1,
  name: "Alice",
});

emit("order:completed", 1001, 250);
```

For production event systems, a discriminated union or a typed event map may provide stronger relationships between event names and payload types.

# 🗁 Common Mistakes

**Using an incorrect rest element type**

Incorrect:

```typescript
type User = [
  id: number,
  ...roles: string
];
```

A tuple rest element must use an array or tuple type.

Correct:

```typescript
type User = [
  id: number,
  ...roles: string[]
];
```

**Assuming a rest element requires at least one value**

```typescript
type Values = [
  id: number,
  ...numbers: number[]
];

const values: Values = [1];
```

An array rest element can contain zero or more values.

**Forgetting the required tuple elements**

```typescript
type User = [
  id: number,
  name: string,
  ...roles: string[]
];

// Type error:
const user: User = [1];

// Correct:
const validUser: User = [1, "Alice"];
```

**Using incompatible values in the rest section**

```typescript
type User = [
  id: number,
  ...roles: string[]
];

// Type error:
const user: User = [
  1,
  "admin",
  100,
];
```

The rest section only accepts strings.

# 🗁 Best Practices

* Use rest elements when a tuple has a variable-length section.
* Use named tuple elements to make positional structures easier to understand.
* Choose precise rest element types instead of `any[]`.
* Use `unknown[]` when additional values are unrestricted and must be narrowed before use.
* Use readonly tuples when tuple values should not be modified.
* Extract reusable tuple structures into named type aliases.
* Prefer object types when positional arguments become difficult to understand.
* Use discriminated unions for complex data structures with different possible shapes.

# 🗁 Summary

* Rest elements use the `...` syntax in tuple types.
* A rest element represents zero or more values of a specified type.
* Rest elements can follow required or optional tuple elements.
* Fixed tuple elements can follow a rest element.
* Multiple unbounded rest elements cannot be declared in one tuple.
* Tuple types can be composed using spread syntax.
* Rest elements are especially useful for variable-length function parameters.

# 🗁 Related Topics

* Arrays
* Readonly Arrays
* Tuples
* Optional Tuple Elements
* Named Tuples
* Rest Parameters
* Function Types
* Generic Types
