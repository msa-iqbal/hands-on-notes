# Type Narrowing

Type narrowing is a TypeScript feature that allows the compiler to determine a more specific type from a broader type.

When a variable has multiple possible types, TypeScript initially treats it as the declared union or broader type.

By checking the value or its properties, TypeScript can narrow the type within a particular scope.

For example, a variable might accept both strings and numbers:

```typescript
function printValue(value: string | number): void {
  console.log(value);
}
```

Inside this function, `value` can be either a string or a number.

If you check its type, TypeScript can determine which type is currently available:

```typescript
function printValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

TypeScript understands that:

* Inside the `if` block, `value` is a string.
* Inside the `else` block, `value` is a number.

This process is called type narrowing.

# 🗁 Why Type Narrowing Is Important

Type narrowing helps you write safer and more predictable TypeScript code.

It is useful when:

* Working with union types.
* Handling API responses.
* Validating function arguments.
* Accessing properties of different object structures.
* Handling nullable values.
* Processing different application states.
* Avoiding unnecessary type assertions.

Without narrowing, TypeScript cannot safely allow operations that are valid for only some possible types.

# 🗁 Type Narrowing with typeof

The `typeof` operator is one of the most common ways to narrow primitive types.

JavaScript's `typeof` operator returns a string describing the runtime type of a value.

Common results include:

| Value | Result |
| --- | --- |
| `"Hello"` | `"string"` |
| `100` | `"number"` |
| `true` | `"boolean"` |
| `undefined` | `"undefined"` |
| `Symbol()` | `"symbol"` |
| `100n` | `"bigint"` |
| `function () {}` | `"function"` |
| `null` | `"object"` |

Example:

```typescript
function processValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}

processValue("hello");
processValue(25);
```

TypeScript narrows the type based on the condition.

# 🗁 Type Narrowing with typeof and Multiple Conditions

You can use multiple `typeof` checks to handle several possible types.

```typescript
function processValue(
  value: string | number | boolean,
): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else if (typeof value === "number") {
    console.log(value.toFixed(2));
  } else {
    console.log(value ? "Enabled" : "Disabled");
  }
}
```

Each branch receives a more specific type.

# 🗁 Type Narrowing with Truthiness

TypeScript can narrow types using truthiness checks.

JavaScript considers certain values falsy:

* `false`
* `0`
* `-0`
* `0n`
* `""`
* `null`
* `undefined`
* `NaN`

All other values are truthy.

Example:

```typescript
function printName(name: string | null): void {
  if (name) {
    console.log(name.toUpperCase());
  } else {
    console.log("Name is unavailable");
  }
}
```

Inside the `if` block, TypeScript knows that `name` is a string.

Inside the `else` block, `name` is `null`.

However, truthiness checks can also exclude valid values such as an empty string or zero.

Consider:

```typescript
function printCount(count: number | undefined): void {
  if (count) {
    console.log(count);
  } else {
    console.log("Count is missing or zero");
  }
}
```

This condition treats both `undefined` and `0` as falsy.

If zero is a valid value, use an explicit check:

```typescript
function printCount(count: number | undefined): void {
  if (count !== undefined) {
    console.log(count);
  } else {
    console.log("Count is unavailable");
  }
}
```

# 🗁 Type Narrowing with Equality Checks

Equality checks can narrow union types.

Example:

```typescript
function printValue(value: string | number): void {
  if (value === "admin") {
    console.log("Administrator");
  } else if (typeof value === "number") {
    console.log(value.toFixed(2));
  } else {
    console.log(value.toUpperCase());
  }
}
```

TypeScript understands the possible values in each branch.

You can also narrow nullable values:

```typescript
function displayName(name: string | null): void {
  if (name !== null) {
    console.log(name.toUpperCase());
  } else {
    console.log("No name provided");
  }
}
```

Strict equality checks are generally preferable because they make the intended comparison explicit.

# 🗁 Type Narrowing with in Operator

The `in` operator checks whether a property exists in an object or its prototype chain.

TypeScript can use this check to distinguish between object types.

```typescript
type Admin = {
  name: string;
  permissions: string[];
};

type Customer = {
  name: string;
  shippingAddress: string;
};

function displayUser(user: Admin | Customer): void {
  if ("permissions" in user) {
    console.log(user.permissions);
  } else {
    console.log(user.shippingAddress);
  }
}
```

TypeScript narrows the type based on the presence of the property.

The `in` operator also checks inherited properties, not only an object's own properties.

# 🗁 Type Narrowing with instanceof

The `instanceof` operator checks whether an object is an instance of a particular class or constructor.

TypeScript uses it to narrow object types.

```typescript
class Dog {
  bark(): void {
    console.log("Woof!");
  }
}

