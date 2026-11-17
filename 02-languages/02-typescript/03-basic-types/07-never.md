# TypeScript Never Type

The `never` type represents values that never occur.

It is used when a function never successfully returns, when execution cannot reach a particular point, or when a value is impossible according to the type system.

Unlike `void`, which represents a function that returns without providing a meaningful value, `never` represents a function that does not complete normally.

The `never` type is particularly useful for:

* Functions that always throw errors.
* Functions that never terminate.
* Exhaustiveness checking in discriminated unions.
* Impossible states in type systems.
* Advanced conditional and generic types.

# 🗁 Understanding Never

The `never` type represents a value that cannot exist.

```typescript
let value: never;
```

You cannot assign ordinary values to a variable of type `never`.

```typescript
let value: never;

// TypeScript errors:
// value = "Hello";
// value = 100;
// value = true;
// value = null;
// value = undefined;
```

A `never` variable can only receive another value that is itself of type `never`.

# 🗁 Never vs Void

Both `never` and `void` are used in function return types, but their meanings are different.

* `void` means the function completes without returning a meaningful value.
* `never` means the function never completes normally.

**Using void:**

```typescript
function logMessage(): void {
    console.log("Application started");
}

logMessage();
```

The function executes and finishes normally.

**Using never:**

```typescript
function throwError(): never {
    throw new Error("Something went wrong");
}
```

The function throws an exception and never reaches a normal return.

Comparison:

| Feature | `void` | `never` |
| --- | --- | --- |
| Meaning | No meaningful return value | No normal completion |
| Function completes normally | Yes | No |
| Can return without a value | Yes | No |
| Can throw an error | Yes | Yes |
| Can contain an infinite loop | Yes | Yes |
| Common usage | Logging and side effects | Errors and unreachable execution |

# 🗁 Functions That Always Throw Errors

A function that always throws an exception can have a `never` return type.

```typescript
function fail(message: string): never {
    throw new Error(message);
}

fail("Something went wrong");
```

Because the function always throws an exception, it never returns normally.

Another example:

```typescript
function getUser(): never {
    throw new Error("User not found");
}
```

This return type communicates that successful execution never produces a return value.

# 🗁 Functions with Infinite Loops

A function containing an infinite loop also has a `never` return type when it cannot terminate normally.

```typescript
function infiniteLoop(): never {
    while (true) {
        console.log("Running...");
    }
}
```

The function never reaches its end.

A more practical example might be a process that continuously monitors a queue:

```typescript
function monitorQueue(): never {
    while (true) {
        // Continuously monitor incoming tasks.
    }
}
```

The function has no normal completion path.

# 🗁 Unreachable Code

Unreachable code is code that cannot execute because the program has already entered a path that cannot continue normally.

```typescript
function processValue(value: string | number): void {
    if (typeof value === "string") {
        console.log(value.toUpperCase());
        return;
    }

    if (typeof value === "number") {
        console.log(value.toFixed(2));
        return;
    }

    // No remaining possible value.
}
```

After both conditions are handled, there are no remaining possible types for `value`.

TypeScript can use `never` to represent this impossible state.

# 🗁 Exhaustiveness Checking

Exhaustiveness checking ensures that every possible case of a union type is handled.

The `never` type is commonly used to detect missing cases in `switch` statements.

Consider a union type:

```typescript
type Status = "pending" | "success" | "error";
```

Create a function that handles every status:

```typescript
function handleStatus(status: Status): string {
    switch (status) {
        case "pending":
            return "Request is pending";

        case "success":
            return "Request completed successfully";

        case "error":
            return "Request failed";

        default:
            const exhaustiveCheck: never = status;
            return exhaustiveCheck;
    }
}
```

Once all possible cases are handled, TypeScript knows that `status` has the type `never` inside the `default` branch.

If a new status is added:

```typescript
type Status =
    | "pending"
    | "success"
    | "error"
    | "cancelled";
```

The `default` branch now produces a TypeScript error because `status` could be `"cancelled"` rather than `never`.

This forces you to handle the new case.

# 🗁 Reusable Exhaustiveness Helper

You can create a reusable helper function for exhaustive checking.

```typescript
function assertNever(value: never): never {
    throw new Error(`Unexpected value: ${value}`);
}
```

Use it in a `switch` statement:

```typescript
type Role = "admin" | "editor" | "viewer";

function getPermissions(role: Role): string {
    switch (role) {
        case "admin":
            return "Full access";

        case "editor":
            return "Edit access";

        case "viewer":
            return "Read-only access";

        default:
            return assertNever(role);
    }
}
```

If a new role is added without updating the switch statement, TypeScript reports an error.

This pattern is useful in large applications where union types represent application states, user roles, events, or API response categories.

# 🗁 Never with Discriminated Unions

Discriminated unions use a common property to distinguish between multiple object types.

The `never` type can help ensure every variant is handled.

```typescript
type Payment =
    | {
        type: "cash";
        amount: number;
    }
    | {
        type: "card";
        amount: number;
        cardNumber: string;
    }
    | {
        type: "bank";
        amount: number;
        accountNumber: string;
    };
```

