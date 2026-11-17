# Type Predicates

Type predicates are a TypeScript feature that allows a function to communicate a specific type-checking result to the compiler.

They are commonly used when creating custom type guards.

A type predicate tells TypeScript that when a function returns `true`, a particular value belongs to a specified type.

The syntax uses the `is` keyword.

Example:

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

Here:

* `value` is the parameter being checked.
* `unknown` means the parameter can contain any value.
* `value is string` is the type predicate.
* `typeof value === "string"` performs the runtime check.

Usage:

```typescript
const value: unknown = "Hello";

if (isString(value)) {
  console.log(value.toUpperCase());
}
```

Inside the `if` block, TypeScript knows that `value` is a string.

# 🗁 Type Predicate Syntax

The general syntax of a type predicate is:

```typescript
function functionName(
  parameter: ParameterType,
): parameter is TargetType {
  // Runtime validation
}
```

The predicate must refer to a parameter of the function.

A type predicate is not a runtime JavaScript operator. It is TypeScript syntax that communicates the result of a runtime check to the compiler.

# 🗁 Basic Type Predicate

Create a function that checks whether a value is a string.

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

Usage:

```typescript
function displayValue(value: unknown): void {
  if (isString(value)) {
    console.log(value.toUpperCase());
  } else {
    console.log("Value is not a string");
  }
}

displayValue("Hello");
displayValue(100);
```

TypeScript narrows the value to `string` when the predicate returns `true`.

# 🗁 Type Predicates with Primitive Types

Type predicates can identify different primitive types.

String:

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

Number:

```typescript
function isNumber(value: unknown): value is number {
  return typeof value === "number";
}
```

Boolean:

```typescript
function isBoolean(value: unknown): value is boolean {
  return typeof value === "boolean";
}
```

Usage:

```typescript
function processValue(value: unknown): void {
  if (isString(value)) {
    console.log(value.toUpperCase());
  } else if (isNumber(value)) {
    console.log(value.toFixed(2));
  } else if (isBoolean(value)) {
    console.log(value ? "True" : "False");
  }
}
```

Each predicate communicates a different type to TypeScript.

# 🗁 Type Predicates with Union Types

Type predicates are particularly useful when narrowing union types.

```typescript
type User = {
  id: number;
  name: string;
};

type Product = {
  id: number;
  title: string;
};

type Entity = User | Product;
```

Create a predicate for `User`:

```typescript
function isUser(entity: Entity): entity is User {
  return "name" in entity;
}
```

Usage:

```typescript
function displayEntity(entity: Entity): void {
  if (isUser(entity)) {
    console.log(entity.name);
  } else {
    console.log(entity.title);
  }
}
```

The predicate identifies whether the value belongs to the `User` type.

# 🗁 Type Predicates with Object Types

You can use type predicates to validate object structures.

Example:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

function isUser(value: unknown): value is User {
  if (typeof value !== "object" || value === null) {
    return false;
  }

  if (!("id" in value) || !("name" in value) || !("email" in value)) {
    return false;
  }

  return (
    typeof value.id === "number" &&
    typeof value.name === "string" &&
    typeof value.email === "string"
  );
}
```

Usage:

```typescript
function displayUser(value: unknown): void {
  if (isUser(value)) {
    console.log(value.id);
    console.log(value.name);
    console.log(value.email);
  } else {
    console.log("Invalid user");
  }
}
```

The function verifies the object's structure before claiming that it is a `User`.

# 🗁 Type Predicates with Arrays

Type predicates can identify arrays containing elements of a particular type.

Example:

```typescript
function isStringArray(value: unknown): value is string[] {
  return (
    Array.isArray(value) &&
    value.every((item) => typeof item === "string")
  );
}
```

Usage:

```typescript
function displayItems(value: unknown): void {
  if (isStringArray(value)) {
    value.forEach((item) => {
      console.log(item.toUpperCase());
    });
  }
}
```

The predicate verifies both:

* The value is an array.
* Every array element is a string.

# 🗁 Type Predicates with Generic Types

Type predicates can work with generic types.

For example, you can create a reusable predicate that checks whether a value is an array of a specified element type.

```typescript
function isArrayOf<T>(
  value: unknown,
  guard: (item: unknown) => item is T,
): value is T[] {
  return Array.isArray(value) && value.every(guard);
}
```

Define element guards:

```typescript
function isNumber(value: unknown): value is number {
  return typeof value === "number";
}

function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

Usage:

```typescript
const numbers: unknown = [10, 20, 30];

if (isArrayOf(numbers, isNumber)) {
  const total = numbers.reduce((sum, item) => sum + item, 0);

  console.log(total);
}
```

Another example:

```typescript
const names: unknown = ["Alice", "Bob"];

if (isArrayOf(names, isString)) {
  names.forEach((name) => {
    console.log(name.toUpperCase());
  });
}
```

The generic predicate makes array validation reusable for different element types.

# 🗁 Type Predicates with Classes

Type predicates can identify class instances.