class Cat {
  meow(): void {
    console.log("Meow!");
  }
}

function makeSound(animal: Dog | Cat): void {
  if (animal instanceof Dog) {
    animal.bark();
  } else {
    animal.meow();
  }
}
```

Inside each branch, TypeScript knows the specific class type.

`instanceof` is especially useful when working with class instances and built-in objects such as `Date` and `Error`.

# 🗁 Type Narrowing with Array.isArray

The `Array.isArray()` method checks whether a value is an array.

TypeScript recognizes it as a type guard.

```typescript
function processValue(value: string | string[]): void {
  if (Array.isArray(value)) {
    value.forEach((item) => {
      console.log(item.toUpperCase());
    });
  } else {
    console.log(value.toUpperCase());
  }
}
```

TypeScript narrows:

* The first branch to `string[]`.
* The second branch to `string`.

# 🗁 Type Narrowing with Discriminated Unions

Discriminated unions use a shared property with different literal values to identify each union member.

Example:

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

Use the discriminant property to narrow the type:

```typescript
function handleResponse(response: ApiResponse): void {
  if (response.status === "success") {
    console.log(response.data);
  } else {
    console.log(response.message);
  }
}
```

TypeScript determines the correct object structure from the value of `status`.

This pattern is especially useful for API responses, application states, and event handling.

# 🗁 Type Narrowing with Custom Type Guards

You can create reusable functions that check whether a value belongs to a specific type.

A custom type guard uses a type predicate in its return type.

Syntax:

```typescript
function functionName(
  value: unknown,
): value is SomeType {
  // Runtime validation
}
```

Example:

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

Use the function:

```typescript
function printValue(value: unknown): void {
  if (isString(value)) {
    console.log(value.toUpperCase());
  } else {
    console.log("Value is not a string");
  }
}
```

The return type `value is string` tells TypeScript that a successful check establishes the value as a string.

# 🗁 Type Narrowing with never

The `never` type represents values that cannot occur.

It is useful for checking whether all members of a union have been handled.

Example:

```typescript
type Status = "pending" | "approved" | "rejected";

function handleStatus(status: Status): void {
  switch (status) {
    case "pending":
      console.log("Waiting for approval");
      break;

    case "approved":
      console.log("Request approved");
      break;

    case "rejected":
      console.log("Request rejected");
      break;

    default: {
      const exhaustiveCheck: never = status;
      return exhaustiveCheck;
    }
  }
}
```

After all possible cases have been handled, TypeScript narrows `status` to `never` in the default branch.

If a new status is added without updating the switch statement, TypeScript reports an error.

# 🗁 Type Narrowing with Optional Properties

TypeScript can narrow object types by checking whether an optional property is defined.

```typescript
type User = {
  name: string;
  email?: string;
};

function displayUser(user: User): void {
  if (user.email !== undefined) {
    console.log(user.email.toLowerCase());
  } else {
    console.log("Email is unavailable");
  }
}
```

The explicit check ensures that `email` is available before calling a string method.

# 🗁 Type Narrowing with Null and Undefined

TypeScript supports narrowing nullable values through explicit comparisons.

```typescript
function getLength(
  value: string | null | undefined,
): number {
  if (value === null || value === undefined) {
    return 0;
  }

  return value.length;
}
```

After the condition, TypeScript knows that `value` is a string.

You can also use a nullish comparison:

```typescript
function getLength(
  value: string | null | undefined,
): number {
  if (value != null) {
    return value.length;
  }

  return 0;
}
```

The loose inequality check `!= null` intentionally excludes both `null` and `undefined`.

# 🗁 Control Flow Analysis

TypeScript uses control flow analysis to track how variable types change through different branches of a program.

Example:

```typescript
function processValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
    return;
  }

  console.log(value.toFixed(2));
}
```

After the first branch returns, TypeScript knows that the remaining execution path can only contain a number.

Type narrowing works with:

* `if` and `else` statements.
* `switch` statements.
* Conditional expressions.
* Early returns.
* Throw statements.
* Assignments.
* Logical operators.

TypeScript analyzes the reachable code paths to determine the most specific type available at each point.

# 🗁 Type Narrowing with Logical Operators

Logical operators can also help narrow types.

Using the logical AND operator:

```typescript
function printName(name: string | null): void {
  if (name !== null && name.length > 0) {
    console.log(name.toUpperCase());
  }
}
```

The second condition is evaluated only when the first condition is true.

TypeScript therefore knows that `name` is a string when accessing `length`.

Using the logical OR operator:

```typescript
function displayName(name: string | null): string {
  return name === null || name.length === 0
    ? "Unknown"
    : name.toUpperCase();
}
```

TypeScript uses the conditions to determine which operations are safe.

# 🗁 Type Narrowing and Type Assertions

Type narrowing and type assertions are different concepts.

**Type narrowing**

TypeScript determines a more specific type based on runtime checks.

```typescript
function printValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  }
}
```

**Type assertion**

A developer explicitly tells TypeScript to treat a value as a particular type.

```typescript
const input = document.querySelector("#username") as HTMLInputElement;
```

A type assertion does not perform runtime validation.

Prefer type narrowing when the actual runtime type must be verified.

# 🗁 Practical Example: Processing User Input

A function may receive a string, a number, or an array of strings.

```typescript
function processInput(
  input: string | number | string[],
): void {
  if (typeof input === "string") {
    console.log("String:", input.toUpperCase());
  } else if (typeof input === "number") {
    console.log("Number:", input.toFixed(2));
  } else {
    console.log("Array:");

    input.forEach((item) => {
      console.log(item.toUpperCase());
    });
  }
}
```

Usage:

```typescript
processInput("hello");
processInput(100);
processInput(["alice", "bob"]);
```

Each possible input is handled safely.

# 🗁 Practical Example: API Response Handling

Consider an API that returns either a successful result or an error.

```typescript
type ApiSuccess = {
  success: true;
  data: {
    id: number;
    name: string;
  };
};

