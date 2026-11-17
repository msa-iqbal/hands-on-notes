# Parameters Utility Type

The `Parameters<Type>` utility type extracts the parameter types of a function type and returns them as a tuple.

It is useful when you need to reuse a function's parameter types without manually defining them again.

This helps keep related function signatures synchronized and improves type consistency across an application.

# 🗁 Syntax

```typescript
Parameters<Type>
```

* `Type`: A function type.
* Result: A tuple containing the function's parameter types in their original order.

# 🗁 Basic Example

```typescript
function createUser(
  name: string,
  age: number,
  active: boolean
): void {
  console.log(name, age, active);
}

type UserParameters = Parameters<typeof createUser>;

// Result:
// [name: string, age: number, active: boolean]
```

The resulting tuple contains all three parameter types in their original order.

# 🗁 Working with Function Type Aliases

You can extract parameters from a function type alias directly.

```typescript
type Calculate = (
  a: number,
  b: number
) => number;

type CalculateParameters = Parameters<Calculate>;

// Result:
// [a: number, b: number]
```

You do not need `typeof` because `Calculate` is already a function type.

# 🗁 Working with Arrow Functions

`Parameters` also works with arrow functions.

```typescript
const login = (
  email: string,
  password: string
): boolean => {
  return email.length > 0 && password.length > 0;
};

type LoginParameters = Parameters<typeof login>;

// Result:
// [email: string, password: string]
```

# 🗁 Working with Optional Parameters

Optional parameters remain optional in the resulting tuple.

```typescript
function getUser(
  id: number,
  includeProfile?: boolean
): void {
  console.log(id, includeProfile);
}

type UserParameters = Parameters<typeof getUser>;

// Result:
// [id: number, includeProfile?: boolean]
```

The second tuple element is optional.

# 🗁 Working with Default Parameters

A parameter with a default value is optional when calling the function. `Parameters` represents this optionality in the resulting tuple.

```typescript
function connect(
  host: string,
  port: number = 3000
): void {
  console.log(host, port);
}

type ConnectParameters = Parameters<typeof connect>;

// Result:
// [host: string, port?: number]
```

# 🗁 Working with Rest Parameters

Rest parameters are represented as rest elements in the resulting tuple.

```typescript
function sum(
  ...numbers: number[]
): number {
  return numbers.reduce((total, value) => total + value, 0);
}

type SumParameters = Parameters<typeof sum>;

// Result:
// [numbers: number[]]
```

The tuple contains one rest-parameter element representing the array of numbers.

# 🗁 Accessing Individual Parameter Types

Because `Parameters` returns a tuple, you can access individual parameter types using indexed access.

```typescript
function createProduct(
  name: string,
  price: number,
  available: boolean
): void {
  console.log(name, price, available);
}

type ProductParameters = Parameters<typeof createProduct>;

type ProductName = ProductParameters[0];
type ProductPrice = ProductParameters[1];
type ProductAvailability = ProductParameters[2];

// ProductName: string
// ProductPrice: number
// ProductAvailability: boolean
```

Tuple indexes begin at zero.

# 🗁 Extracting All Parameter Types as a Union

You can use `[number]` to extract all element types from the parameter tuple as a union.

```typescript
function process(
  id: number,
  name: string,
  active: boolean
): void {
  console.log(id, name, active);
}

type ProcessParameters = Parameters<typeof process>;

type ParameterTypes = ProcessParameters[number];

// Result:
// number | string | boolean
```

This is useful when you need a union of all possible parameter types rather than their ordered tuple.

# 🗁 Working with Generic Functions

For generic functions, `Parameters` generally extracts the parameter types using their constraints or default behavior rather than a specific function invocation.

```typescript
function identity<T>(value: T): T {
  return value;
}

type IdentityParameters = Parameters<typeof identity>;

// Result:
// [value: unknown]
```

Because `T` has no constraint, its extracted parameter type is `unknown`.

With a constraint:

```typescript
function getValue<T extends string | number>(
  value: T
): T {
  return value;
}

type ValueParameters = Parameters<typeof getValue>;

// Result:
// [value: string | number]
```

# 🗁 Working with Function Overloads

For overloaded functions, `Parameters` uses the parameter tuple of the last overload signature.

```typescript
function format(value: string): string;
function format(value: number): string;
function format(value: string | number): string {
  return String(value);
}

type FormatParameters = Parameters<typeof format>;

// Result:
// [value: number]
```

