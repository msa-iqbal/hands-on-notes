# Type Guards

Type guards are runtime checks that help TypeScript determine the specific type of a value.

They are commonly used when working with union types, unknown values, and objects with different possible structures.

A type guard allows TypeScript to narrow a broader type into a more specific type within a particular code branch.

For example:

```typescript
function printValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

The `typeof` condition acts as a type guard.

* When the condition is true, TypeScript treats `value` as a string.
* Otherwise, TypeScript treats `value` as a number.

# 🗁 Why Type Guards Are Important

Type guards are important because JavaScript applications frequently work with values whose types are not known in advance.

They help when:

* Processing API responses.
* Validating user input.
* Handling union types.
* Working with unknown values.
* Accessing object-specific properties.
* Checking class instances.
* Building reusable validation functions.
* Writing type-safe application logic.

Type guards allow you to perform runtime checks while benefiting from TypeScript's static type analysis.

# 🗁 Built-in Type Guards

TypeScript supports several JavaScript expressions and methods that act as type guards.

Common built-in type guards include:

| Type Guard | Purpose |
| --- | --- |
| `typeof` | Checks primitive types |
| `instanceof` | Checks class or constructor instances |
| `in` | Checks whether an object has a property |
| `Array.isArray()` | Checks whether a value is an array |
| Equality checks | Compares literal or nullable values |
| Truthiness checks | Checks whether a value is truthy or falsy |

# 🗁 typeof Type Guard

The `typeof` operator checks the runtime type of a value.

It is commonly used to narrow primitive types.

Syntax:

```typescript
typeof value === "type"
```

Example:

```typescript
function processValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

Common `typeof` results:

```typescript
console.log(typeof "Hello");      // "string"
console.log(typeof 100);          // "number"
console.log(typeof true);         // "boolean"
console.log(typeof undefined);    // "undefined"
console.log(typeof Symbol());     // "symbol"
console.log(typeof 100n);         // "bigint"
console.log(typeof (() => {}));   // "function"
console.log(typeof null);         // "object"
```

**Important:** `typeof null` returns `"object"` because of a historical JavaScript behavior.

Use an explicit null comparison when checking for null:

```typescript
function displayValue(value: string | null): void {
  if (value !== null) {
    console.log(value.toUpperCase());
  }
}
```

# 🗁 instanceof Type Guard

The `instanceof` operator checks whether an object is an instance of a particular constructor.

It is useful for narrowing class types and built-in object types.

Example:

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

Usage:

```typescript
makeSound(new Dog());
makeSound(new Cat());
```

TypeScript narrows the type based on the `instanceof` check.

You can also use it with built-in classes:

```typescript
function displayDate(value: Date | string): void {
  if (value instanceof Date) {
    console.log(value.toISOString());
  } else {
    console.log(value.toUpperCase());
  }
}
```

# 🗁 in Operator Type Guard

The `in` operator checks whether a property exists in an object or its prototype chain.

It can distinguish between object types that have different properties.

Example:

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

TypeScript narrows the object type according to the property check.

The property does not need to be a method. It can be any property that distinguishes the possible object types.

# 🗁 Array.isArray() Type Guard

The `Array.isArray()` method checks whether a value is an array.

TypeScript recognizes this method as a type guard.

Example:

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

* The `if` branch to `string[]`.
* The `else` branch to `string`.

This is useful when a function accepts either a single value or an array of values.

# 🗁 Equality Type Guards

Equality comparisons can narrow union types.

Example:

```typescript
type Status = "pending" | "approved" | "rejected";

function handleStatus(status: Status): void {
  if (status === "pending") {
    console.log("Waiting for approval");
  } else if (status === "approved") {
    console.log("Approved");
  } else {
    console.log("Rejected");
  }
}
```

TypeScript identifies the remaining possible literal values in each branch.

Equality checks are also useful for nullable types:

```typescript
function displayName(name: string | null): void {
  if (name !== null) {
    console.log(name.toUpperCase());
  } else {
    console.log("Name is unavailable");
  }
}
```

# 🗁 Truthiness Type Guards

A truthiness check can narrow values by excluding falsy possibilities.

