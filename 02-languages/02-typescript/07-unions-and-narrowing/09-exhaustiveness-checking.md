# Exhaustiveness Checking

Exhaustiveness checking is a TypeScript technique used to ensure that all possible members of a union type are handled in conditional logic.

It is particularly useful when working with:

* Union types.
* Discriminated unions.
* Switch statements.
* Application states.
* API response types.
* Event handling.
* State machines.

TypeScript provides the `never` type to help detect missing cases at compile time.

Without exhaustiveness checking, a function may silently fail to handle a newly added union member. With exhaustive checks, TypeScript can identify incomplete logic before the application runs.

# 🗁 What Is Exhaustiveness?

A conditional statement is exhaustive when it handles every possible value of the type being checked.

Consider a union type:

```typescript
type Direction = "north" | "south" | "east" | "west";
```

A function that handles all four directions is exhaustive:

```typescript
function move(direction: Direction): void {
  switch (direction) {
    case "north":
      console.log("Moving north");
      break;

    case "south":
      console.log("Moving south");
      break;

    case "east":
      console.log("Moving east");
      break;

    case "west":
      console.log("Moving west");
      break;
  }
}
```

Every possible member of `Direction` has been handled.

However, TypeScript does not necessarily report a missing case just because a `switch` statement does not contain a `default` branch.

Exhaustiveness checking makes the compiler verify that all members are covered.

# 🗁 Understanding the never Type

The `never` type represents values that should never occur.

It is used when:

* A function never returns normally.
* An expression cannot have any possible value.
* All possible members of a union have been eliminated.

Example:

```typescript
function throwError(message: string): never {
  throw new Error(message);
}
```

This function has the return type `never` because it always throws an exception.

Another example:

```typescript
function infiniteLoop(): never {
  while (true) {
    // This function never finishes normally.
  }
}
```

The `never` type is especially useful for checking whether a union has been completely handled.

# 🗁 Basic Exhaustiveness Checking

Consider a union of application statuses:

```typescript
type Status =
  | "pending"
  | "approved"
  | "rejected";
```

Create a helper function:

```typescript
function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}
```

Use it in a switch statement:

```typescript
function handleStatus(status: Status): string {
  switch (status) {
    case "pending":
      return "Waiting for approval";

    case "approved":
      return "Request approved";

    case "rejected":
      return "Request rejected";

    default:
      return assertNever(status);
  }
}
```

Because every possible status has been handled, TypeScript narrows `status` to `never` inside the `default` branch.

The call to `assertNever(status)` is therefore valid.

# 🗁 Detecting Missing Cases

Now add another member to the union:

```typescript
type Status =
  | "pending"
  | "approved"
  | "rejected"
  | "cancelled";
```

The existing function has not been updated:

```typescript
function handleStatus(status: Status): string {
  switch (status) {
    case "pending":
      return "Waiting for approval";

    case "approved":
      return "Request approved";

    case "rejected":
      return "Request rejected";

    default:
      return assertNever(status);
  }
}
```

TypeScript reports a compilation error because `status` is no longer `never` in the `default` branch.

It can still be `"cancelled"`.

Fix the problem by adding the missing case:

```typescript
function handleStatus(status: Status): string {
  switch (status) {
    case "pending":
      return "Waiting for approval";

    case "approved":
      return "Request approved";

    case "rejected":
      return "Request rejected";

    case "cancelled":
      return "Request cancelled";

    default:
      return assertNever(status);
  }
}
```

Now every possible status is handled again.

# 🗁 Exhaustiveness Checking with Discriminated Unions

Exhaustiveness checking works particularly well with discriminated unions.

Consider a payment system:

```typescript
type CashPayment = {
  method: "cash";
  amount: number;
};

type CardPayment = {
  method: "card";
  amount: number;
  cardNumber: string;
};

type BankTransferPayment = {
  method: "bank-transfer";
  amount: number;
  accountNumber: string;
};

type Payment =
  | CashPayment
  | CardPayment
  | BankTransferPayment;
```

Create a payment handler:

```typescript
function processPayment(payment: Payment): void {
  switch (payment.method) {
    case "cash":
      console.log("Cash payment:", payment.amount);
      break;

    case "card":
      console.log("Card payment:", payment.amount);
      console.log("Card number:", payment.cardNumber);
      break;

    case "bank-transfer":
      console.log("Bank transfer:", payment.amount);
      console.log("Account:", payment.accountNumber);
      break;

    default:
      assertNever(payment);
  }
}
```