```typescript
class Admin {
  constructor(
    public name: string,
    public permissions: string[],
  ) {}
}

class Customer {
  constructor(
    public name: string,
    public address: string,
  ) {}
}
```

Create a predicate:

```typescript
function isAdmin(
  user: Admin | Customer,
): user is Admin {
  return user instanceof Admin;
}
```

Usage:

```typescript
function displayUser(user: Admin | Customer): void {
  if (isAdmin(user)) {
    console.log(user.permissions);
  } else {
    console.log(user.address);
  }
}
```

The predicate makes the class check reusable.

# 🗁 Type Predicates with Discriminated Unions

Type predicates can identify members of a discriminated union.

```typescript
type SuccessResponse = {
  status: "success";
  data: string;
};

type ErrorResponse = {
  status: "error";
  message: string;
};

type ApiResponse = SuccessResponse | ErrorResponse;
```

Create a predicate:

```typescript
function isSuccessResponse(
  response: ApiResponse,
): response is SuccessResponse {
  return response.status === "success";
}
```

Usage:

```typescript
function handleResponse(response: ApiResponse): void {
  if (isSuccessResponse(response)) {
    console.log(response.data);
  } else {
    console.log(response.message);
  }
}
```

This allows you to reuse the same narrowing logic across multiple functions.

# 🗁 Type Predicates with Filter

The `Array.prototype.filter()` method supports type predicates.

This is particularly useful when filtering arrays containing multiple types.

Example:

```typescript
const values: (string | number)[] = [
  "Alice",
  100,
  "Bob",
  200,
  "Charlie",
];
```

Create a predicate:

```typescript
function isString(value: string | number): value is string {
  return typeof value === "string";
}
```

Filter the array:

```typescript
const names = values.filter(isString);

console.log(names);
```

The inferred type of `names` is:

```typescript
string[]
```

You can also filter numbers:

```typescript
function isNumber(value: string | number): value is number {
  return typeof value === "number";
}

const numbers = values.filter(isNumber);
```

The inferred type of `numbers` is:

```typescript
number[]
```

# 🗁 Type Predicates with find

The `Array.prototype.find()` method can also use type predicates.

Example:

```typescript
type User = {
  id: number;
  name: string;
};

type Product = {
  id: number;
  title: string;
};

type Entity = User | Product;

const entities: Entity[] = [
  { id: 1, name: "Alice" },
  { id: 2, title: "Laptop" },
  { id: 3, name: "Bob" },
];
```

Create a predicate:

```typescript
function isUser(entity: Entity): entity is User {
  return "name" in entity;
}
```

Find a user:

```typescript
const user = entities.find(isUser);

if (user) {
  console.log(user.name);
}
```

The result is narrowed to `User | undefined`.

# 🗁 Type Predicates with filter and find

The same type predicate can be reused with multiple array methods.

```typescript
type Admin = {
  role: "admin";
  name: string;
};

type Customer = {
  role: "customer";
  name: string;
};

type User = Admin | Customer;

const users: User[] = [
  { role: "admin", name: "Alice" },
  { role: "customer", name: "Bob" },
  { role: "admin", name: "Charlie" },
];
```

Create a reusable predicate:

```typescript
function isAdmin(user: User): user is Admin {
  return user.role === "admin";
}
```

Filter administrators:

```typescript
const admins = users.filter(isAdmin);
```

Find an administrator:

```typescript
const firstAdmin = users.find(isAdmin);
```

The inferred types are:

* `admins`: `Admin[]`
* `firstAdmin`: `Admin | undefined`

# 🗁 Type Predicate vs Type Guard

These concepts are closely related.

A type guard is a runtime check that helps narrow a value's type.

A type predicate is TypeScript syntax that communicates a custom function's narrowing result.

Example:

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

In this example:

* `typeof value === "string"` is the runtime check.
* `value is string` is the type predicate.
* `isString()` is a custom type guard function.

A type predicate is one way to implement a custom type guard.

# 🗁 Type Predicate vs Type Assertion

Type predicates and type assertions serve different purposes.

**Type predicate:**

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

The function checks the actual runtime value.

**Type assertion:**

```typescript
const value = someUnknownValue as string;
```

The assertion tells TypeScript to treat the value as a string without checking it at runtime.

| Feature | Type Predicate | Type Assertion |
| --- | --- | --- |
| Runtime validation | Yes, if implemented correctly | No |
| Communicates narrowing | Yes | No runtime narrowing |
| Syntax | `value is Type` | `value as Type` |
| Reusable validation | Yes | No |
| Typical use | Custom type guards | Known external type information |

Prefer type predicates when you need to verify the actual value.

# 🗁 Explicit Type Predicates vs Inferred Type Predicates

TypeScript can sometimes infer type predicates from function bodies.

For example:

```typescript
const isNumber = (value: unknown) =>
  typeof value === "number";
```

In modern TypeScript versions, this function can receive an inferred type predicate return type.

You can also declare it explicitly:

```typescript
function isNumber(value: unknown): value is number {
  return typeof value === "number";
}
```