Example:

```typescript
function displayName(name: string | null | undefined): void {
  if (name) {
    console.log(name.toUpperCase());
  } else {
    console.log("Name is unavailable");
  }
}
```

Inside the `if` block, TypeScript knows that `name` is a non-empty string.

However, truthiness checks also exclude valid falsy values such as:

* `0`
* `false`
* `""`

For values where these are meaningful, use explicit comparisons.

```typescript
function displayCount(count: number | undefined): void {
  if (count !== undefined) {
    console.log(count);
  }
}
```

# 🗁 Custom Type Guards

A custom type guard is a function that performs a runtime check and communicates the result to TypeScript.

Custom type guards are useful when built-in checks are insufficient.

They are commonly implemented using type predicates.

Syntax:

```typescript
function functionName(
  value: unknown,
): value is SomeType {
  // Runtime validation
}
```

The expression `value is SomeType` is called a type predicate.

Example:

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
```

TypeScript understands that `value` is a string inside the successful branch.

# 🗁 Custom Type Guard for Numbers

You can create a reusable type guard for number values.

```typescript
function isNumber(value: unknown): value is number {
  return typeof value === "number";
}
```

Usage:

```typescript
function processValue(value: unknown): void {
  if (isNumber(value)) {
    console.log(value.toFixed(2));
  }
}
```

The type guard allows the function to safely use number-specific methods.

# 🗁 Custom Type Guard for Objects

You can create custom type guards to validate object structures.

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
    console.log(value.name);
    console.log(value.email);
  } else {
    console.log("Invalid user data");
  }
}
```

This validates the required properties and their types.

The check is useful for basic object validation, although production applications may need more complete validation for nested data and additional constraints.

# 🗁 Custom Type Guard for Arrays

You can validate whether an unknown value is an array containing only strings.

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

`Array.isArray()` checks the array structure, while `every()` verifies that each element is a string.

# 🗁 Type Guards with Discriminated Unions

Discriminated unions use a common property with different literal values to identify each possible type.

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

Use the discriminant as a type guard:

```typescript
function handleResponse(response: ApiResponse): void {
  switch (response.status) {
    case "success":
      console.log(response.data);
      break;

    case "error":
      console.log(response.message);
      break;
  }
}
```

TypeScript narrows the response automatically in each case.

# 🗁 Type Guards with Optional Properties

Optional properties can be checked before accessing them.

```typescript
interface User {
  name: string;
  email?: string;
}

function displayUser(user: User): void {
  if (user.email !== undefined) {
    console.log(user.email.toLowerCase());
  } else {
    console.log("Email not provided");
  }
}
```

The check narrows `user.email` from `string | undefined` to `string`.

# 🗁 Type Guards with Unknown

The `unknown` type requires a value to be narrowed before it can be used in type-specific operations.

Example:

```typescript
function processInput(value: unknown): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else if (typeof value === "number") {
    console.log(value.toFixed(2));
  } else {
    console.log("Unsupported value");
  }
}
```

This makes `unknown` useful for handling values received from external sources.

Unlike `any`, it does not allow unrestricted operations before validation.

# 🗁 Type Guards with Classes

You can create custom type guards for class instances.

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
    public shippingAddress: string,
  ) {}
}

function isAdmin(user: Admin | Customer): user is Admin {
  return user instanceof Admin;
}
```

Usage:

```typescript
function displayUser(user: Admin | Customer): void {
  if (isAdmin(user)) {
    console.log(user.permissions);
  } else {
    console.log(user.shippingAddress);
  }
}
```

The custom type guard makes the class check reusable across the application.

# 🗁 Type Guard vs Type Assertion

Type guards and type assertions are different mechanisms.

**Type guard**

Performs a runtime check and allows TypeScript to narrow a type.

```typescript
function processValue(value: unknown): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  }
}
```

**Type assertion**

Tells TypeScript to treat a value as a particular type without performing runtime validation.

```typescript
const input = document.querySelector("#username") as HTMLInputElement;
```

Comparison:

| Feature | Type Guard | Type Assertion |
| --- | --- | --- |
| Runtime check | Yes | No |
| Narrows type | Yes | Changes compiler's assumed type |
| Validates actual value | Depends on the check | No |
| Type safety | Based on runtime condition | Depends on developer correctness |
| Typical use | Input validation | Known DOM or external type information |

Prefer type guards when the actual runtime value needs to be verified.

# 🗁 Type Guard vs Type Predicate

These concepts are closely related but not identical.

A type guard is the runtime condition or check that helps identify a type.

A type predicate is TypeScript syntax that communicates the result of a custom check.

Example:

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

In this example:

* `typeof value === "string"` performs the runtime check.
* `value is string` is the type predicate.
* `isString()` is the custom type guard function.

# 🗁 Practical Example: Validating API Data

API responses are external data and should not automatically be trusted as a particular TypeScript type.

Consider an API that should return a user object.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}
```

