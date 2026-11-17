# TypeScript Operators

Operators are symbols or keywords that perform operations on values and variables. TypeScript supports the same fundamental operators as JavaScript while adding static type checking to help detect invalid operations during development.

Understanding operators is essential for writing expressions, performing calculations, comparing values, controlling program flow, and manipulating data.

# 🗁 Arithmetic Operators

Arithmetic operators perform mathematical calculations.

| Operator | Name | Description |
| --- | --- | --- |
| `+` | Addition | Adds two values |
| `-` | Subtraction | Subtracts the right value from the left |
| `*` | Multiplication | Multiplies two values |
| `/` | Division | Divides the left value by the right |
| `%` | Remainder | Returns the remainder of division |
| `**` | Exponentiation | Raises a value to a power |
| `++` | Increment | Increases a value by one |
| `--` | Decrement | Decreases a value by one |

```typescript
let a: number = 20;
let b: number = 6;

console.log(a + b);  // 26
console.log(a - b);  // 14
console.log(a * b);  // 120
console.log(a / b);  // 3.333...
console.log(a % b);  // 2
console.log(a ** 2); // 400
```

Increment and decrement operators:

```typescript
let count: number = 5;

count++;
console.log(count); // 6

count--;
console.log(count); // 5
```

# 🗁 String Concatenation

The `+` operator can also concatenate strings.

```typescript
const firstName: string = "John";
const lastName: string = "Doe";

const fullName: string = firstName + " " + lastName;

console.log(fullName); // John Doe
```

The `+=` operator can append text to an existing string variable.

```typescript
let message: string = "Hello";

message += " TypeScript";

console.log(message); // Hello TypeScript
```

For more readable string interpolation, TypeScript also supports template literals.

```typescript
const name: string = "John";
const age: number = 25;

const message: string = `My name is ${name} and I am ${age} years old.`;
```

# 🗁 Assignment Operators

Assignment operators assign values to variables or update their existing values.

| Operator | Example | Equivalent |
| --- | --- | --- |
| `=` | `x = 10` | Assign 10 to x |
| `+=` | `x += 5` | `x = x + 5` |
| `-=` | `x -= 5` | `x = x - 5` |
| `*=` | `x *= 5` | `x = x * 5` |
| `/=` | `x /= 5` | `x = x / 5` |
| `%=` | `x %= 5` | `x = x % 5` |
| `**=` | `x **= 2` | `x = x ** 2` |

Example:

```typescript
let score: number = 100;

score += 20;
console.log(score); // 120

score -= 10;
console.log(score); // 110

score *= 2;
console.log(score); // 220

score /= 2;
console.log(score); // 110
```

# 🗁 Comparison Operators

Comparison operators compare two values and return a Boolean result (`true` or `false`).

| Operator | Name | Description |
| --- | --- | --- |
| `==` | Equality | Compares values after type coercion |
| `===` | Strict equality | Compares values and types |
| `!=` | Inequality | Compares values after type coercion |
| `!==` | Strict inequality | Compares values and types |
| `>` | Greater than | Checks whether the left value is greater |
| `<` | Less than | Checks whether the left value is smaller |
| `>=` | Greater than or equal | Checks whether the left value is greater or equal |
| `<=` | Less than or equal | Checks whether the left value is smaller or equal |

Example:

```typescript
const a: number = 10;
const b: number = 20;

console.log(a === b); // false
console.log(a !== b); // true
console.log(a > b);   // false
console.log(a < b);   // true
console.log(a >= 10); // true
console.log(b <= 20); // true
```

Prefer strict equality (`===`) and strict inequality (`!==`) in most TypeScript code.

```typescript
console.log(10 == "10");  // true
console.log(10 === "10"); // false
```

TypeScript may report an error when it can determine that the compared types are incompatible. The example above demonstrates JavaScript runtime behavior rather than a recommended TypeScript pattern.

# 🗁 Logical Operators

Logical operators combine or invert Boolean expressions.

| Operator | Name | Description |
| --- | --- | --- |
| `&&` | Logical AND | Returns true when both conditions are true |
| ` | | ` | Logical OR | Returns true when at least one condition is true |
| `!` | Logical NOT | Reverses a Boolean value |

Logical AND:

```typescript
const age: number = 25;
const hasLicense: boolean = true;

const canDrive: boolean = age >= 18 && hasLicense;

console.log(canDrive); // true
```

Logical OR:

```typescript
const isAdmin: boolean = false;
const isEditor: boolean = true;

const canManageContent: boolean = isAdmin || isEditor;

console.log(canManageContent); // true
```

Logical NOT:

```typescript
const isLoggedIn: boolean = false;

console.log(!isLoggedIn); // true
```

Logical operators use short-circuit evaluation.

With `&&`, the right operand is evaluated only when the left operand is truthy.

