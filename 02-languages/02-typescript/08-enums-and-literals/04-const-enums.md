# Const Enums

Const enums are a special kind of TypeScript enum declared using the `const enum` keyword.

They are designed to reduce runtime overhead by allowing TypeScript to inline enum member values during compilation instead of generating a regular enum object.

Const enums are useful when:

* You want named constants in TypeScript code.
* You want to avoid generating a runtime enum object.
* You want to reduce unnecessary JavaScript output.
* You have fixed values known at compile time.
* You want to optimize small, frequently used constants.

Example:

```typescript
const enum Direction {
  North,
  South,
  East,
  West,
}

const direction = Direction.North;

console.log(direction);
```

The compiled JavaScript is approximately:

```javascript
const direction = 0;

console.log(direction);
```

Notice that the `Direction` enum object is not generated. Its value is substituted directly into the compiled code.

# 🗁 Basic Syntax

Use the `const enum` keywords to declare a const enum.

```typescript
const enum EnumName {
  MemberOne,
  MemberTwo,
  MemberThree,
}
```

Example:

```typescript
const enum Status {
  Pending,
  Approved,
  Rejected,
}
```

Like regular numeric enums, values start at zero and increment automatically unless explicit values are provided.

# 🗁 Numeric Const Enums

Const enums can contain numeric members.

```typescript
const enum Priority {
  Low = 1,
  Medium = 2,
  High = 3,
  Critical = 4,
}

const priority = Priority.High;

console.log(priority);
```

Compiled JavaScript:

```javascript
const priority = 3;

console.log(priority);
```

# 🗁 String Const Enums

Const enums can also contain string values.

```typescript
const enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}

const currentStatus = Status.Approved;

console.log(currentStatus);
```

Compiled JavaScript:

```javascript
const currentStatus = "approved";

console.log(currentStatus);
```

String const enums are useful when you want readable constant values without generating an enum object.

# 🗁 Explicit Values

You can assign explicit values to const enum members.

```typescript
const enum HttpStatus {
  Ok = 200,
  Created = 201,
  BadRequest = 400,
  Unauthorized = 401,
  NotFound = 404,
  InternalServerError = 500,
}

const responseStatus = HttpStatus.NotFound;

console.log(responseStatus);
```

Output:

```text
404
```

Explicit values are useful when working with HTTP status codes, application codes, and external numeric contracts.

# 🗁 Automatic Incrementing

Numeric const enums support automatic value assignment.

```typescript
const enum Status {
  Pending = 10,
  Processing,
  Completed,
  Cancelled,
}
```

Values:

| Member | Value |
| --- | ---: |
| Pending | 10 |
| Processing | 11 |
| Completed | 12 |
| Cancelled | 13 |

# 🗁 Const Enum Member Access

Const enum members can be accessed using dot notation.

```typescript
const enum Direction {
  North,
  South,
  East,
  West,
}

const direction = Direction.North;
const opposite = Direction.South;

console.log(direction);
console.log(opposite);
```

Each reference is replaced with its corresponding value during compilation.

# 🗁 Const Enums with Variables

Const enum values can be assigned to variables with the enum type.

```typescript
const enum UserRole {
  Admin,
  Editor,
  Viewer,
}

const role: UserRole = UserRole.Admin;

console.log(role);
```

The TypeScript compiler uses the enum member value when generating JavaScript.

# 🗁 Const Enums with Functions

Const enums can be used in function parameters and return types.

```typescript
const enum Status {
  Pending,
  Approved,
  Rejected,
}

function getStatusMessage(status: Status): string {
  switch (status) {
    case Status.Pending:
      return "Waiting for approval";

    case Status.Approved:
      return "Request approved";

    case Status.Rejected:
      return "Request rejected";
  }
}

console.log(getStatusMessage(Status.Approved));
```

The enum member references are inlined during compilation.

# 🗁 Const Enums with Objects

Const enums can be used as object property types.

```typescript
const enum AccountStatus {
  Active,
  Suspended,
  Closed,
}

type Account = {
  id: number;
  username: string;
  status: AccountStatus;
};

const account: Account = {
  id: 101,
  username: "alice",
  status: AccountStatus.Active,
};
```

