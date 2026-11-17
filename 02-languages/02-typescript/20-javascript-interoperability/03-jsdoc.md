# JSDoc in TypeScript

JSDoc is a documentation syntax that uses specially formatted comments to describe JavaScript code.

TypeScript understands JSDoc annotations and uses them to provide type information for JavaScript files.

This allows developers to add static type checking, IntelliSense, and documentation without converting JavaScript files into TypeScript.

JSDoc is particularly useful when:

* Maintaining existing JavaScript projects.
* Enabling TypeScript checking in JavaScript files.
* Documenting functions and APIs.
* Defining object structures.
* Describing generic functions.
* Gradually migrating JavaScript to TypeScript.

# 🗁 Basic JSDoc Syntax

JSDoc comments begin with `/**` and end with `*/`.

```javascript
/**
 * Adds two numbers.
 *
 * @param {number} a - First number.
 * @param {number} b - Second number.
 * @returns {number} The calculated sum.
 */
function add(a, b) {
  return a + b;
}
```

Common JSDoc tags:

| Tag | Purpose |
| --- | --- |
| `@param` | Documents a function parameter |
| `@returns` | Documents a return value |
| `@type` | Declares a variable's type |
| `@typedef` | Defines a reusable type |
| `@property` | Documents an object property |
| `@template` | Declares a generic type parameter |
| `@extends` | Documents class inheritance |
| `@implements` | Documents interface implementation |
| `@deprecated` | Marks an API as deprecated |
| `@example` | Provides usage examples |
| `@throws` | Documents possible exceptions |

# 🗁 JSDoc Parameter Types

Use `@param` to describe function parameters.

```javascript
/**
 * Greets a user.
 *
 * @param {string} name
 */
function greet(name) {
  return `Hello, ${name}`;
}

greet("Alice");
```

TypeScript can detect incorrect arguments when JavaScript checking is enabled.

```javascript
// TypeScript reports an error.
greet(123);
```

You can also provide parameter descriptions:

```javascript
/**
 * Calculates the total price.
 *
 * @param {number} price - Original price.
 * @param {number} quantity - Number of items.
 * @returns {number} Total price.
 */
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

# 🗁 JSDoc Return Types

Use `@returns` to describe the function's return type.

```javascript
/**
 * Gets a username.
 *
 * @returns {string}
 */
function getUsername() {
  return "Alice";
}
```

For a function that does not return a meaningful value:

```javascript
/**
 * Logs a message.
 *
 * @returns {void}
 */
function logMessage() {
  console.log("Application started");
}
```

For asynchronous functions, describe the resolved value using `Promise`.

```javascript
/**
 * Retrieves a user.
 *
 * @returns {Promise<string>}
 */
async function getUser() {
  return "Alice";
}
```

# 🗁 JSDoc Variable Types

Use `@type` to explicitly declare a variable's type.

```javascript
/** @type {string} */
let username = "Alice";

/** @type {number} */
let age = 25;

/** @type {boolean} */
let isActive = true;
```

TypeScript checks assignments against the declared types.

```javascript
/** @type {number} */
let score = 100;

// TypeScript reports an error.
score = "high";
```

# 🗁 JSDoc Object Types

Use inline object types to describe object structures.

```javascript
/**
 * @param {{ id: number, name: string }} user
 */
function displayUser(user) {
  console.log(user.id);
  console.log(user.name);
}
```

You can also define optional properties:

```javascript
/**
 * @param {{ id: number, name: string, email?: string }} user
 */
function createUser(user) {
  console.log(user.name);
}
```

The `?` indicates that a property is optional.

# 🗁 Using typedef

Use `@typedef` to define reusable custom types.

```javascript
/**
 * @typedef {Object} User
 * @property {number} id
 * @property {string} name
 * @property {string} email
 */

/**
 * @param {User} user
 */
function displayUser(user) {
  console.log(user.name);
}
```

The `User` type can be reused throughout the same JavaScript file.

For larger projects, shared type definitions can be exported from dedicated declaration or TypeScript files.

# 🗁 Optional and Nullable Types

JSDoc supports optional and nullable type expressions.

**Optional parameter**

```javascript
/**
 * @param {string} name
 * @param {number} [age]
 */
function createUser(name, age) {
  console.log(name, age);
}

createUser("Alice");
createUser("Bob", 25);
```

**Nullable type**

```javascript
/**
 * @param {?string} username
 */
function displayUsername(username) {
  if (username !== null) {
    console.log(username);
  }
}
```

**Nullable or undefined**

```javascript
/**
 * @param {string | null | undefined} value
 */
function processValue(value) {
  console.log(value);
}
```

# 🗁 JSDoc Union Types

Use the union operator `|` to allow multiple types.

```javascript
/**
 * @param {string | number} value
 */
function displayValue(value) {
  console.log(value);
}

displayValue("Hello");
displayValue(100);
```

TypeScript checks that the supplied argument belongs to the declared union.

# 🗁 JSDoc Array Types

Use `Array<Type>` or `Type[]` to describe arrays.

```javascript
/**
 * @param {string[]} names
 */
function displayNames(names) {
  names.forEach((name) => {
    console.log(name);
  });
}
```

Equivalent notation:

```javascript
/**
 * @param {Array<number>} scores
 */
function calculateAverage(scores) {
  return scores.reduce((total, score) => total + score, 0)
    / scores.length;
}
```

# 🗁 JSDoc Function Types

Use function type expressions to describe callback parameters.

```javascript
/**
 * @param {(a: number, b: number) => number} callback
 */
function calculate(callback) {
  return callback(10, 20);
}