Create a type guard:

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

Use it with parsed JSON:

```typescript
const response: unknown = JSON.parse(`
  {
    "id": 1,
    "name": "Alice",
    "email": "alice@example.com"
  }
`);

if (isUser(response)) {
  console.log(response.name);
  console.log(response.email);
} else {
  console.log("Invalid API response");
}
```

The application accesses user properties only after validating the response.

# 🗁 Practical Example: Application Event Handling

An application may process different event structures.

```typescript
type ClickEvent = {
  type: "click";
  x: number;
  y: number;
};

type KeyboardEvent = {
  type: "keyboard";
  key: string;
};

type AppEvent = ClickEvent | KeyboardEvent;
```

Create a handler:

```typescript
function handleEvent(event: AppEvent): void {
  if (event.type === "click") {
    console.log(`Clicked at: ${event.x}, ${event.y}`);
  } else {
    console.log(`Pressed key: ${event.key}`);
  }
}
```

The discriminant property identifies the correct event structure.

# 🗁 Common Mistakes

**Using a type predicate without a real check**

```typescript
function isString(value: unknown): value is string {
  return true;
}
```

This tells TypeScript that every value is a string, which is incorrect.

Perform a real runtime check:

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

**Checking only whether a value is an object**

```typescript
function isUser(value: unknown): value is User {
  return typeof value === "object" && value !== null;
}
```

This does not prove that the object has the required properties.

Validate the required structure and property types.

**Using instanceof for interfaces**

```typescript
interface User {
  name: string;
}

// An interface cannot be used as an instanceof constructor.
```

Interfaces exist only during compilation and do not create runtime constructors.

Use property checks or custom type guards instead.

**Forgetting that arrays are objects**

```typescript
console.log(typeof []); // "object"
```

Use `Array.isArray()` to identify arrays.

**Assuming a type guard validates every possible constraint**

A check that confirms `age` is a number does not establish that it is positive or within an allowed range.

Validate additional business rules when required.

# 🗁 Best Practices

* Use built-in type guards whenever they are sufficient.
* Create custom type guards for reusable runtime validation.
* Use `unknown` for values from untrusted external sources.
* Make type predicates match the actual runtime checks.
* Validate object properties individually.
* Use discriminated unions for related object variants.
* Use `Array.isArray()` for array detection.
* Prefer explicit null and undefined checks when falsy values are valid.
* Avoid unnecessary type assertions.
* Keep validation logic readable and reusable.
* Separate structural validation from business-rule validation when appropriate.

# 🗁 Summary

* Type guards help TypeScript identify specific types at runtime.
* Built-in guards include `typeof`, `instanceof`, `in`, and `Array.isArray()`.
* Equality and truthiness checks can also narrow types.
* Custom type guards are reusable functions that validate values.
* Type predicates use the `value is Type` syntax.
* Discriminated unions provide an effective way to identify object variants.
* Type guards are useful when working with `unknown` values and API responses.
* Type assertions do not perform runtime validation.
* Correct type guards improve application safety and maintainability.

# 🗁 Related Topics

* Union Types
* Intersection Types
* Type Narrowing
* Type Predicates
* Discriminated Unions
* In Operator
* Instanceof
* Unknown Type
* Never Type
* Type Assertions
* Runtime Validation
* Control Flow Analysis