The enum provides compile-time type information while its member values are substituted into the emitted JavaScript.

# 🗁 Const Enums with Arrays

Const enum members can be stored in arrays.

```typescript
const enum Permission {
  Read = 1,
  Write = 2,
  Update = 3,
  Delete = 4,
}

const permissions: Permission[] = [
  Permission.Read,
  Permission.Write,
  Permission.Update,
];

console.log(permissions);
```

Compiled JavaScript contains the actual values rather than references to a runtime enum object.

# 🗁 Const Enums with Interfaces

Interfaces can use const enum types in their property declarations.

```typescript
const enum OrderStatus {
  Pending,
  Processing,
  Completed,
  Cancelled,
}

interface Order {
  id: number;
  total: number;
  status: OrderStatus;
}

const order: Order = {
  id: 1001,
  total: 2500,
  status: OrderStatus.Processing,
};
```

Interfaces are erased during compilation, and const enum values are inlined where referenced.

# 🗁 Const Enums and Computed Values

Const enum members must have constant enum expressions.

Supported expressions include numeric literals, references to earlier constant enum members, and supported arithmetic or bitwise operations.

```typescript
const enum Configuration {
  Timeout = 1000 * 30,
  MaxRetries = 3,
  RetryDelay = 1000 * 5,
}

const timeout = Configuration.Timeout;

console.log(timeout);
```

Compiled JavaScript:

```javascript
const timeout = 30000;

console.log(timeout);
```

A const enum cannot use arbitrary runtime expressions.

For example, this is invalid:

```typescript
function getValue(): number {
  return 10;
}

// Invalid const enum member initializer
// const enum Example {
//   Value = getValue(),
// }
```

The value must be computable by the compiler.

# 🗁 Const Enums and Runtime Objects

Regular enums generate JavaScript objects.

```typescript
enum Direction {
  North,
  South,
  East,
  West,
}

console.log(Direction.North);
```

Conceptually, the compiled JavaScript includes an object containing the enum members and reverse mappings.

Const enums behave differently:

```typescript
const enum Direction {
  North,
  South,
  East,
  West,
}

console.log(Direction.North);
```

Compiled JavaScript:

```javascript
console.log(0);
```

There is no generated `Direction` object in the ordinary inlining behavior.

# 🗁 Const Enums and Reverse Mapping

Const enums do not provide runtime reverse mapping.

```typescript
const enum Status {
  Pending = 0,
  Approved = 1,
  Rejected = 2,
}

console.log(Status.Approved); // 1

// Not available as a runtime enum object:
// console.log(Status[1]);
```

Because the enum object is not generated, runtime reverse lookup is unavailable.

If runtime enumeration or reverse mapping is required, use a regular enum or a separate object.

# 🗁 Const Enums and Object.values()

You cannot use `Object.values()` on a const enum because it does not exist as a runtime object.

```typescript
const enum Direction {
  North,
  South,
  East,
  West,
}

// Invalid: Direction is not available as a runtime value.
// console.log(Object.values(Direction));
```

If you need runtime values, define a separate array:

```typescript
const directions = [
  "north",
  "south",
  "east",
  "west",
] as const;

type Direction = (typeof directions)[number];

console.log(directions);
```

This approach provides both runtime data and a compile-time union type.

# 🗁 Const Enums and Compiler Configuration

The `preserveConstEnums` compiler option controls how const enums are emitted.

In a typical TypeScript configuration:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "preserveConstEnums": false
  }
}
```

With `preserveConstEnums` disabled, const enum declarations are normally removed from emitted JavaScript, and their references are inlined.

When enabled, TypeScript emits enum declarations while still inlining references in the compilation output.

This option does not make const enums behave exactly like regular enums in every context.

# 🗁 Const Enums and Isolated Modules

The `isolatedModules` compiler option requires each file to be compilable independently without relying on cross-file type information.

Ambient const enum references are problematic in isolated compilation workflows because another file's enum values may not be available for inlining.

For example, a project using tools such as Babel or other single-file transpilers may not support all TypeScript const enum behavior.

For this reason, const enums require particular care in projects using:

* Babel TypeScript transformation.
* Single-file transpilation.
* `isolatedModules`.
* Declaration files from external packages.
* Multiple independent build tools.

Check the compiler and transpiler documentation before introducing const enums into such projects.

# 🗁 Ambient Const Enums

An ambient const enum declares a const enum whose implementation is provided externally.

It is declared using `declare const enum`.

```typescript
declare const enum Environment {
  Development = "development",
  Production = "production",
}