const result = calculate((a, b) => a + b);
```

You can also define reusable callback types:

```javascript
/**
 * @callback Formatter
 * @param {string} value
 * @returns {string}
 */

/**
 * @param {Formatter} formatter
 */
function formatName(formatter) {
  return formatter("alice");
}
```

# 🗁 JSDoc Generic Types

Use `@template` to declare generic type parameters.

```javascript
/**
 * Returns the provided value.
 *
 * @template T
 * @param {T} value
 * @returns {T}
 */
function identity(value) {
  return value;
}
```

TypeScript preserves the input type:

```javascript
const name = identity("Alice");
const age = identity(25);
```

The inferred types are `string` and `number`, respectively.

You can also use generic types with arrays:

```javascript
/**
 * Returns the first item.
 *
 * @template T
 * @param {T[]} items
 * @returns {T | undefined}
 */
function first(items) {
  return items[0];
}
```

# 🗁 JSDoc Class Documentation

JSDoc can document JavaScript classes and their properties.

```javascript
/**
 * Represents a user.
 */
class User {
  /**
   * Creates a user.
   *
   * @param {number} id
   * @param {string} name
   */
  constructor(id, name) {
    /** @type {number} */
    this.id = id;

    /** @type {string} */
    this.name = name;
  }

  /**
   * Returns the user's name.
   *
   * @returns {string}
   */
  getName() {
    return this.name;
  }
}
```

This improves type information for class instances and their methods.

# 🗁 JSDoc Type Imports

JavaScript files can reference types declared in TypeScript files.

Consider:

```typescript
// types.ts

export interface User {
  id: number;
  name: string;
}
```

Import the type in JavaScript using a JSDoc import expression:

```javascript
// user.js

/**
 * @param {import("./types.js").User} user
 */
export function displayUser(user) {
  console.log(user.name);
}
```

This provides access to existing TypeScript definitions without importing runtime JavaScript values.

# 🗁 JSDoc Enums and Literal Types

You can describe a fixed set of string values using a union type.

```javascript
/**
 * @typedef {"pending" | "success" | "failed"} Status
 */

/**
 * @param {Status} status
 */
function updateStatus(status) {
  console.log(status);
}

updateStatus("success");
```

Invalid values are rejected during type checking.

```javascript
// TypeScript reports an error.
updateStatus("unknown");
```

# 🗁 JSDoc Readonly Properties

Use `@readonly` to document properties that should not be modified.

```javascript
/**
 * @typedef {Object} User
 * @property {number} id
 * @property {string} name
 */

/**
 * @type {Readonly<User>}
 */
const user = {
  id: 1,
  name: "Alice",
};
```

TypeScript's `Readonly<T>` utility type makes the object's properties readonly during type checking.

# 🗁 Enabling JSDoc Type Checking

JSDoc annotations become especially useful when TypeScript checking is enabled.

Configure `tsconfig.json`:

```json
{
  "compilerOptions": {
    "allowJs": true,
    "checkJs": true,
    "strict": true,
    "noEmit": true
  },
  "include": [
    "src/**/*.js",
    "src/**/*.ts"
  ]
}
```

Run the compiler:

```bash
npx tsc --noEmit
```

TypeScript analyzes JavaScript files and reports errors based on the available JSDoc information.

You can also enable checking for an individual JavaScript file:

```javascript
// @ts-check
```

# 🗁 JSDoc vs TypeScript

| Feature | JSDoc | TypeScript |
| --- | --- | --- |
| File extension | `.js` | `.ts` |
| Type annotations | Comments | Language syntax |
| Type checking | Through TypeScript tooling | Native compiler support |
| Interfaces | Usually through typedefs or imported types | Native interfaces |
| Generics | `@template` | Native generics |
| Migration effort | Lower for existing JavaScript | Requires converting files |
| Runtime | JavaScript | Compiled JavaScript |

JSDoc is useful when retaining JavaScript is important. TypeScript is generally more expressive for large applications and complex type systems.

# 🗁 JSDoc Best Practices

* Document public functions and reusable APIs.
* Use explicit parameter and return types.
* Define reusable structures with `@typedef`.
* Use `@template` for generic functions.
* Prefer imported shared types over duplicated definitions.
* Enable `checkJs` or `// @ts-check` to validate annotations.
* Keep documentation accurate when implementations change.
* Avoid documenting obvious implementation details unnecessarily.
* Use TypeScript directly when JSDoc becomes difficult to maintain.

# 🗁 Common Mistakes

**Using `@param` without a type**

```javascript
/**
 * @param name
 */
function greet(name) {}
```

Prefer:

```javascript
/**
 * @param {string} name
 */
function greet(name) {}
```

**Incorrect return annotations**

The declared return type should match the actual implementation.

**Confusing optional and nullable types**

An optional property may be absent, while a nullable property explicitly allows `null`.

**Using inconsistent typedef definitions**

Duplicating the same object structure in multiple files can create maintenance problems.

**Assuming JSDoc validates code automatically**

JSDoc comments provide type information, but TypeScript checking must be enabled to receive compiler diagnostics.

# 🗁 Key Takeaways

* JSDoc documents JavaScript code using structured comments.
* TypeScript understands JSDoc annotations and uses them for type checking.
* `@param`, `@returns`, and `@type` describe common types.
* `@typedef` defines reusable custom types.
* `@template` supports generic functions.
* `import()` expressions allow JavaScript files to reference TypeScript types.
* `checkJs` and `// @ts-check` enable type checking.
* JSDoc supports gradual adoption of TypeScript without immediately converting JavaScript files.