type ApiError = {
  success: false;
  error: {
    code: number;
    message: string;
  };
};

type ApiResponse = ApiSuccess | ApiError;
```

Handle the response:

```typescript
function handleApiResponse(response: ApiResponse): void {
  if (response.success) {
    console.log("User ID:", response.data.id);
    console.log("User Name:", response.data.name);
  } else {
    console.log("Error Code:", response.error.code);
    console.log("Message:", response.error.message);
  }
}
```

The `success` property allows TypeScript to narrow the response automatically.

# 🗁 Common Mistakes

**Assuming typeof null returns null**

```typescript
console.log(typeof null); // "object"
```

This is a historical JavaScript behavior.

Use an explicit null check:

```typescript
if (value !== null) {
  // Value is not null.
}
```

**Using truthiness when zero or an empty string is valid**

```typescript
function processCount(count: number | undefined): void {
  if (count) {
    console.log(count);
  }
}
```

This incorrectly treats zero as missing.

Use:

```typescript
if (count !== undefined) {
  console.log(count);
}
```

**Accessing union-specific properties without checking**

```typescript
type Admin = {
  permissions: string[];
};

type Customer = {
  address: string;
};

function displayUser(user: Admin | Customer): void {
  // Error:
  // console.log(user.permissions);
}
```

Narrow the type before accessing properties that are not shared.

**Confusing type assertions with runtime checks**

```typescript
const value = someUnknownValue as string;
```

This does not verify that the value is actually a string.

Use a runtime check when the value is not already trusted.

**Assuming every object can be checked with instanceof**

`instanceof` works with constructor functions and class instances, not arbitrary TypeScript interfaces.

Use property checks, discriminated unions, or custom type guards for structural object types.

# 🗁 Best Practices

* Prefer runtime checks over unnecessary type assertions.
* Use `typeof` for primitive types.
* Use `instanceof` for class instances.
* Use `in` to check object properties.
* Use `Array.isArray()` to identify arrays.
* Use discriminated unions for related object variants.
* Create reusable custom type guards for complex validation.
* Handle `null` and `undefined` explicitly.
* Avoid truthiness checks when valid values include `0`, `false`, or empty strings.
* Use exhaustive checking for important union-based logic.
* Keep type guards aligned with the actual runtime validation they perform.

# 🗁 Summary

* Type narrowing makes a broad type more specific.
* It is commonly used with union types.
* `typeof` narrows primitive types.
* Truthiness and equality checks narrow values.
* The `in` operator checks object properties.
* `instanceof` identifies class instances.
* `Array.isArray()` narrows array types.
* Discriminated unions use literal properties to identify variants.
* Custom type guards use type predicates.
* `never` supports exhaustive checking.
* TypeScript uses control flow analysis to track types across execution paths.
* Type narrowing improves type safety without relying on unnecessary assertions.

# 🗁 Related Topics

* Union Types
* Intersection Types
* Type Guards
* Type Predicates
* Discriminated Unions
* In Operator
* Instanceof
* Exhaustiveness Checking
* Unknown Type
* Never Type
* Type Assertions
* Control Flow Analysis