const currentEnvironment = Environment.Development;
```

Ambient declarations do not generate their own implementation.

The compiler assumes that the referenced values are provided by the external declaration contract.

Ambient const enums can create compatibility problems when different versions of declarations and runtime implementations are used.

# 🗁 Const Enum Pitfalls

Const enums have several important limitations.

**No runtime enum object**

You cannot inspect the enum through `Object.keys()`, `Object.values()`, or runtime property access.

**No reverse mapping**

Numeric const enums do not generate reverse mappings.

**Potential version mismatch**

When enum values are inlined from an external package, compiled code can retain old values even after the dependency changes.

For example, if a library changes a member's numeric value, an application compiled against the old declaration may still contain the previous inlined value.

**Transpiler compatibility**

Not every TypeScript-compatible transpiler supports const enum inlining.

**Ambient declaration problems**

Publishing ambient const enums in declaration files can introduce issues for consumers using isolated compilation or different dependency versions.

# 🗁 Const Enums vs Regular Enums

| Feature | Regular Enum | Const Enum |
| --- | --- | --- |
| Declaration | `enum` | `const enum` |
| Runtime object | Yes | Normally no |
| Member inlining | No | Yes |
| Reverse mapping for numeric members | Yes | No |
| Runtime enumeration | Yes | No |
| Runtime inspection | Yes | No |
| Compiler optimization | Regular enum object | Avoids enum object generation |
| Transpiler compatibility | Generally broader | Requires more care |

# 🗁 Const Enums vs Const Objects

A const object is another way to define named constants.

Const enum:

```typescript
const enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}

const status = Status.Pending;
```

Const object:

```typescript
const Status = {
  Pending: "pending",
  Approved: "approved",
  Rejected: "rejected",
} as const;

type Status = (typeof Status)[keyof typeof Status];

const status: Status = Status.Pending;
```

Comparison:

| Feature | Const Enum | Const Object |
| --- | --- | --- |
| Runtime object | Normally no | Yes |
| Member access | Yes | Yes |
| Runtime enumeration | No | Yes |
| Reverse lookup | No | Can be implemented |
| Union type generation | Enum type | `typeof` and `keyof` |
| External runtime compatibility | Requires care | Generally straightforward |

Const objects are often a practical alternative when runtime access and compile-time type safety are both needed.

# 🗁 Best Practices

* Use const enums only when their compilation behavior is understood.
* Prefer regular enums when runtime enumeration is required.
* Avoid publishing ambient const enums in public libraries unless compatibility is carefully managed.
* Check compatibility with Babel, isolated modules, and other build tools.
* Use explicit values when working with external contracts.
* Consider const objects with `as const` when runtime access is necessary.
* Do not assume const enums generate JavaScript objects.
* Understand `preserveConstEnums` before changing compiler behavior.
* Avoid unnecessary optimization when a regular enum is simpler and more maintainable.

# 🗁 Summary

* Const enums are declared using `const enum`.
* They allow enum member values to be inlined during compilation.
* They normally do not generate runtime enum objects.
* They support numeric and string members.
* They do not provide runtime reverse mapping.
* They cannot be inspected using `Object.values()` or `Object.keys()`.
* Their members must have compile-time constant values.
* `preserveConstEnums` affects JavaScript emission.
* Ambient const enums and isolated compilation require special care.
* Const objects with `as const` are an alternative when runtime access is required.

# 🗁 Related Topics

* Enums
* Numeric Enums
* String Enums
* Literal Types
* Template Literal Types
* As Const
* Type Inference
* Type Assertions
* TypeScript Compiler Options
* Isolated Modules
* Declaration Files