The last overload is the number signature, so its parameter tuple is extracted.

# 🗁 Using Parameters with Other Utility Types

You can combine `Parameters` with other utility types to build reusable function-related types.

**Using ReturnType**

```typescript
function calculate(a: number, b: number): number {
  return a + b;
}

type CalculateParameters = Parameters<typeof calculate>;
type CalculateResult = ReturnType<typeof calculate>;

// CalculateParameters:
// [a: number, b: number]

// CalculateResult:
// number
```

**Using Awaited and ReturnType**

```typescript
async function fetchUser(id: number) {
  return {
    id,
    name: "John",
  };
}

type FetchUserParameters = Parameters<typeof fetchUser>;

type FetchUserResult = Awaited<
  ReturnType<typeof fetchUser>
>;

// FetchUserParameters:
// [id: number]

// FetchUserResult:
// { id: number; name: string }
```

# 🗁 How Parameters Works Internally

The built-in `Parameters` utility type uses a conditional type with the `infer` keyword.

Its simplified definition is:

```typescript
type Parameters<T extends (...args: any) => any> =
  T extends (...args: infer P) => any
    ? P
    : never;
```

The `infer P` expression asks TypeScript to infer the function's parameter tuple and assign it to `P`.

Consider:

```typescript
type Greet = (
  name: string,
  age: number
) => void;

type Result = Parameters<Greet>;
```

TypeScript infers:

```typescript
type Result = [name: string, age: number];
```

# 🗁 Practical Example: Reusing Function Parameters

Suppose an application has a function for creating users. Another function needs to accept exactly the same arguments.

Instead of duplicating the parameter definitions, derive them using `Parameters`.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

function createUser(
  name: string,
  email: string,
  active: boolean = true
): User {
  return {
    id: Date.now(),
    name,
    email,
  };
}

type CreateUserParameters = Parameters<typeof createUser>;

function logUserCreation(
  ...args: CreateUserParameters
): void {
  console.log("Creating user:", args);
}

logUserCreation("John", "john@example.com");
logUserCreation("Alice", "alice@example.com", false);
```

If the original function's parameter types change, the derived tuple updates automatically.

# 🗁 Practical Example: Creating a Function Wrapper

You can use `Parameters` and `ReturnType` together to create a reusable wrapper that preserves a function's parameter and return types.

```typescript
function withLogging<T extends (...args: any[]) => any>(
  fn: T
): (...args: Parameters<T>) => ReturnType<T> {
  return (...args: Parameters<T>): ReturnType<T> => {
    console.log("Function called with:", args);

    return fn(...args);
  };
}

function multiply(a: number, b: number): number {
  return a * b;
}

const loggedMultiply = withLogging(multiply);

const result = loggedMultiply(5, 4);

// result: number
```

The wrapper preserves the original function's parameter and return types.

# 🗁 Important Behavior

**Parameters requires a function type.**

```typescript
type Result = Parameters<string>;

// TypeScript error:
// string does not satisfy the function constraint.
```

**Parameters preserves parameter order.**

```typescript
type Handler = (
  id: number,
  name: string,
  active: boolean
) => void;

type HandlerParameters = Parameters<Handler>;

// [id: number, name: string, active: boolean]
```

Changing the order changes the resulting tuple.

**Parameters does not execute the function.**

It performs compile-time type extraction only.

**Generic functions may produce broader parameter types.**

A generic parameter without a constraint is commonly inferred as `unknown` when extracted.

**Overloaded functions use their last overload signature.**

The utility does not create a union of every overload's parameter tuple.

# 🗁 Best Practices

* Use `Parameters` to reuse existing function parameter types.
* Combine it with `typeof` when referencing declared function variables.
* Use tuple indexing to access individual parameter types.
* Use `[number]` when you need a union of all parameter element types.
* Combine it with `ReturnType` when building function wrappers.
* Remember that optional and rest parameters are preserved.
* Understand how generic functions and overloads affect extracted types.

# 🗁 Summary

* `Parameters<T>` extracts the parameter types of a function.
* It returns the parameter types as a tuple.
* It preserves parameter order, optionality, and rest parameters.
* It works with regular functions, arrow functions, and function type aliases.
* It uses the `infer` keyword internally.
* Generic functions may produce broader parameter types.
* For overloaded functions, it uses the last overload signature.
* It is useful for reusable function signatures and strongly typed wrappers.