With `||`, the right operand is evaluated only when the left operand is falsy.

```typescript
const user: string | null = null;

const displayName = user || "Guest";

console.log(displayName); // Guest
```

# 🗁 Nullish Coalescing Operator

The nullish coalescing operator (`??`) returns the right operand only when the left operand is `null` or `undefined`.

```typescript
const username: string | null = null;

const displayName: string = username ?? "Guest";

console.log(displayName); // Guest
```

Unlike the logical OR operator, `??` does not treat `0`, `false`, or an empty string as missing values.

```typescript
const quantity: number = 0;
const result = quantity ?? 10;

console.log(result); // 0
```

Compare it with `||`:

```typescript
const quantity: number = 0;

console.log(quantity || 10); // 10
console.log(quantity ?? 10); // 0
```

Use `??` when a default value should apply only to `null` or `undefined`.

# 🗁 Ternary Operator

The ternary operator (`? :`) is a concise alternative to an `if...else` statement.

Syntax:

```typescript
condition ? expressionIfTrue : expressionIfFalse;
```

Example:

```typescript
const age: number = 20;

const status: string = age >= 18 ? "Adult" : "Minor";

console.log(status); // Adult
```

It can also be used to determine a value based on multiple conditions.

```typescript
const score: number = 85;

const grade: string =
    score >= 90 ? "A" :
    score >= 80 ? "B" :
    score >= 70 ? "C" :
    "F";

console.log(grade); // B
```

Avoid deeply nested ternary expressions because they can reduce readability.

# 🗁 Type Operators

TypeScript provides operators that help developers inspect or work with types.

**`typeof` operator**

The `typeof` operator returns the runtime type of a value as a string. It can also be used in TypeScript type positions to refer to the type of an existing variable.

Runtime example:

```typescript
const name: string = "John";
const age: number = 25;

console.log(typeof name); // string
console.log(typeof age);  // number
```

Type example:

```typescript
const user = {
    name: "John",
    age: 25
};

type User = typeof user;
```

The `User` type is derived from the type of the `user` variable.

**`instanceof` operator**

The `instanceof` operator checks whether an object belongs to a particular class or constructor's prototype chain.

```typescript
class Person {
    name: string;

    constructor(name: string) {
        this.name = name;
    }
}

const person = new Person("John");

console.log(person instanceof Person); // true
```

**`in` operator**

The `in` operator checks whether a property exists in an object or its prototype chain. It can also narrow union types.

```typescript
type Admin = {
    name: string;
    permissions: string[];
};

type Customer = {
    name: string;
    email: string;
};

function getDetails(user: Admin | Customer): string {
    if ("permissions" in user) {
        return user.permissions.join(", ");
    }

    return user.email;
}
```

When the `permissions` property exists, TypeScript narrows the value to the `Admin` type.

# 🗁 Bitwise Operators

Bitwise operators perform operations on the binary representations of numbers.

| Operator | Name | Description |
| --- | --- | --- |
| `&` | AND | Sets a bit when both corresponding bits are 1 |
| `\|` | OR | Sets a bit when either corresponding bit is 1 |
| `^` | XOR | Sets a bit when corresponding bits differ |
| `~` | NOT | Inverts the bits |
| `<<` | Left shift | Shifts bits to the left |
| `>>` | Sign-propagating right shift | Shifts bits to the right while preserving the sign |
| `>>>` | Zero-fill right shift | Shifts bits to the right and fills with zeros |

Example:

```typescript
const a: number = 5; // Binary: 0101
const b: number = 3; // Binary: 0011

console.log(a & b);  // 1
console.log(a | b);  // 7
console.log(a ^ b);  // 6
console.log(~a);     // -6
console.log(a << 1); // 10
console.log(a >> 1); // 2
```

Bitwise operations are commonly used in low-level programming, permission flags, binary protocols, and performance-sensitive operations.

JavaScript bitwise operators convert numbers to 32-bit integer representations before performing the operation. They should not be confused with arbitrary-precision arithmetic.

# 🗁 Optional Chaining Operator

The optional chaining operator (`?.`) safely accesses properties or methods when an intermediate value might be `null` or `undefined`.

Without optional chaining:

```typescript
type User = {
    profile?: {
        name?: string;
    };
};

const user: User = {};

const name = user.profile?.name;

console.log(name); // undefined
```

With optional chaining, JavaScript stops evaluating the chain and returns `undefined` when it encounters a nullish value.

Optional chaining can also be used with arrays and function calls.

```typescript
const users: string[] | undefined = undefined;

console.log(users?.[0]); // undefined
```

Optional function call:

```typescript
type Logger = {
    log?: (message: string) => void;
};

const logger: Logger = {};

logger.log?.("Application started");
```

# 🗁 Spread Operator

The spread operator (`...`) expands elements of an iterable or properties of an object into another context.

Array example:

```typescript
const numbers: number[] = [1, 2, 3];

const copiedNumbers: number[] = [...numbers];

console.log(copiedNumbers); // [1, 2, 3]
```

Combining arrays:

```typescript
const first: number[] = [1, 2];
const second: number[] = [3, 4];

const combined: number[] = [...first, ...second];

console.log(combined); // [1, 2, 3, 4]
```

Object example:

```typescript
const user = {
    name: "John",
    age: 25
};

const updatedUser = {
    ...user,
    age: 26
};

console.log(updatedUser);
```

The spread operation creates a shallow copy. Nested objects and arrays remain shared references unless they are copied separately.

# 🗁 Rest Operator

The rest operator (`...`) collects multiple elements or properties into a single variable.

Function example:

```typescript
function calculateTotal(...prices: number[]): number {
    return prices.reduce((total, price) => total + price, 0);
}

console.log(calculateTotal(10, 20, 30)); // 60
```

Array destructuring:

```typescript
const numbers: number[] = [10, 20, 30, 40];

const [first, ...remaining] = numbers;

console.log(first);     // 10
console.log(remaining); // [20, 30, 40]
```

Object destructuring:

```typescript
const user = {
    name: "John",
    age: 25,
    email: "john@example.com"
};

const { name, ...details } = user;

console.log(name);    // John
console.log(details); // { age: 25, email: "john@example.com" }
```

The spread and rest operators use the same `...` syntax, but their purposes depend on context:

* Spread expands elements or properties.
* Rest collects elements or properties.

# 🗁 Operator Precedence

Operator precedence determines the order in which operators are evaluated within an expression.

For example:

```typescript
const result: number = 10 + 5 * 2;

console.log(result); // 20
```

Multiplication has higher precedence than addition, so `5 * 2` is evaluated first.

Parentheses can explicitly control evaluation order.

```typescript
const result: number = (10 + 5) * 2;

console.log(result); // 30
```

Common precedence order, from higher to lower:

| Operators | Description |
| --- | --- |
| `()` | Grouping expressions |
| `?.`, `[]`, `.` | Property access and optional chaining |
| `!`, `~`, `typeof`, unary `+`, unary `-` | Unary operators |
| `**` | Exponentiation |
| `*`, `/`, `%` | Multiplication, division, remainder |
| `+`, `-` | Addition and subtraction |
| `<<`, `>>`, `>>>` | Bitwise shifts |
| `<`, `>`, `<=`, `>=`, `in`, `instanceof` | Relational operators |
| `===`, `!==` | Equality operators |
| `&`, `^`, `\|` | Bitwise operators |
| `&&` | Logical AND |
| `\|\|` | Logical OR |
| `??` | Nullish coalescing |
| `?:` | Conditional operator |
| `=`, `+=`, `-=`, etc. | Assignment operators |

This is a simplified reference rather than an exhaustive precedence table. When an expression becomes difficult to read, use parentheses to make the intended evaluation order explicit.

# 🗁 TypeScript-Specific Considerations

TypeScript checks many operations during compilation, but it does not change JavaScript's runtime operator behavior.

For example, TypeScript can reject incompatible operations:

```typescript
const age: number = 25;
const name: string = "John";

// TypeScript error
// const result = age - name;
```

Type assertions do not convert runtime values:

```typescript
const value: unknown = "100";

const numberValue = value as number;

// The runtime value is still a string.
console.log(typeof numberValue); // string
```

Use actual conversion when a different runtime type is required:

```typescript
const value: string = "100";

const numberValue: number = Number(value);

console.log(numberValue + 50); // 150
```

# 🗁 Best Practices

* Prefer `===` and `!==` instead of loose equality operators.
* Use `??` when providing defaults for only `null` and `undefined`.
* Use `?.` when accessing potentially missing nested properties.
* Use parentheses to make complex expressions easier to understand.
* Avoid deeply nested ternary expressions.
* Use explicit type annotations when they improve readability.
* Understand the difference between logical operators and bitwise operators.
* Remember that spread creates shallow copies, not deep copies.
* Avoid unnecessary type assertions to bypass TypeScript's type checking.
* Use appropriate conversion functions such as `Number()`, `String()`, and `Boolean()` when runtime conversion is needed.

# 🗁 Summary

TypeScript supports JavaScript operators and provides additional type-level capabilities through constructs such as `typeof` and type-aware usage of `instanceof` and `in`.

The most frequently used operator categories include:

* Arithmetic operators for calculations.
* Assignment operators for updating variables.
* Comparison operators for comparing values.
* Logical operators for combining conditions.
* Nullish coalescing and optional chaining for handling missing values.
* Ternary operators for conditional expressions.
* Spread and rest operators for working with collections and objects.
* Type operators for inspecting and narrowing types.

A solid understanding of operators makes TypeScript expressions more predictable, readable, and type-safe.
