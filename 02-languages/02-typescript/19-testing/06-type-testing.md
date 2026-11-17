# Type Testing in TypeScript

Type testing is the process of verifying that TypeScript correctly accepts or rejects specific types and expressions.

Unlike runtime tests, which execute JavaScript and verify application behavior, type tests validate the behavior of the TypeScript compiler.

They help ensure that:

* Functions accept the intended argument types.
* Functions return the expected types.
* Generic types behave correctly.
* Type utilities produce the expected results.
* Invalid assignments are rejected.
* Public APIs maintain their intended type contracts.

Type tests are especially useful when building libraries, reusable components, SDKs, and complex TypeScript applications.

# 🗁 Runtime Testing vs Type Testing

| Runtime Testing | Type Testing |
| --- | --- |
| Tests executed JavaScript behavior | Tests compile-time type behavior |
| Uses assertions such as `expect()` | Uses type assertions and compiler diagnostics |
| Detects runtime errors | Detects type-level errors |
| Runs application code | Usually relies on TypeScript compilation |
| Examples: Jest, Vitest | Examples: `tsc`, `expect-type`, `tsd` |

Consider a function:

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

A runtime test verifies its output:

```typescript
expect(add(10, 20)).toBe(30);
```

A type test verifies that its parameters and return value have the intended types.

# 🗁 Testing Types with TypeScript Compiler

The TypeScript compiler can verify type contracts without executing the application.

Create a test file:

```typescript
// type-tests.ts

function add(a: number, b: number): number {
  return a + b;
}

const result: number = add(10, 20);
```

Compile it:

```bash
npx tsc --noEmit --strict type-tests.ts
```

The `--noEmit` option checks the source without generating JavaScript files.

If the code violates the declared types, the compiler reports an error.

# 🗁 Testing Expected Type Errors

Use `@ts-expect-error` to verify that an expression produces a TypeScript error.

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}

// Valid usage
greet("Alice");

// @ts-expect-error - name must be a string
greet(123);
```

If the following line produces a TypeScript error, the compiler considers the expectation satisfied.

If the line no longer produces an error, TypeScript reports an unused `@ts-expect-error` directive.

This makes it useful for regression testing.

# 🗁 Using @ts-expect-error Correctly

Prefer `@ts-expect-error` over `@ts-ignore` in type tests.

**`@ts-expect-error`**

```typescript
// @ts-expect-error
const username: string = 123;
```

The compiler reports an error if the expected error disappears.

**`@ts-ignore`**

```typescript
// @ts-ignore
const username: string = 123;
```

The compiler suppresses the error without checking whether an error actually exists.

Use `@ts-expect-error` when testing invalid type usage.

# 🗁 Testing Types with expect-type

The `expect-type` library provides compile-time assertions for TypeScript types.

Install it:

```bash
npm install -D expect-type
```

Example:

```typescript
import { expectTypeOf } from "expect-type";

function getUsername(id: number): string {
  return `user-${id}`;
}

expectTypeOf(getUsername).parameter(0).toBeNumber();

expectTypeOf(getUsername).returns.toBeString();

expectTypeOf(getUsername(1)).toBeString();
```

These assertions are checked by TypeScript rather than executed as ordinary runtime assertions.

# 🗁 Testing Type Equality

You can use `expectTypeOf().toEqualTypeOf()` to verify that two types are equivalent.

```typescript
import { expectTypeOf } from "expect-type";

type User = {
  id: number;
  name: string;
};

type ExpectedUser = {
  id: number;
  name: string;
};

expectTypeOf<User>().toEqualTypeOf<ExpectedUser>();
```

This is useful when testing:

* Complex generic types.
* Utility types.
* Conditional types.
* Mapped types.
* Public API definitions.

# 🗁 Testing Generic Functions

Generic functions should preserve relationships between input and output types.

Consider:

```typescript
function identity<T>(value: T): T {
  return value;
}
```

Test its inferred types:

```typescript
import { expectTypeOf } from "expect-type";

expectTypeOf(identity("hello")).toBeString();

expectTypeOf(identity(123)).toBeNumber();

expectTypeOf(identity(true)).toBeBoolean();
```

You can also verify the generic function's declared signature:

```typescript
expectTypeOf(identity)
  .parameter(0)
  .toBeUnknown();
```

This checks the generic parameter's constraint as exposed through the library's type inspection.

# 🗁 Testing Union Types

Type tests can verify that a type accepts all intended members of a union.

```typescript
type Status = "pending" | "success" | "failed";