Explicit predicates make the intended contract clear and are useful when documenting reusable validation functions.

# 🗁 Assertion Functions vs Type Predicates

TypeScript also supports assertion functions.

An assertion function uses the `asserts` keyword instead of the `is` keyword.

A type predicate returns a boolean:

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

Usage:

```typescript
if (isString(value)) {
  console.log(value.toUpperCase());
}
```

An assertion function either completes normally or throws an error:

```typescript
function assertString(value: unknown): asserts value is string {
  if (typeof value !== "string") {
    throw new TypeError("Expected a string");
  }
}
```

Usage:

```typescript
function processValue(value: unknown): void {
  assertString(value);

  console.log(value.toUpperCase());
}
```

After the assertion function completes successfully, TypeScript treats `value` as a string.

| Feature | Type Predicate | Assertion Function |
| --- | --- | --- |
| Syntax | `value is Type` | `asserts value is Type` |
| Return behavior | Boolean | Normally returns `void` |
| Failed check | Returns `false` | Usually throws |
| Typical use | Conditional narrowing | Enforcing required conditions |

# 🗁 Practical Example: API Response Validation

Suppose an application receives user data from an external API.

Define the expected structure:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}
```

Create a type predicate:

```typescript
function isUser(value: unknown): value is User {
  if (typeof value !== "object" || value === null) {
    return false;
  }

  if (!("id" in value) || !("name" in value) || !("email" in value)) {
    return false;
  }

  return (
    typeof value.id === "number" &&
    typeof value.name === "string" &&
    typeof value.email === "string"
  );
}
```

Use it to validate an API response:

```typescript
async function getUser(): Promise<void> {
  const response = await fetch("/api/user");
  const data: unknown = await response.json();

  if (isUser(data)) {
    console.log("User:", data.name);
    console.log("Email:", data.email);
  } else {
    console.log("Invalid user response");
  }
}
```

The API data is treated as `unknown` until the validation function confirms its structure.

# 🗁 Practical Example: Filtering Mixed Data

Consider an array containing strings, numbers, and booleans.

```typescript
const values: unknown[] = [
  "Alice",
  100,
  true,
  "Bob",
  200,
  false,
];
```

Create type predicates:

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}

function isNumber(value: unknown): value is number {
  return typeof value === "number";
}

function isBoolean(value: unknown): value is boolean {
  return typeof value === "boolean";
}
```

Filter each type:

```typescript
const strings = values.filter(isString);
const numbers = values.filter(isNumber);
const booleans = values.filter(isBoolean);
```

The inferred types are:

```typescript
string[]
number[]
boolean[]
```

This approach keeps filtering logic reusable and type-safe.

# 🗁 Common Mistakes

**Using an incorrect type predicate**

```typescript
function isString(value: unknown): value is string {
  return true;
}
```

This incorrectly claims that every value is a string.

Always make the runtime condition match the declared predicate.

**Checking only the object type**

```typescript
function isUser(value: unknown): value is User {
  return typeof value === "object" && value !== null;
}
```

This does not verify the required properties.

Validate the structure and property types before returning `true`.

**Confusing the predicate with a runtime operator**

```typescript
value is string
```

This is not a JavaScript expression that can be evaluated directly.

It is TypeScript return-type syntax used in function declarations.

**Assuming a type predicate guarantees business rules**

A predicate that checks whether `age` is a number does not guarantee that the age is valid for the application.

For example:

```typescript
function isValidAge(value: unknown): value is number {
  return typeof value === "number" && value >= 0 && value <= 120;
}
```

If the application requires a particular range, include that condition in the runtime check.

**Using type assertions instead of validation**

```typescript
const user = response as User;
```

This does not validate the API response.

Use a type predicate when the actual structure is unknown.

# 🗁 Best Practices

* Use type predicates for reusable custom type guards.
* Use `unknown` for values that require validation.
* Make runtime checks match the declared predicate.
* Validate object properties and their actual types.
* Use generic predicates to create reusable validation utilities.
* Reuse predicates with array methods such as `filter()` and `find()`.
* Use discriminated unions when a literal property can identify an object variant.
* Prefer explicit predicates for important public validation functions.
* Use assertion functions when invalid values should immediately cause an exception.
* Keep structural validation separate from application-specific business rules when appropriate.

# 🗁 Summary

* Type predicates communicate custom type-checking results to TypeScript.
* They use the `parameter is Type` syntax.
* They are commonly used to implement custom type guards.
* Type predicates support primitive, object, array, class, and union types.
* Generic predicates make validation functions reusable.
* Array methods such as `filter()` and `find()` can benefit from type predicates.
* Type predicates differ from type assertions.
* Assertion functions use `asserts` to narrow values after successful validation.
* Accurate runtime checks are essential for reliable type predicates.

# 🗁 Related Topics

* Union Types
* Intersection Types
* Type Narrowing
* Type Guards
* Discriminated Unions
* Generic Functions
* Generic Constraints
* Unknown Type
* Never Type
* Type Assertions
* Assertion Functions
* Runtime Validation