The `method` property identifies each variant.

After all three cases are handled, TypeScript considers `payment` to be `never` in the `default` branch.

# 🗁 Creating a Reusable assertNever Function

The `assertNever` function can be reused throughout an application.

```typescript
export function assertNever(
  value: never,
  message = "Unexpected value",
): never {
  throw new Error(`${message}: ${String(value)}`);
}
```

Import it wherever exhaustive checking is needed:

```typescript
import { assertNever } from "./assert-never";

type Result =
  | { status: "success"; data: string }
  | { status: "error"; message: string }
  | { status: "loading" };

function handleResult(result: Result): string {
  switch (result.status) {
    case "success":
      return result.data;

    case "error":
      return result.message;

    case "loading":
      return "Loading...";

    default:
      return assertNever(result, "Unhandled result");
  }
}
```

This keeps the exhaustive-checking logic consistent across the project.

# 🗁 Exhaustiveness Checking with if and else

Exhaustiveness checking is not limited to switch statements.

You can use conditional statements to narrow union types.

```typescript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; side: number }
  | { kind: "rectangle"; width: number; height: number };
```

Example:

```typescript
function calculateArea(shape: Shape): number {
  if (shape.kind === "circle") {
    return Math.PI * shape.radius ** 2;
  }

  if (shape.kind === "square") {
    return shape.side ** 2;
  }

  if (shape.kind === "rectangle") {
    return shape.width * shape.height;
  }

  return assertNever(shape);
}
```

After all possible variants have been handled, `shape` is narrowed to `never`.

If a new shape is added without updating the function, TypeScript reports an error.

# 🗁 Exhaustiveness Checking with Boolean Unions

Exhaustiveness checking also works with boolean literal unions.

```typescript
type ToggleState = true | false;
```

Example:

```typescript
function describeToggle(state: ToggleState): string {
  switch (state) {
    case true:
      return "Enabled";

    case false:
      return "Disabled";

    default:
      return assertNever(state);
  }
}
```

Both possible values are handled.

Although this example is simple, the same principle applies to more complex literal unions.

# 🗁 Exhaustiveness Checking with Enum

Enums can also be checked exhaustively.

```typescript
enum OrderStatus {
  Pending = "pending",
  Processing = "processing",
  Shipped = "shipped",
  Delivered = "delivered",
}
```

Create a handler:

```typescript
function getOrderMessage(status: OrderStatus): string {
  switch (status) {
    case OrderStatus.Pending:
      return "Order is waiting";

    case OrderStatus.Processing:
      return "Order is being processed";

    case OrderStatus.Shipped:
      return "Order has been shipped";

    case OrderStatus.Delivered:
      return "Order has been delivered";

    default:
      return assertNever(status);
  }
}
```

If another enum member is added, the compiler can identify the missing case when the function is checked against the updated enum.

# 🗁 Exhaustiveness Checking with API Responses

API responses often contain multiple possible states.

```typescript
type ApiSuccess<T> = {
  status: "success";
  data: T;
};

type ApiValidationError = {
  status: "validation-error";
  errors: string[];
};

type ApiServerError = {
  status: "server-error";
  message: string;
};

type ApiResponse<T> =
  | ApiSuccess<T>
  | ApiValidationError
  | ApiServerError;
```

Create a handler:

```typescript
type User = {
  id: number;
  name: string;
};

function handleApiResponse(
  response: ApiResponse<User>,
): string {
  switch (response.status) {
    case "success":
      return `Welcome ${response.data.name}`;

    case "validation-error":
      return response.errors.join(", ");

    case "server-error":
      return response.message;

    default:
      return assertNever(response);
  }
}
```

If the API response model changes, the compiler helps identify every handler that needs updating.

# 🗁 Exhaustiveness Checking with State Machines

State machines represent an application as a collection of explicitly defined states.

Discriminated unions are useful for modeling these states.

```typescript
type IdleState = {
  status: "idle";
};

type LoadingState = {
  status: "loading";
};

type SuccessState = {
  status: "success";
  data: string[];
};

type ErrorState = {
  status: "error";
  message: string;
};

type AppState =
  | IdleState
  | LoadingState
  | SuccessState
  | ErrorState;
```

Create a renderer:

```typescript
function renderState(state: AppState): string {
  switch (state.status) {
    case "idle":
      return "Ready";

    case "loading":
      return "Loading data";

    case "success":
      return `Loaded ${state.data.length} items`;

    case "error":
      return `Error: ${state.message}`;

    default:
      return assertNever(state);
  }
}
```

Every state has an explicit rendering behavior.

This is particularly useful in frontend applications where components must respond to loading, success, and error states.

# 🗁 Exhaustiveness Checking Without a Default Branch

You can also rely on TypeScript's control-flow analysis and explicit return types without using an `assertNever` helper.

Example:

```typescript
type Status = "pending" | "approved" | "rejected";

function getStatusMessage(status: Status): string {
  switch (status) {
    case "pending":
      return "Waiting";

    case "approved":
      return "Approved";

    case "rejected":
      return "Rejected";
  }
}
```

With an explicit return type, TypeScript reports an error because the function may reach its end without returning a string.

If another status is added and not handled, the same problem remains.

However, this approach does not provide the same explicit compile-time check at the missing branch as `assertNever`.

Using `assertNever` makes the intention clearer and identifies the unhandled value in the error.

# 🗁 The Role of strict Compiler Options

Compiler options can improve the reliability of exhaustive logic.

A useful configuration is:

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true
  }
}
```

Important options:

| Option | Purpose |
| --- | --- |
| `strict` | Enables strict type-checking options |
| `noImplicitReturns` | Reports functions where not all code paths return a value |
| `noFallthroughCasesInSwitch` | Prevents accidental fallthrough between switch cases |

These options complement exhaustive checking but do not replace explicit `never` checks.

# 🗁 Common Mistakes

**Forgetting the assertNever call**

```typescript
type Status = "pending" | "approved" | "rejected";

function handleStatus(status: Status): void {
  switch (status) {
    case "pending":
      console.log("Pending");
      break;

    case "approved":
      console.log("Approved");
      break;
  }
}
```

The function does not explicitly verify that every member is handled.

Add a `default` branch:

```typescript
default:
  return assertNever(status);
```

**Using any instead of never**

```typescript
function assertNever(value: any): never {
  throw new Error("Unexpected value");
}
```

Using `any` removes the compile-time protection.

The function should accept `never`:

```typescript
function assertNever(value: never): never {
  throw new Error(`Unexpected value: ${value}`);
}
```

**Using a default branch that silently ignores unknown values**

```typescript
function handleStatus(status: Status): void {
  switch (status) {
    case "pending":
      console.log("Pending");
      break;

    case "approved":
      console.log("Approved");
      break;

    default:
      break;
  }
}
```

This does not provide exhaustive checking.

A newly added union member can silently reach the default branch without being handled.

**Assuming every runtime value is valid**

TypeScript checks types at compile time. External data can still contain unexpected values.

For example, data received from an API may not match the declared union.

Validate external input at runtime before treating it as a trusted union value.

**Forgetting to update related logic**

A union type may be used in several functions and components.

When adding a new member, update every location that handles the union. Exhaustive checks help locate these locations.

# 🗁 Best Practices

* Use discriminated unions to model related variants.
* Use `never` to verify that all union members are handled.
* Create a reusable `assertNever` helper.
* Prefer exhaustive `switch` statements for unions with multiple variants.
* Use explicit return types for functions that process unions.
* Enable `strict` and `noImplicitReturns`.
* Avoid using `any` in exhaustive-checking helpers.
* Do not silently ignore unhandled union members.
* Validate external data at runtime.
* Add new variants to every relevant handler when updating a union.

# 🗁 Summary

* Exhaustiveness checking ensures that every possible union member is handled.
* The `never` type represents values that cannot occur.
* TypeScript narrows a fully handled union to `never`.
* An `assertNever` function provides explicit compile-time verification.
* Exhaustiveness checking works with discriminated unions, literal unions, and enums.
* It is useful for API responses, application states, events, and state machines.
* Adding a new union member can reveal missing cases during compilation.
* Compiler options such as `strict` and `noImplicitReturns` provide additional protection.
* Exhaustive handling makes applications easier to maintain and refactor.

# 🗁 Related Topics

* Never Type
* Union Types
* Type Narrowing
* Type Guards
* Type Predicates
* Discriminated Unions
* Switch Statements
* Literal Types
* Enums
* Control Flow Analysis
* Strict Compiler Options
* State Machines