function updateStatus(status: Status): void {
  console.log(status);
}
```

Test valid values:

```typescript
updateStatus("pending");
updateStatus("success");
updateStatus("failed");
```

Test invalid values:

```typescript
// @ts-expect-error - invalid status
updateStatus("unknown");
```

# 🗁 Testing Utility Types

Type tests are valuable when creating custom utility types.

Consider a utility type that makes selected properties optional:

```typescript
type MakeOptional<T, K extends keyof T> =
  Omit<T, K> & Partial<Pick<T, K>>;
```

Test it:

```typescript
import { expectTypeOf } from "expect-type";

type User = {
  id: number;
  name: string;
  email: string;
};

type UserUpdate = MakeOptional<User, "email">;

expectTypeOf<UserUpdate>().toEqualTypeOf<{
  id: number;
  name: string;
  email?: string;
}>();
```

This verifies that the utility type transforms the intended properties.

# 🗁 Testing with tsd

`tsd` is a dedicated tool for testing TypeScript type definitions.

Install it:

```bash
npm install -D tsd
```

Create a declaration test file:

```typescript
// index.test-d.ts

import { expectType, expectError } from "tsd";

import { add, getUsername } from "./index";

expectType<number>(add(10, 20));

expectType<string>(getUsername(1));

expectError(add("10", 20));
```

Run the tests:

```bash
npx tsd
```

The tool checks whether the actual inferred types match the expectations.

`tsd` is particularly useful for libraries that expose public TypeScript declarations.

# 🗁 Testing Public API Contracts

Type testing helps prevent accidental changes to exported APIs.

Consider a library function:

```typescript
export interface User {
  id: number;
  name: string;
}

export function getUser(id: number): User {
  return {
    id,
    name: "Alice",
  };
}
```

A type test can verify its public contract:

```typescript
import { expectTypeOf } from "expect-type";

import { getUser, type User } from "./index";

expectTypeOf(getUser).parameter(0).toBeNumber();

expectTypeOf(getUser).returns.toEqualTypeOf<User>();

expectTypeOf(getUser(1)).toEqualTypeOf<User>();
```

If someone changes the function's return type unintentionally, the type test can detect the change.

# 🗁 Running Type Tests with npm Scripts

You can integrate type tests into your project's development workflow.

Example `package.json`:

```json
{
  "scripts": {
    "typecheck": "tsc --noEmit",
    "test:types": "tsd",
    "test": "vitest run"
  }
}
```

Run the checks:

```bash
npm run typecheck
npm run test:types
npm test
```

These commands serve different purposes:

* `typecheck` checks the project's TypeScript code.
* `test:types` verifies dedicated type assertions.
* `test` executes runtime tests.

# 🗁 Type Testing Best Practices

* Test public type contracts, not every inferred local variable.
* Verify both valid and invalid usage.
* Use `@ts-expect-error` for intentional compiler errors.
* Test generic functions with multiple input types.
* Test custom utility types and complex type transformations.
* Keep type tests separate from runtime tests when practical.
* Run type tests in CI pipelines.
* Avoid relying entirely on implementation-level types when testing public APIs.
* Keep compiler configuration consistent between development and CI.

# 🗁 Common Mistakes

**Confusing runtime assertions with type assertions**

A runtime assertion such as `expect(value).toBe("hello")` does not prove that the TypeScript type of `value` is exactly `string`.

**Using `@ts-ignore` for negative tests**

It suppresses errors without verifying that the expected error exists.

**Testing only successful assignments**

A useful type test should also verify that invalid inputs are rejected.

**Ignoring compiler configuration**

Different compiler options can change type-checking behavior. Run type tests using the intended project configuration.

**Assuming type tests replace runtime tests**

Type tests cannot verify network requests, database behavior, business logic, or other runtime behavior.

# 🗁 Key Takeaways

* Type testing verifies TypeScript's compile-time behavior.
* Runtime tests and type tests solve different problems.
* `tsc --noEmit` checks types without generating JavaScript.
* `@ts-expect-error` verifies that invalid code produces a compiler error.
* `expect-type` provides compile-time type assertions.
* `tsd` is useful for testing exported type definitions.
* Generic functions, utility types, and public APIs benefit significantly from type testing.
* Type tests help prevent accidental changes to a library's type contracts.