Create a function that processes every payment type:

```typescript
function processPayment(payment: Payment): string {
    switch (payment.type) {
        case "cash":
            return `Cash payment: ${payment.amount}`;

        case "card":
            return `Card payment: ${payment.amount}`;

        case "bank":
            return `Bank payment: ${payment.amount}`;

        default:
            return assertNever(payment);
    }
}
```

If another payment type is introduced, the compiler identifies the missing implementation.

# 🗁 Never in Conditional Types

The `never` type has special behavior in conditional types.

Consider:

```typescript
type IsString<T> = T extends string ? true : false;

type Result1 = IsString<string>;
type Result2 = IsString<number>;
```

The resulting types are:

```typescript
type Result1 = true;
type Result2 = false;
```

Conditional types can also use `never` to exclude types.

```typescript
type RemoveString<T> = T extends string ? never : T;

type Result = RemoveString<string | number | boolean>;
```

The resulting type is:

```typescript
type Result = number | boolean;
```

The string type is removed because its conditional result is `never`.

This behavior is the foundation of utility types such as `Exclude`.

# 🗁 Never and Union Types

The `never` type is the bottom type in TypeScript's type system.

It represents a type with no possible values.

When combined with a union, `never` has no effect.

```typescript
type Result = string | never;
```

The resulting type is equivalent to:

```typescript
type Result = string;
```

Another example:

```typescript
type Result = number | string | never;
```

The resulting type is:

```typescript
type Result = number | string;
```

This behavior is important when building conditional and generic types.

# 🗁 Never and Intersection Types

When incompatible types are combined using an intersection, the result can become `never`.

```typescript
type Result = string & number;
```

No value can simultaneously satisfy both `string` and `number`, so the resulting type is `never`.

Another example:

```typescript
type User = {
    id: string;
};

type Admin = {
    id: number;
};

type Combined = User & Admin;
```

The `id` property must satisfy both `string` and `number`, which is impossible.

Consequently, the resulting intersection cannot represent a valid object.

# 🗁 Never and Array Types

An array of `never` cannot contain ordinary values.

```typescript
const values: never[] = [];
```

The empty array is valid because it contains no elements.

However, adding ordinary values is not allowed.

```typescript
const values: never[] = [];

// TypeScript errors:
// values.push("Hello");
// values.push(100);
// values.push(true);
```

Arrays of `never` are sometimes useful in advanced type-level programming and when representing collections that must remain empty.

# 🗁 Never and Function Assignability

A function returning `never` can be assigned to a function type that returns another type.

```typescript
type StringFunction = () => string;

const fail: StringFunction = () => {
    throw new Error("Operation failed");
};
```

The function never returns a value, so it cannot violate the expected return type by returning an incompatible value.

The reverse assignment is not generally valid.

```typescript
type NeverFunction = () => never;

const getMessage = (): string => {
    return "Hello";
};

// TypeScript error:
// const fail: NeverFunction = getMessage;
```

A function returning a string can complete normally, so it does not satisfy a function type that promises never to return normally.

# 🗁 Never in Generic Types

The `never` type can be used in generic types to represent impossible values or remove unwanted types.

```typescript
type NonString<T> = T extends string ? never : T;

type Values = NonString<string | number | boolean>;
```

The resulting type is:

```typescript
type Values = number | boolean;
```

Another example uses `never` to represent a type that should not accept any values.

```typescript
type EmptyObject = Record<string, never>;

const empty: EmptyObject = {};

// TypeScript error:
// const invalid: EmptyObject = {
//     name: "John"
// };
```

This pattern can be useful when defining objects that must not contain additional properties.

# 🗁 Never and Error Handling

The `never` type is useful for functions that terminate execution by throwing an error.

```typescript
function throwApplicationError(message: string): never {
    throw new Error(message);
}

function getUsername(username: string | null): string {
    if (username === null) {
        throwApplicationError("Username is required");
    }

    return username.toUpperCase();
}
```

After the error function is called, execution cannot continue normally along that path.

TypeScript therefore understands that `username` must be a string at the return statement.

# 🗁 Best Practices

* Use `never` for functions that never complete normally.
* Use `never` for functions that always throw exceptions.
* Use exhaustive checking in `switch` statements.
* Create reusable `assertNever()` helpers for discriminated unions.
* Use `never` in conditional types to exclude unwanted types.
* Understand how `never` behaves in union and intersection types.
* Avoid using `never` as a replacement for `void`.
* Use `never` to make impossible states explicit in advanced type definitions.

# 🗁 Summary

The `never` type represents values that cannot exist and execution paths that cannot complete normally.

Important concepts include:

* Declaring variables with `never`.
* Understanding the difference between `never` and `void`.
* Functions that always throw errors.
* Functions containing infinite loops.
* Unreachable code.
* Exhaustiveness checking.
* Discriminated unions.
* Reusable `assertNever()` helpers.
* Conditional and generic types.
* Union and intersection behavior.
* Function assignability.

The key principle is that `never` represents something that cannot happen. It is particularly valuable for making TypeScript applications more predictable and ensuring that all possible cases are handled.
